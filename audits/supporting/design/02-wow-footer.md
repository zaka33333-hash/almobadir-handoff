# Wave 2 — Footer (.ftr3) WOW Moves

**Authored:** 2026-04-26
**Section:** `.ftr3` — CTA banner + 5-col sitemap + base bar
**Source:** `website/index.html` lines 1109–1202 · `website/assets/v3.css` lines 876–965 · `website/assets/v3.js` lines 447–502
**Reference packs:** `wow-research-{interaction, motion, typography, arabic}.md` (Wave 1)
**Bar:** billion-dollar website (Stripe / Linear / Apple / NYT / Bloomberg / GitHub / Rauno).

---

## Executive Summary

The footer is the most operationally complete region on almobadir.com — it already does the boring work (sitemap, CTA, base bar, lang toggle, back-to-top, live clock, brand mark). What it lacks is *signature*: a single moment where a serious editorial Saudi business brand asserts itself instead of mimicking Stripe. The audit (`02-footer.md`) identified this footer as one fix-pass away from GitHub-tier; what it needs from this Wave is two or three high-trust gestures that earn the right to sit at the bottom of a 14,809-px page and be the **last** thing the reader experiences.

The 7 moves below split into three classes. **Class A — Identity statements** (1–3): make the wordmark, the timestamp, and the network handles feel like a finance institution rather than an SDK splash page. The debossed-on-paper wordmark is the bold move — it would mean inverting the footer's surface from `--void` to `--paper`, paid for by deleting the 60-px CTA banner the audit already flagged for removal. **Class B — Earned moments** (4–5): hover-revealed wordmark construction and the "mubadir" type-the-phrase easter egg from REF 12. Both are low-effort, high-taste; both require zero copy or backend wiring. **Class C — Functional polish** (6–7): a wormhole back-to-top that respects the 14,809-px scroll, and a clock pill that pulses *correctly* — once per second, in tabular-numerals, with a subtle scale-tick that matches the second hand instead of free-running.

The ranked top three (deboss wordmark, wordmark construction reveal, "mubadir" easter egg) ship in ~12 hours combined and lift the footer from "competent" to "the last thing they remember." Risk lives in MOVE 1 (cream surface inversion fights the rest of the dark site); ship it as a sub-region instead of swapping the whole footer if that risk reads too high.

---

## MOVE 1: Debossed wordmark on cream paper sub-region

**The moment**: The footer's bottom third inverts to cream paper, and the giant `المبادر` wordmark is **pressed into the paper** — visible only as a soft inner-shadow letterform you can almost feel under your thumbnail. It is the single most editorial moment on the entire site. Print-shop authority.

**Where**: Replace `<section class="ftr3-cta">` (`index.html:1111`) with the brand sub-region; the wordmark moves out of `.ftr3-col--brand` (currently lines 1126–1137) into a dedicated cream block above the sitemap. The dark sitemap + base bar stay as-is.

**Implementation**:
```css
/* New cream sub-region — replaces the deleted ftr3-cta banner. */
.ftr3-press {
  background: var(--paper);          /* #F8F4EA */
  color: var(--void);
  padding: clamp(72px, 9vw, 140px) clamp(20px, 4vw, 56px) clamp(56px, 7vw, 112px);
  border-block: 1px solid rgba(10,15,30,.12);
  position: relative;
  isolation: isolate;
}

.ftr3-press__inner {
  max-width: 1320px; margin-inline: auto;
  display: grid; gap: 28px;
}

/* The deboss is a multi-shadow inset trick on transparent text.
   Two inner shadows (light from top-left, dark from bottom-right)
   simulate the pressed-into-paper letterpress effect.            */
.ftr3-press__mark {
  font-family: var(--font-ar-display);
  font-weight: 900;
  font-size: clamp(96px, 18vw, 280px);
  line-height: 0.9;
  letter-spacing: -0.025em;
  margin: 0;
  text-align: center;
  color: transparent;
  /* The deboss: two text-shadows simulating pressed-into-paper depth. */
  text-shadow:
    -1px -1px 0 rgba(255,255,255,0.9),
     1px  1px 1px rgba(10,15,30,0.18),
     0    2px 2px rgba(10,15,30,0.10);
  /* Better than text-shadow: background-clip with a baked deboss gradient. */
  background-image: linear-gradient(180deg,
    rgba(10,15,30,0.08) 0%,
    rgba(10,15,30,0.02) 35%,
    rgba(10,15,30,0.10) 100%);
  -webkit-background-clip: text;
          background-clip: text;
  /* Optional paper grain overlay — adds the print feel. */
  filter: drop-shadow(0 1px 0 rgba(255,255,255,0.6));
}

/* Paper grain texture, masked behind the wordmark only. */
.ftr3-press::before {
  content: ""; position: absolute; inset: 0; z-index: -1;
  background:
    radial-gradient(circle at 20% 30%, rgba(10,15,30,.04) 0, transparent 40%),
    radial-gradient(circle at 80% 70%, rgba(10,15,30,.03) 0, transparent 45%),
    repeating-linear-gradient(0deg,
      rgba(10,15,30,0.015) 0 1px, transparent 1px 3px);
  pointer-events: none;
  mix-blend-mode: multiply;
}

.ftr3-press__caption {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(10,15,30,0.52);
  text-align: center;
  margin: 0;
}
```
```html
<!-- Replaces lines 1111–1123 (.ftr3-cta) -->
<section class="ftr3-press" aria-label="ختام الصفحة">
  <div class="ftr3-press__inner">
    <p class="ftr3-press__caption">الرياض · ٢٠٢٦ · المبادر</p>
    <h2 class="ftr3-press__mark" aria-hidden="true">المبادر</h2>
    <p class="ftr3-press__caption">شبكة محتوى الأعمال والمال في السعودية</p>
  </div>
</section>
```

**Reference**: Wave 1 typography REF 8 (Letters from Sweden — Funkis ABC, architectural lettering as identity); Stripe Press masthead pattern; Bloomberg's printed-brand-page footers.
URL: https://lettersfromsweden.se/font/funkis/ + https://stripe.press/

**Effort**: ~5hr (1 to delete the CTA banner, 2 to compose the deboss + grain, 1 for RTL/breakpoint testing, 1 for prefers-contrast/PRM fallbacks).

**RTL safe**: yes — the wordmark is centered Arabic, no directional gradient, no flipped glyphs.

**prefers-reduced-motion**: no motion to disable; deboss is a static effect. Honor `prefers-contrast: more` by switching from text-shadow deboss to a solid `color: var(--void)` rendering — the deboss requires <4.5:1 contrast by design and high-contrast users need real AAA-text.

**Risk**: **The cream surface inverts the site's dark posture.** The rest of `.ftr3` stays dark, so this is a 200-px bright stripe between the dark page body and the dark sitemap. Test at scroll velocity — if the brightness flash feels jarring, soften with a vertical gradient `linear-gradient(180deg, var(--void) 0, var(--paper) 12%, var(--paper) 88%, var(--void) 100%)` to ease the transition. **Mitigation:** ship behind a feature flag or as the brand-column-only treatment first — keep the press strip optional until you've watched 5 sessions of users scrolling past.

---

## MOVE 2: Hover-construct on the wordmark (path-stroke decompose)

**The moment**: Hover the `المبادر` wordmark and over 700 ms the filled letterforms hollow out into their constructed strokes — the curves, joins, and counters reveal themselves like a font-specimen sheet. Move away and they fill back in. It says: *we know typography. The brand is built, not chosen.*

**Where**: Apply to the existing brand column wordmark (`.ftr3-mark__svg` at `index.html:1128–1132`), or to the new `.ftr3-press__mark` from MOVE 1. Convert the SVG `<text>` to outlined `<path>` first — the audit (#7) already flagged this for font-fidelity; this move pays back that work.

**Implementation**:
```css
/* Assumes wordmark is now a <path d="…"> with stroke + fill. */
.ftr3-mark__path {
  fill: var(--ink);
  stroke: var(--ink);
  stroke-width: 0;
  transition:
    fill .55s var(--ease-glide),
    stroke-width .55s var(--ease-glide);
}

.ftr3-mark:hover .ftr3-mark__path,
.ftr3-mark:focus-visible .ftr3-mark__path {
  fill: transparent;
  stroke-width: 1.4;
}

/* Construction lines fade in on hover — vertical & horizontal guides
   intersecting at the joins of each letter. */
.ftr3-mark__guides {
  opacity: 0;
  transition: opacity .55s var(--ease-glide) .15s;
  stroke: var(--crimson);
  stroke-width: 0.5;
  stroke-dasharray: 2 4;
  fill: none;
}

.ftr3-mark:hover .ftr3-mark__guides,
.ftr3-mark:focus-visible .ftr3-mark__guides {
  opacity: 0.6;
}

@media (prefers-reduced-motion: reduce) {
  .ftr3-mark__path,
  .ftr3-mark__guides {
    transition: none;
  }
}
```
```html
<!-- Inside .ftr3-mark, replacing the inline SVG text node -->
<svg viewBox="0 0 600 220" class="ftr3-mark__svg" aria-hidden="true">
  <!-- Construction guides drawn first (behind) -->
  <g class="ftr3-mark__guides">
    <line x1="80"  y1="40" x2="80"  y2="180"/>
    <line x1="220" y1="40" x2="220" y2="180"/>
    <line x1="380" y1="40" x2="380" y2="180"/>
    <line x1="520" y1="40" x2="520" y2="180"/>
    <line x1="40"  y1="100" x2="560" y2="100"/>
    <line x1="40"  y1="160" x2="560" y2="160"/>
  </g>
  <!-- Outlined wordmark paths (replace with traced from Mostaqbali) -->
  <path class="ftr3-mark__path" d="M…/* outlined المبادر paths */"/>
</svg>
```

**Reference**: Wave 1 typography REF 7 (Apple WWDC — variable-axis specimen typography) + Rauno's craft archive hover-reveals.
URL: https://rauno.me/craft + https://developer.apple.com/fonts/

**Effort**: ~4hr (3 to outline the wordmark to paths via Glyphs/Illustrator + 1 to wire the hover transitions). The outlining work is **already required** by audit #7; this move costs only the +1 hour of construction-line authoring.

**RTL safe**: yes — the wordmark is Arabic, its construction is symmetrical, no left/right semantics.

**prefers-reduced-motion**: drop the transition (instant state swap); user still sees the construction on hover, just without the morph. Easter-egg integrity preserved.

**Risk**: low. The crimson construction lines must be subtle enough not to read as decoration. If the paths look noisy, drop the guides entirely and just do the fill→stroke morph. The wordmark must already be path-outlined — if we ship live `<text>` it 404s the brand font (audit #7).

---

## MOVE 3: "mubadir" type-the-phrase easter egg

**The moment**: Anywhere on the page, no input focused, the user types `m-u-b-a-d-i-r`. The footer wordmark wakes up: it cycles through the 5 brand colors (crimson → verde → gold → azure → crimson) over 1.6s, the live-clock pulse syncs to the cycle, and a faint Arabic footnote types itself out below the wordmark: `أهلاً بك في الشبكة` ("welcome to the network"). One-shot per session, persisted in `sessionStorage`.

**Where**: Global keystroke buffer at the document level; visual effect targets `.ftr3-mark`, `.ftr3-clock__pulse`, and a new `.ftr3-press__welcome` node hidden by default in the cream press region (or appended dynamically if MOVE 1 isn't shipped).

**Implementation**:
```javascript
// Append inside the existing IIFE in v3.js around line 497 (after the form handler).
(() => {
  const TARGET = 'mubadir';
  if (sessionStorage.getItem('ftr3.eg.mubadir')) return;
  let buf = '';
  const root = document.querySelector('.ftr3');
  if (!root) return;

  const trigger = () => {
    sessionStorage.setItem('ftr3.eg.mubadir', '1');
    document.documentElement.classList.add('ftr3-eg-mubadir');
    // Smooth-scroll the user to the footer if not already there.
    const prm = matchMedia('(prefers-reduced-motion: reduce)').matches;
    root.scrollIntoView({ behavior: prm ? 'auto' : 'smooth', block: 'end' });
    // Welcome footnote types itself out (skip the type-out under PRM).
    const welcome = root.querySelector('.ftr3-eg-welcome');
    if (welcome) {
      const text = 'أهلاً بك في الشبكة';
      if (prm) { welcome.textContent = text; welcome.classList.add('is-on'); }
      else {
        welcome.textContent = '';
        welcome.classList.add('is-on');
        let i = 0;
        const tick = () => {
          if (i >= text.length) return;
          welcome.textContent += text[i++];
          setTimeout(tick, 55);
        };
        tick();
      }
    }
    setTimeout(() => {
      document.documentElement.classList.remove('ftr3-eg-mubadir');
    }, 8000);
  };

  document.addEventListener('keydown', (e) => {
    if (e.target.matches('input, textarea, [contenteditable]')) return;
    if (e.metaKey || e.ctrlKey || e.altKey) return;
    buf = (buf + (e.key || '')).slice(-TARGET.length).toLowerCase();
    if (buf === TARGET) trigger();
  });
})();
```
```css
/* Color-cycle the wordmark + sync the clock pulse during the easter egg.
   8s window total: 1.6s × 5 stops, holds at the end. */
@keyframes ftr3-eg-hue {
   0% { color: var(--ink); }
  20% { color: var(--crimson); }
  40% { color: var(--verde); }
  60% { color: var(--gold); }
  80% { color: var(--azure); }
 100% { color: var(--ink); }
}

.ftr3-eg-mubadir .ftr3-mark__path,
.ftr3-eg-mubadir .ftr3-press__mark {
  animation: ftr3-eg-hue 1.6s linear 0s 5;
}

.ftr3-eg-mubadir .ftr3-clock__pulse {
  animation-duration: 0.32s; /* 5x faster — sync to the hue cycle */
}

.ftr3-eg-welcome {
  font-family: var(--font-ar-display);
  font-size: clamp(14px, 1.2vw, 18px);
  color: var(--ink-soft);
  text-align: center;
  opacity: 0;
  transition: opacity .4s var(--ease-glide);
  margin: 12px 0 0;
  min-height: 1.6em; /* prevents layout shift during type-out */
}

.ftr3-eg-welcome.is-on { opacity: 1; }

@media (prefers-reduced-motion: reduce) {
  .ftr3-eg-mubadir .ftr3-mark__path,
  .ftr3-eg-mubadir .ftr3-press__mark { animation: none; color: var(--crimson); }
  .ftr3-eg-mubadir .ftr3-clock__pulse { animation: none; }
}
```
```html
<!-- Inside .ftr3-press__inner (or appended to .ftr3-col--brand) -->
<p class="ftr3-eg-welcome" aria-live="polite"></p>
```

**Reference**: Wave 1 interaction REF 12 (Type-the-Phrase Easter Egg — tasteful, brand-anchored).
URL: `design-audit/wow-research-interaction.md:233-253`

**Effort**: ~2hr (45 min for keystroke buffer + 45 min for hue cycle + 30 min for the welcome type-out).

**RTL safe**: yes — typed phrase is Latin (`mubadir`), the welcome footnote is Arabic (`أهلاً بك في الشبكة`), both render in their natural direction.

**prefers-reduced-motion**: no animation; instant state swap (color jumps to crimson, welcome footnote appears static, clock pulse halts). Discovery survives without motion.

**Risk**: Very low. The trigger requires intent (7-letter sequence, no inputs focused). Worst case: a Latin keyboard user accidentally types it while drafting an email — but they'd need an empty input context, which the guard prevents. **Don't add a Konami-style indicator** — telling the user the egg exists kills it. Track activation via GA4 custom event so marketing can measure how many readers find it.

---

## MOVE 4: Wormhole back-to-top (next-section-pulls-up gesture)

**The moment**: As the user nears the page bottom, the hero section's first element starts to **rise into the back-to-top button** — the button isn't just an icon, it's becoming a portal back to the page top. Hover it: a thin vertical line draws upward from the button toward the top of the viewport, then snaps into a smooth scroll. On click: the page collapses upward in 1.2s with a slight curve, and on arrival the hero's wordmark gently settles into place.

**Where**: Replace the existing `.ftr3-top` button behavior at `index.html:1197` and `v3.js:470–471`.

**Implementation**:
```css
.ftr3-top {
  position: relative; /* anchor for the wormhole line */
  overflow: visible;
}

/* The wormhole line: hidden, drawn upward on hover. */
.ftr3-top::before {
  content: "";
  position: absolute;
  bottom: 100%;
  left: 50%;
  width: 1px;
  height: 0;
  background: linear-gradient(180deg, transparent 0, var(--crimson) 100%);
  transform: translateX(-50%);
  transition: height .9s var(--ease-glide);
  pointer-events: none;
}

.ftr3-top:hover::before,
.ftr3-top:focus-visible::before {
  height: 80px; /* draws upward toward the page */
}

/* Active-press tightens the line briefly. */
.ftr3-top:active::before {
  height: 32px;
  transition: height .15s var(--ease-glide);
}

@media (prefers-reduced-motion: reduce) {
  .ftr3-top::before { display: none; }
}
```
```javascript
// Replace v3.js:470–471
const topBtn = root.querySelector('#ftr3-top');
if (topBtn) {
  const prm = matchMedia('(prefers-reduced-motion: reduce)').matches;
  topBtn.addEventListener('click', (e) => {
    e.preventDefault();
    if (prm) {
      window.scrollTo({ top: 0, behavior: 'auto' });
      return;
    }
    // Custom curved scroll: ease-out-quint over 1.2s for the 14,809-px page.
    const start = window.scrollY;
    const t0 = performance.now();
    const dur = Math.min(1400, 600 + start * 0.08); // longer page = longer scroll, capped
    const ease = (t) => 1 - Math.pow(1 - t, 5); // quint-out
    const step = (now) => {
      const t = Math.min(1, (now - t0) / dur);
      window.scrollTo(0, start * (1 - ease(t)));
      if (t < 1) requestAnimationFrame(step);
      else {
        // Settle: tiny rebound on the hero wordmark for "arrival" feel.
        const wm = document.querySelector('.hero3__wordmark, .hdr3-logo-word');
        if (wm) {
          wm.animate(
            [{ transform: 'translateY(-4px)' }, { transform: 'translateY(0)' }],
            { duration: 280, easing: 'cubic-bezier(.28,.11,.32,1)' }
          );
        }
      }
    };
    requestAnimationFrame(step);
  });
}
```

**Reference**: Wave 1 interaction REF 8 (Scroll-Cue "Wormhole" — the next section reaches up).
URL: `design-audit/wow-research-interaction.md:155-171` + https://rauno.me/craft

**Effort**: ~3hr (1 for the line draw, 1 for the curved scroll loop, 1 for the arrival settle + PRM fallback).

**RTL safe**: yes — the wormhole line is vertical (no horizontal direction), arrival rebound is vertical.

**prefers-reduced-motion**: skip the line draw, skip the curved scroll, skip the arrival rebound. Native `window.scrollTo({top: 0, behavior: 'auto'})` — instant.

**Risk**: 1.2s of forced scroll on a 14,809-px page can feel long. Cap the duration via `Math.min(1400, 600 + start * 0.08)` — if user is already near top, the scroll is short. Test on iOS Safari where smooth-scroll has historically been buggy. **Don't combine** with native `scroll-behavior: smooth` on `<html>` — explicit RAF loop wins, native fallback is the PRM path.

---

## MOVE 5: Live-clock per-second tick pulse

**The moment**: Every time the seconds digit rolls over (`14:32:14` → `14:32:15`), the entire clock pill performs a 200-ms breath: green pulse dot scale-pulses (1 → 1.18 → 1) and the time digits' tabular-nums slot the new second in with a 50-ms vertical slide. It's the first thing that feels *actually* live on the page — not a "live data" lie like the audit (#9) flagged, but a literal "this is reading the current second from your timezone right now" honesty.

**Where**: Existing clock at `index.html:1136` + the rAF/setTimeout loop in `v3.js:452–460`. The audit (#3) already flagged the loop as broken; this move both fixes it AND adds the tick pulse.

**Implementation**:
```javascript
// Replace v3.js:452–460
const clockEl = root.querySelector('#ftr3-clock');
const pulseEl = root.querySelector('.ftr3-clock__pulse');
if (clockEl) {
  const fmt = new Intl.DateTimeFormat('ar-SA-u-nu-latn', {
    timeZone: 'Asia/Riyadh', hour12: false,
    hour: '2-digit', minute: '2-digit', second: '2-digit'
  });
  const prm = matchMedia('(prefers-reduced-motion: reduce)').matches;
  let prev = '';
  const tick = () => {
    const next = fmt.format(new Date());
    if (next !== prev) {
      clockEl.textContent = next;
      prev = next;
      if (!prm) {
        // Pulse the dot; honor the existing keyframe-pulse (already 2s loop)
        // by adding a brief secondary "tick" class for 200ms.
        clockEl.classList.add('is-tick');
        if (pulseEl) pulseEl.classList.add('is-tick');
        setTimeout(() => {
          clockEl.classList.remove('is-tick');
          pulseEl?.classList.remove('is-tick');
        }, 220);
      }
    }
  };
  tick();
  // SINGLE setInterval — fixes the audit #3 rAF/setTimeout double-pump bug.
  let interval = setInterval(tick, 1000);
  document.addEventListener('visibilitychange', () => {
    clearInterval(interval);
    if (!document.hidden) {
      tick();
      interval = setInterval(tick, 1000);
    }
  });
}
```
```css
/* Tick state: 220ms breathing pulse on the dot + a hairline slide on digits. */
.ftr3-clock__time {
  display: inline-block;
  transition: transform .12s var(--ease-glide);
}

.ftr3-clock.is-tick .ftr3-clock__time {
  transform: translateY(-1px);
}

.ftr3-clock__pulse.is-tick {
  /* Layer the tick scale on top of the existing 2s pulse keyframe. */
  animation: ftr3-clock-tick .22s var(--ease-glide), ftr3-pulse 2s var(--ease-glide) infinite;
}

@keyframes ftr3-clock-tick {
   0% { transform: scale(1);    box-shadow: 0 0 0 0 rgba(31,163,122,.45); }
  50% { transform: scale(1.18); box-shadow: 0 0 0 6px rgba(31,163,122,.18); }
 100% { transform: scale(1);    box-shadow: 0 0 0 0 rgba(31,163,122,0); }
}

@media (prefers-reduced-motion: reduce) {
  .ftr3-clock__time,
  .ftr3-clock__pulse,
  .ftr3-clock__pulse.is-tick { animation: none; transform: none; }
}
```

**Reference**: Wave 1 motion REF 8 (Arc Browser — small but precise UI motion); Stripe Sigma billing dashboard's per-update pulse.
URL: https://arc.net/ + Stripe internal patterns observed via Stripe Press masthead clock.

**Effort**: ~2hr (1 to fix the loop per audit #3 + 1 for the tick layer + PRM gate).

**RTL safe**: decorative — the dot pulses radially, the tabular-nums digits slide vertically. No horizontal direction.

**prefers-reduced-motion**: kill both the per-tick animation and the existing 2s ambient pulse (currently runs unconditionally — audit miss #30). Time text still updates every second; the pulsing stops. Honest, accessible, alive.

**Risk**: Audit #9 flagged the clock as a vanity feature ("kill criteria: if no reader said they checked it, delete"). This move makes it *better* but doesn't answer that question. Consider this a **conditional ship**: if Wave 3 keeps the clock, do this; if it deletes the clock, this move is moot. **Critically**: also remove `aria-live="polite"` from `.ftr3-clock` (audit #9) — the SR will not announce every second tick.

---

## MOVE 6: Per-handle "verified" tick + last-active dot

**The moment**: Each Instagram handle in the Network column gets a 12-px verified tick after it (real Meta blue check, only on the accounts that are actually verified — which is most of them). On hover, a tooltip appears: "آخر منشور · ٣ ساعات" (last post · 3h). The handles stop feeling like a static link list and start feeling like a presence dashboard.

**Where**: Network column links at `index.html:1141–1147`. Verified status is hardcoded per handle for now (no backend). Last-active is a placeholder until a feed integration ships.

**Implementation**:
```html
<!-- Per handle, replace lines like: -->
<li><a href="https://instagram.com/almobadir" target="_blank" rel="noopener" data-color="#E6213B" data-verified="true" data-last="3h">
  <span class="ftr3-cdot" style="--c:#E6213B"></span>
  <span class="ftr3-handle">@almobadir</span>
  <svg class="ftr3-verified" viewBox="0 0 24 24" width="14" height="14" aria-label="موثّق">
    <path fill="#1DA1F2" d="M12 2 9.4 4.6 5.8 4.6 5.8 8.2 3.2 12l2.6 3.8v3.6h3.6L12 22l2.6-2.6h3.6v-3.6L20.8 12l-2.6-3.8V4.6h-3.6zM10.5 16 7 12.5l1.4-1.4 2.1 2.1 5.1-5.1 1.4 1.4-6.5 6.5z"/>
  </svg>
  <span class="ftr3-meta">Flagship</span>
</a></li>
```
```css
.ftr3-verified {
  flex: 0 0 auto;
  margin-inline-start: 4px;
  opacity: 0.75;
  transition: opacity .25s var(--ease-glide), transform .35s var(--ease-glide);
}

.ftr3-list--net a:hover .ftr3-verified {
  opacity: 1;
  transform: scale(1.1);
}

/* Last-active tooltip — appears on hover next to .ftr3-meta. */
.ftr3-list--net a {
  position: relative;
}

.ftr3-list--net a[data-last]::after {
  content: "آخر منشور · " attr(data-last);
  position: absolute;
  inset-inline-end: 0;
  bottom: calc(100% + 4px);
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.04em;
  color: var(--ink-soft);
  background: var(--void-2);
  padding: 4px 8px;
  border-radius: 6px;
  border: 1px solid var(--hairline-strong);
  opacity: 0;
  transform: translateY(4px);
  pointer-events: none;
  transition: opacity .25s var(--ease-glide), transform .25s var(--ease-glide);
  white-space: nowrap;
}

.ftr3-list--net a:hover[data-last]::after,
.ftr3-list--net a:focus-visible[data-last]::after {
  opacity: 1;
  transform: translateY(0);
}

@media (prefers-reduced-motion: reduce) {
  .ftr3-verified,
  .ftr3-list--net a[data-last]::after { transition: none; }
}
```

**Reference**: Wave 1 interaction REF 13 (Hover-Earned Reveal — the "1px" payoff) + Twitter/X verified-badge pattern.
URL: `design-audit/wow-research-interaction.md:256-275`

**Effort**: ~2.5hr (1 for the verified-tick SVG + per-handle data attributes + 1 for the tooltip + 30 min for RTL/spacing tuning).

**RTL safe**: yes — the verified tick sits adjacent to the handle's leading edge (right side in RTL); the tooltip uses `inset-inline-end` and originates above the handle row. No directional glyphs.

**prefers-reduced-motion**: drop the tooltip transition (it appears/disappears without animation). Verified tick is static.

**Risk**: **Brand color collision.** The Twitter blue check (#1DA1F2) doesn't sit in Almobadir's palette (crimson/verde/gold/azure). Either (a) recolor the tick to `var(--azure)` (#1E5AA8 — close enough to read as a "verified" cue without aping Twitter), or (b) use a custom check glyph that's clearly Almobadir's. (a) is safer for first ship. The "آخر منشور · 3h" data is **fake** until a live feed ships — either remove the `data-last` attribute on accounts you can't verify, or pull from Instagram Basic Display API on build.

---

## MOVE 7: Network-handle hue-shift on hover

**The moment**: Hover any Instagram handle, the handle text **briefly shifts to that account's brand color** (the existing `data-color` already maps to the right hex), AND the surrounding sitemap column dims by 8% for 320 ms — focusing attention on the one handle you're hovering. It's the Linear-list-row pattern, applied to a network directory.

**Where**: Network column at `index.html:1141–1147`. The `data-color` attribute and `--c` custom property are already wired from the dot logic at `v3.css:925-928`.

**Implementation**:
```css
.ftr3-list--net a {
  --c: var(--crimson); /* fallback */
  transition: color .3s var(--ease-glide), transform .3s var(--ease-glide);
}

/* Pull the per-link color onto the handle text. */
.ftr3-list--net a[data-color] {
  --c: attr(data-color color, var(--crimson)); /* Chrome 125+; fallback below */
}

/* Browser-safe: read --c from inline style (already set on .ftr3-cdot). */
.ftr3-list--net a:hover .ftr3-handle,
.ftr3-list--net a:focus-visible .ftr3-handle {
  color: var(--c, var(--ink));
}

/* Dim the column when ANY handle in it is hovered. */
.ftr3-list--net:hover {
  --net-dim: 0.6;
}

.ftr3-list--net:hover li {
  opacity: var(--net-dim, 1);
  transition: opacity .32s var(--ease-glide);
}

.ftr3-list--net:hover li:has(a:hover) {
  opacity: 1;
}

/* Fallback for browsers without :has() — accept the dim doesn't lift on the
   hovered row; still readable, no broken state. */
@supports not (selector(:has(a))) {
  .ftr3-list--net:hover li { opacity: 1; }
}

@media (prefers-reduced-motion: reduce) {
  .ftr3-list--net a,
  .ftr3-list--net:hover li { transition: none; }
}
```
```javascript
// Set --c on each <a> from data-color so CSS can read it without attr().
root.querySelectorAll('.ftr3-list--net a[data-color]').forEach(a => {
  a.style.setProperty('--c', a.dataset.color);
});
```

**Reference**: Wave 1 interaction REF 13 (Hover-Earned Reveal — Linear's sidebar item hover).
URL: `design-audit/wow-research-interaction.md:256-275`

**Effort**: ~1.5hr (45 min CSS, 30 min JS to set the custom prop, 15 min RTL/breakpoint check).

**RTL safe**: decorative — color shifts on the text in place, no direction.

**prefers-reduced-motion**: drop the column-dim transition. Color still shifts on hover (instant).

**Risk**: minimal. The `:has()` selector works in 92% of browsers (Apr 2026); the `@supports` fallback gracefully degrades. **Watch**: the column-dim creates a momentary flash — at 320 ms it should feel intentional, but if it reads as a glitch on slower devices, drop the dim entirely and keep just the per-handle hue-shift.

---

## Ranked Top 3 (ship first)

1. **MOVE 1 — Debossed wordmark on cream paper sub-region.** Highest taste-per-hour ratio. Replaces the audit's worst offender (the redundant 60-px CTA banner duplicating `nl3-section`) with the strongest editorial moment on the entire site. Pays back two audit issues (#4, #5, #8) and earns one signature gesture. The cream sub-region inversion is the bold call — flag that risk to the brand owner before shipping.

2. **MOVE 2 — Hover-construct on the wordmark.** The cleanest "we know type" gesture available. Pays back audit #7 (wordmark must be path-outlined for font-resilience anyway) and adds 1 hour of visual reward on top. Works whether or not MOVE 1 ships.

3. **MOVE 3 — "mubadir" type-the-phrase easter egg.** Pure delight, two hours of work, zero copy required. Ships behind a session flag so it's a one-time discovery. Best surface for measuring "is anyone reading deeply enough to find this?" via a GA4 event — gives the team a quantifiable engagement signal that doesn't depend on form submissions.

**Combined ship cost: ~11 hours.** Remaining 4 moves (4 wormhole, 5 clock-tick, 6 verified, 7 hue-shift) are good "Phase 2 polish" — pair them with the post-audit Phase 2 typography and density sweep when that work cycles.

---

## 200-Word Summary

The footer needs signature, not more features. Seven moves are proposed; ranked top three ship in ~11 hours and pay back the strongest unresolved audit issues simultaneously. **MOVE 1** replaces the redundant CTA banner (audit #4) with a cream-paper sub-region whose pressed-into-paper wordmark is the most editorial moment on the site — a Stripe-Press-grade closing statement the page has not earned anywhere else. **MOVE 2** turns the wordmark into a typographic specimen on hover, paying back the path-outlining work audit #7 already requires. **MOVE 3** adds a type-the-phrase easter egg (`mubadir`) that costs two hours and gives the brand a discovered-not-broadcast moment fit for a Saudi business audience. The remaining four moves polish what's already there: a wormhole back-to-top respecting the 14,809-px page (REF 8), a per-second clock tick that fixes audit #3's broken rAF loop, verified ticks on Instagram handles, and a Linear-style hue-shift on the network directory. All seven moves are RTL-safe, `prefers-reduced-motion`-compliant, and require zero copy changes. The single risk worth flagging: MOVE 1 inverts a 200-px stripe of the dark footer to cream — ship behind a feature flag and watch five sessions before committing.
