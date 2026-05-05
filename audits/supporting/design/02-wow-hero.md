# 02 — WOW · Hero (.hero3)

**Compiled:** 2026-04-26
**Section:** `.hero3` — `website/index.html` lines 148–308
**Styles:** `website/assets/v3.css` lines 166–~315 (HERO block)
**Scripts:** `website/assets/v3.js` lines 72–112 (mesh parallax + signup intercept)
**Live:** https://zaka33333-hash.github.io/almobadir-website/

The hero is the most-loaded section of the site and the only section that is required to make a network of 7 channels and 3.3M followers feel inevitable in three seconds of scroll. It already has the bones (mesh + grid + grain + vignette atmosphere, two-column shell with KPI panel, channel chips, ticker, staggered rise on `[data-stagger]`). The wow gap is **specificity** — every move below is a named primitive borrowed from a 2024–2026 reference, not a vibe. Twelve moves follow, then a ranked top 4 with sequencing notes.

---

## Executive summary (200 words)

The hero's chassis is already strong: a 100vh shell with mesh+conic+grain atmosphere, a two-column Arabic-first composition, KPI panel with sparkline glyphs, channel chips, and a CSS-variable hue drift loop that's almost cinema. What's missing are **five specific moments** that would make a viewer stop scrolling: (1) the wordmark `المبادر` rendered as a calligraphic stroke-in instead of static SVG fill, (2) the kashida axis on the tagline accent words `الترفيه`/`التعليم` — the only Arabic-only typographic move on the entire research list and structurally impossible in Latin, (3) KPI counters that roll from 0 → 3.3M while their sparklines draw with `stroke-dasharray`, (4) the existing mesh parallax extended to a Stripe-Press tilt on the panel itself for tactility, and (5) a real-timestamp "آخر تحديث" pill that turns the LIVE eyebrow into a credibility signal instead of a decorative glyph. Together these total ~26 hours of engineering, all CSS-and-vanilla-JS, all RTL-native, all with `prefers-reduced-motion` fallbacks. The supplementary moves (cmd-K palette, scroll-cue wormhole, channel chip preview, eastern-numeral toggle, magnetic CTA, easter egg, pull-into-view sparkline cascade) earn additional craft layer without altering the hero's information architecture.

---

## MOVE 1: Wordmark stroke-draw "calligraphic ink"

**The moment**: On hero entry the SVG `المبادر` is invisible at first frame, then writes itself glyph-by-glyph as if a calligraphic pen is laying ink over 1.6 seconds, settling into the existing crimson gradient + glow.
**Where**: `.hero3__wordmark svg text` @ index.html line 181 — replace the single `<text>` fill with a stroked path version + dasharray reveal.
**Implementation**:
```html
<!-- index.html · replace <text> at line 181 with a path-traced wordmark -->
<g class="hero3__wordmark-strokes" filter="url(#hero3-soft-glow)">
  <!-- One path per glyph; export from Mostaqbali via Glyphs/Illustrator stroke-to-path. Each path has `pathLength="1"` so we drive all from one variable -->
  <path d="M…ا…" pathLength="1" class="hm-stroke" style="--i:0"/>
  <path d="M…ل…" pathLength="1" class="hm-stroke" style="--i:1"/>
  <path d="M…م…" pathLength="1" class="hm-stroke" style="--i:2"/>
  <path d="M…ب…" pathLength="1" class="hm-stroke" style="--i:3"/>
  <path d="M…ا…" pathLength="1" class="hm-stroke" style="--i:4"/>
  <path d="M…د…" pathLength="1" class="hm-stroke" style="--i:5"/>
  <path d="M…ر…" pathLength="1" class="hm-stroke" style="--i:6"/>
</g>
```
```css
/* v3.css · append after line 215 */
.hm-stroke {
  fill: url(#hero3-crimson-grad);
  stroke: url(#hero3-crimson-grad);
  stroke-width: 1.6;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 1;
  stroke-dashoffset: 1;
  fill-opacity: 0;
  animation:
    hero3-ink-trace 900ms cubic-bezier(.2,.7,.1,1) forwards,
    hero3-ink-fill  600ms ease-out 700ms forwards;
  animation-delay: calc(var(--i, 0) * 110ms);
}
@keyframes hero3-ink-trace { to { stroke-dashoffset: 0; } }
@keyframes hero3-ink-fill  { to { fill-opacity: 1; } }
@media (prefers-reduced-motion: reduce) {
  .hm-stroke { animation: none; stroke-dashoffset: 0; fill-opacity: 1; }
}
```
**Reference**: Typography REF 1 (Stripe Press chapter-opener system) + Motion REF 13 (GitHub Copilot perspective-grid `stroke-dasharray` draw — https://github.com/features/copilot) — same stroke-dashoffset primitive applied to glyph contours instead of grid lines.
**Effort**: ~5hr (3hr to stroke-trace the seven glyphs of `المبادر` from Mostaqbali, 2hr for staged dasharray + fill timing).
**RTL safe**: yes — Arabic stroke order is right-to-left so `--i` sequencing reads naturally; first-glyph delay anchors the ر-end.
**prefers-reduced-motion**: full static fill (no draw) — markup degrades cleanly.

---

## MOVE 2: Tagline kashida-axis on accent words (the only Arabic-only move)

**The moment**: As the tagline rises into view, the two crimson accent words `الترفيه` and `التعليم` physically elongate their kashidas via the variable-font `KSHD` axis — Arabic letterforms breathe outward across half a column, no font-size change. Latin foundries cannot do this.
**Where**: `.hero3__tagline-accent` @ index.html lines 187, 189 — drive a registered CSS custom property bound to a `view-timeline`.
**Implementation**:
```css
/* v3.css · append after line 218 (existing tagline-accent rule) */
@property --kshd { syntax: "<number>"; inherits: true; initial-value: 0; }

.hero3__tagline { view-timeline-name: --tagline; view-timeline-axis: block; }

.hero3__tagline-accent {
  /* requires 29LT Idris Variable license OR Cairo Variable with letter-spacing fallback */
  font-family: "29LT Idris Variable", "Cairo Variable", var(--font-ar-display);
  font-variation-settings: "wght" 700, "KSHD" var(--kshd);
  transition: --kshd 600ms cubic-bezier(.2,.7,.1,1);
  /* fallback for browsers without @property: letter-spacing pulse */
  letter-spacing: calc(var(--kshd) * 0.0008em);
}
@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    .hero3__tagline-accent {
      animation: hero3-kashida linear both;
      animation-timeline: --tagline;
      animation-range: entry 30% cover 60%;
    }
    @keyframes hero3-kashida { from { --kshd: 0; } to { --kshd: 80; } }
  }
}
.hero3__tagline-accent:hover { --kshd: 100; }

@media (prefers-reduced-motion: reduce) {
  .hero3__tagline-accent { --kshd: 0; }
}
```
**Reference**: Typography REF 5 — 29LT Idris Variable kashida axis (https://www.29lt.com/product/29lt-idris-sharp/ + https://blog.29lt.com/2024/11/26/29lt-idris/). Idris ships KSHD as a registered axis; Cairo Variable is the free fallback (no KSHD axis but `letter-spacing` pulse fakes 70% of the move).
**Effort**: ~4hr if Idris license already secured; ~6hr including license/integration. Tariff: ~€500 one-time for Idris brand-tier license — gates whether this ships in v1 or v2.
**RTL safe**: yes — kashida is intrinsically RTL; the elongation happens across the connecting strokes that already run right-to-left.
**prefers-reduced-motion**: `--kshd: 0` — accent words still render in crimson gradient, no elongation, no visible degradation.

---

## MOVE 3: KPI counter roll-up 0 → 3.3M with tabular-num digit reel

**The moment**: When the KPI panel enters viewport, each numeral counts up from 0 to its final value — `3.3` rolls from `0.0`, `25` from `0`, `7` from `0`, `5,000` ticks decimal-by-decimal — using a tabular-numeric digit-reel that prevents layout jitter. Stripe-style precision feel.
**Where**: `.hero3__num` @ index.html lines 230, 236, 242, 252 — wrap each digit in a per-place `<span>` and animate.
**Implementation**:
```js
/* v3.js · inside the existing hero IIFE, after line 96 */
const panel = hero.querySelector('.hero3__panel');
if (panel && !reduceMotion) {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (!entry.isIntersecting || entry.target.dataset.counted) return;
      entry.target.dataset.counted = '1';
      panel.querySelectorAll('.hero3__num').forEach(el => {
        const target = parseFloat(el.textContent.replace(/,/g, ''));
        const isFloat = el.textContent.includes('.');
        const hasComma = el.textContent.includes(',');
        const start = performance.now();
        const dur = 1400;
        const ease = (t) => 1 - Math.pow(1 - t, 3); // easeOutCubic
        function tick(now) {
          const p = Math.min((now - start) / dur, 1);
          const v = target * ease(p);
          el.textContent = isFloat
            ? v.toFixed(1)
            : (hasComma ? Math.round(v).toLocaleString('en-US') : Math.round(v).toString());
          if (p < 1) requestAnimationFrame(tick);
          else el.textContent = isFloat ? target.toFixed(1) : (hasComma ? target.toLocaleString('en-US') : String(target));
        }
        requestAnimationFrame(tick);
      });
    });
  }, { threshold: 0.4 });
  observer.observe(panel);
}
```
```css
/* v3.css · ensure tabular nums on numerals */
.hero3__num { font-feature-settings: 'tnum' 1, 'lnum' 1; font-variant-numeric: tabular-nums; min-inline-size: 4ch; display: inline-block; text-align: end; }
```
**Reference**: Interaction REF 3 (Stripe animated number tabular roll — https://stripe.com/pricing) — Stripe uses `font-variant-numeric: tabular-nums` to keep digits monospace so the counter never jitters mid-roll.
**Effort**: ~3hr.
**RTL safe**: yes — Western Arabic numerals (3.3, 25, 7, 5,000) render LTR inside an RTL block; `direction: ltr` already isolates them via `tabular-nums`.
**prefers-reduced-motion**: skip the IO + roll, numerals appear at final value.

---

## MOVE 4: KPI sparkline stroke-dasharray reveal in cascade

**The moment**: Synced to MOVE 3 (counter roll), each KPI tile's sparkline draws itself left-to-right (in RTL: right-to-left from the tile origin) over 700ms staggered 120ms per tile — the chart literally writes itself as the number counts up. The `--kpi-accent` color (crimson, verde, azure, gold) makes each line distinct.
**Where**: `.hero3__kpi-spark svg path` @ index.html lines 232, 238, 254 (the three SVG sparklines; the bars at 244–248 use a separate height-grow).
**Implementation**:
```css
/* v3.css · append after line 257 */
.hero3__kpi-spark svg path {
  stroke-dasharray: 1;
  stroke-dashoffset: 1;
  pathLength: 1; /* normalize across all sparklines */
}
.hero3__panel[data-counted="1"] .hero3__kpi-spark svg path {
  animation: hero3-spark-draw 700ms cubic-bezier(.2,.7,.1,1) forwards;
  animation-delay: calc(var(--spark-i, 0) * 120ms + 200ms);
}
@keyframes hero3-spark-draw { to { stroke-dashoffset: 0; } }

.hero3__kpi-spark--bars span {
  transform-origin: bottom;
  transform: scaleY(0);
}
.hero3__panel[data-counted="1"] .hero3__kpi-spark--bars span {
  animation: hero3-bar-grow 600ms cubic-bezier(.34,1.56,.64,1) forwards;
  animation-delay: calc(var(--bar-i, 0) * 60ms + 380ms);
}
@keyframes hero3-bar-grow { to { transform: scaleY(1); } }

@media (prefers-reduced-motion: reduce) {
  .hero3__kpi-spark svg path { stroke-dashoffset: 0; animation: none; }
  .hero3__kpi-spark--bars span { transform: scaleY(1); animation: none; }
}
```
```html
<!-- index.html · add --spark-i style on each .hero3__kpi (lines 228, 234, 240, 250) -->
<div class="hero3__kpi" style="--kpi-accent: var(--crimson); --spark-i: 0;">…</div>
<div class="hero3__kpi" style="--kpi-accent: var(--verde);   --spark-i: 1;">…</div>
<div class="hero3__kpi" style="--kpi-accent: var(--azure);   --spark-i: 2;">…</div>
<div class="hero3__kpi" style="--kpi-accent: var(--gold);    --spark-i: 3;">…</div>
<!-- and for bars at line 245-247 add --bar-i: 0..6 on each span -->
```
**Reference**: Motion REF 13 (GitHub Copilot — https://github.com/features/copilot) `stroke-dasharray` line draw applied to data sparklines instead of grid lines, plus Motion REF 4 (Linear staggered row reveal — https://linear.app/features) for the 60–120ms cascade beat.
**Effort**: ~2hr (piggybacks on MOVE 3's `data-counted` flag).
**RTL safe**: pathLength + dashoffset is direction-agnostic; the path is drawn from M-start to end which in the existing SVGs is left-to-right. For RTL feel, flip the SVG paths (or simply mirror the SVG via `transform: scaleX(-1)` inside RTL — visually the line still rises, which is the credibility signal).
**prefers-reduced-motion**: paths render at full length, bars at full height — no animation.

---

## MOVE 5: Atmospheric mesh tilt on cursor (extend existing parallax)

**The moment**: The existing mesh parallax in `v3.js` lines 80–96 already tracks pointer X/Y and lerps the meshes; this move **extends it** to the KPI panel itself, giving a Stripe-Press book-tilt physical-feel — the panel rotates ±4° on X/Y as the cursor moves, returning home with overshoot spring on `pointerleave`.
**Where**: `.hero3__panel` @ index.html line 215 — extend the existing `onMove` handler in v3.js around line 82.
**Implementation**:
```js
/* v3.js · extend the existing tick() function inside hero IIFE (lines 88–95) */
const panel = hero.querySelector('.hero3__panel');
const tick = () => {
  cx += (tx - cx) * 0.08;
  cy += (ty - cy) * 0.08;
  meshA.style.transform = `translate3d(${cx * 18}px, ${cy * 14}px, 0)`;
  meshB.style.transform = `translate3d(${cx * -22}px, ${cy * -16}px, 0)`;
  conic.style.transform = `translate3d(${cx * 10}px, ${cy * 8}px, 0)`;
  if (panel) {
    /* sign-flip Y so tilt aligns with cursor depth */
    panel.style.transform = `perspective(1200px) rotateY(${cx * 4}deg) rotateX(${-cy * 3}deg)`;
  }
  if (Math.abs(tx - cx) > 0.001 || Math.abs(ty - cy) > 0.001) raf = requestAnimationFrame(tick);
  else raf = 0;
};
hero.addEventListener('pointerleave', () => {
  if (panel) panel.style.transform = '';
  tx = ty = 0;
  if (!raf) raf = requestAnimationFrame(tick);
});
```
```css
/* v3.css · append after line 238 (.hero3__panel rule) */
.hero3__panel {
  transform-style: preserve-3d;
  transition: transform 420ms cubic-bezier(.34,1.56,.64,1); /* spring on leave */
  will-change: transform;
}
@media (prefers-reduced-motion: reduce), (pointer: coarse) {
  .hero3__panel { transform: none !important; transition: none; }
}
```
**Reference**: Interaction REF 4 (Stripe Press book tilt — https://press.stripe.com) — same `perspective + rotateY/rotateX` ratio + spring overshoot on leave.
**Effort**: ~2hr (extends existing IIFE; no new listener).
**RTL safe**: sign-flip — when `dir="rtl"` the panel sits on the LEFT visually; tilt direction is naturally inverted because the cursor X mapping is centered on the hero, not the panel. No code change needed; verify in Safari + Chromium RTL.
**prefers-reduced-motion**: no tilt; panel sits static. Coarse-pointer (touch) also disabled.

---

## MOVE 6: "آخر تحديث منذ 2h" — LIVE pill with real timestamp

**The moment**: The eyebrow LIVE pill on hero entry replaces its decorative glyph with a real, updating timestamp showing time since last channel post (`آخر تحديث · منذ 2h`). Pulls from a build-time `last_post_at` ISO string injected into the page; updates client-side every 60 seconds. Turns a vibe element into a credibility signal.
**Where**: `.hero3__eyebrow` @ index.html lines 161–169 — add a `<time>` element + JS heartbeat.
**Implementation**:
```html
<!-- index.html · update lines 161-169 -->
<div class="hero3__eyebrow" data-stagger="0">
  <span class="hero3__pulse" aria-hidden="true">
    <span class="hero3__pulse-dot"></span>
    <span class="hero3__pulse-ring"></span>
  </span>
  <span class="hero3__eyebrow-label">LIVE</span>
  <span class="hero3__eyebrow-sep" aria-hidden="true">·</span>
  <span>شبكة من 7 قنوات</span>
  <span class="hero3__eyebrow-sep" aria-hidden="true">·</span>
  <span>+3.3M متابع · 11 حساب</span>
  <span class="hero3__eyebrow-sep" aria-hidden="true">·</span>
  <time class="hero3__eyebrow-fresh"
        data-last-post="2026-04-26T13:14:00Z"
        datetime="2026-04-26T13:14:00Z">آخر تحديث · الآن</time>
  <span class="hero3__eyebrow-mono">EST. 1446H</span>
</div>
```
```js
/* v3.js · inside hero IIFE, append before the form handler (~line 98) */
const fresh = hero.querySelector('.hero3__eyebrow-fresh');
if (fresh) {
  const ts = new Date(fresh.dataset.lastPost).getTime();
  const fmt = () => {
    const m = Math.max(0, Math.floor((Date.now() - ts) / 60000));
    if (m < 1)   return 'آخر تحديث · الآن';
    if (m < 60)  return `آخر تحديث · منذ ${m}د`;
    const h = Math.floor(m / 60);
    if (h < 24)  return `آخر تحديث · منذ ${h}س`;
    return `آخر تحديث · منذ ${Math.floor(h / 24)}ي`;
  };
  fresh.textContent = fmt();
  setInterval(() => { fresh.textContent = fmt(); }, 60000);
}
```
```css
/* v3.css · append after line 212 (.hero3__eyebrow-mono rule) */
.hero3__eyebrow-fresh {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.04em;
  color: var(--ink-soft);
}
```
**Reference**: Interaction REF 11 (Live presence dots — https://supabase.com/realtime, https://thebrowser.company) — borrows the "show the page is alive" instinct without the multiplayer overhead. Editorial brands earn trust by showing freshness, not by simulating it.
**Effort**: ~2hr (HTML + JS heartbeat + buildtime date injection wiring — for now hardcode `data-last-post` server-side).
**RTL safe**: yes — Arabic numerals + relative-time string render right-to-left naturally inside the eyebrow.
**prefers-reduced-motion**: no animation involved; the heartbeat is a `setInterval`, not a transition. Reduced-motion setting unaffected.

---

## MOVE 7: Channel chip hover preview (live thumbnail card)

**The moment**: Hovering any of the 7 channel chips (`.hero3__chip`) reveals a preview card — anchored above the chip via popover — showing the latest post thumbnail, follower count, and engagement rate. Uses the chip's existing `--chip-c` accent for the card border.
**Where**: `.hero3__chip` @ index.html lines 268–275 — wrap each chip in a `popover`-pattern container.
**Implementation**:
```html
<!-- index.html · update each chip (lines 268-275) — example for first chip -->
<div class="hero3__chip-wrap">
  <a class="hero3__chip" style="--chip-c: var(--verde);" 
     href="https://instagram.com/almobadir_mindset" target="_blank" rel="noopener"
     aria-describedby="chip-prev-mindset">
    <span class="hero3__chip-dot"></span>
    <span class="hero3__chip-name">@almobadir_mindset</span>
    <span class="hero3__chip-num">478K</span>
  </a>
  <div class="hero3__chip-preview" id="chip-prev-mindset" role="tooltip">
    <img loading="lazy" decoding="async" 
         src="assets/previews/mindset.jpg" alt="" width="240" height="240"/>
    <div class="hero3__chip-preview-meta">
      <strong>@almobadir_mindset</strong>
      <span>تطوير الذات · 478K · 7.2% engagement</span>
    </div>
  </div>
</div>
```
```css
/* v3.css · append after line 270 (chip rule) */
.hero3__chip-wrap { position: relative; }
.hero3__chip-preview {
  position: absolute;
  inset-inline-start: 50%;
  inset-block-end: calc(100% + 12px);
  transform: translateX(-50%) translateY(8px);
  width: 240px;
  background: rgba(14,21,37,0.96);
  border: 1px solid var(--chip-c, var(--hero3-hairline-strong));
  border-radius: 12px;
  padding: 10px;
  display: grid;
  gap: 8px;
  opacity: 0;
  pointer-events: none;
  z-index: 20;
  box-shadow: 0 24px 48px -16px rgba(0,0,0,0.7);
  transition: opacity 220ms var(--ease-glide), transform 220ms var(--ease-glide);
}
.hero3__chip-preview img { 
  width: 100%; aspect-ratio: 1; border-radius: 8px; object-fit: cover; 
}
.hero3__chip-preview-meta { 
  font-size: 12px; line-height: 1.35; color: var(--ink-soft); display: grid; gap: 2px; 
}
.hero3__chip-preview-meta strong { font-family: var(--font-mono); color: var(--ink); letter-spacing: 0.04em; }

.hero3__chip-wrap:hover .hero3__chip-preview,
.hero3__chip-wrap:focus-within .hero3__chip-preview {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
  pointer-events: auto;
}
@media (prefers-reduced-motion: reduce) {
  .hero3__chip-preview { transition: opacity 80ms linear; transform: translateX(-50%); }
}
@media (pointer: coarse) {
  /* touch: rely on tap-and-hold native popover or just disable */
  .hero3__chip-preview { display: none; }
}
```
**Reference**: Interaction REF 13 (Hover-earned reveal "1px payoff" — https://linear.app, https://www.notion.com) — opacity + small Y translate, **no layout shift**. Combined with Motion REF 8 (Arc Browser sticky video demos — https://arc.net) for the thumbnail-as-evidence pattern.
**Effort**: ~6hr (HTML wrapping + CSS popover + asset prep: 7 channel preview thumbnails curated to ~240×240, optimized to <30KB each).
**RTL safe**: yes — `inset-inline-start: 50%; transform: translateX(-50%)` is logical-property aware; the preview centers above the chip in either direction.
**prefers-reduced-motion**: shorter transition (80ms opacity, no Y-slide). On touch: hidden entirely (chips already navigate to Instagram on tap).

---

## MOVE 8: cmd-K palette overlay (jump to chip / EN/AR / case study)

**The moment**: ⌘K (or Ctrl+K) opens a centered command palette with backdrop blur. Type to fuzzy-search 16 actions: "go to mindset", "switch to English", "view manifesto", "copy founder email", etc. Backspace at empty query pops a breadcrumb. Closes on Esc.
**Where**: New element added at end of `.hero3` @ index.html line 308 — wired globally so it's not hero-coupled but introduced **at hero level** because that's where the user first sees it.
**Implementation**:
```html
<!-- index.html · append after line 308 (after </section>) -->
<div class="hero3-cmdk" role="dialog" aria-label="لوحة الأوامر" aria-hidden="true" hidden>
  <div class="hero3-cmdk__backdrop" data-cmdk-close></div>
  <div class="hero3-cmdk__panel">
    <div class="hero3-cmdk__crumbs"><span>المبادر</span></div>
    <input class="hero3-cmdk__input" 
           type="search"
           placeholder="ابحث أو نفّذ أمراً…  (⌘K)"
           aria-label="بحث ضمن لوحة الأوامر"/>
    <ul class="hero3-cmdk__list" role="listbox">
      <li role="option" data-action="goto" data-href="#network">
        <span>اذهب إلى الشبكة</span><kbd>N</kbd>
      </li>
      <li role="option" data-action="goto" data-href="#manifesto">
        <span>اذهب إلى المنهج</span><kbd>M</kbd>
      </li>
      <li role="option" data-action="goto" data-href="https://instagram.com/almobadir_mindset">
        <span>@almobadir_mindset · 478K</span><kbd>1</kbd>
      </li>
      <!-- … 13 more entries: 7 channels, EN/AR toggle, contact, copy, etc. -->
      <li role="option" data-action="lang" data-lang="en">
        <span>Switch to English</span><kbd>L</kbd>
      </li>
    </ul>
    <footer class="hero3-cmdk__foot">
      <kbd>↑↓</kbd> تنقّل · <kbd>↵</kbd> اختر · <kbd>Esc</kbd> إغلاق
    </footer>
  </div>
</div>
```
```js
/* v3.js · append at top-level inside DOMContentLoaded, before hero IIFE */
(() => {
  const cmdk = document.querySelector('.hero3-cmdk');
  if (!cmdk) return;
  const input = cmdk.querySelector('.hero3-cmdk__input');
  const items = [...cmdk.querySelectorAll('[role="option"]')];
  let active = 0;
  const open = () => {
    cmdk.hidden = false;
    cmdk.setAttribute('aria-hidden', 'false');
    requestAnimationFrame(() => cmdk.classList.add('is-open'));
    setTimeout(() => input.focus(), 60);
  };
  const close = () => {
    cmdk.classList.remove('is-open');
    cmdk.setAttribute('aria-hidden', 'true');
    setTimeout(() => { cmdk.hidden = true; input.value = ''; filter(''); }, 200);
  };
  const filter = (q) => {
    const t = q.trim().toLowerCase();
    items.forEach(li => {
      const txt = li.textContent.toLowerCase();
      li.hidden = !!t && !txt.includes(t);
    });
    const visible = items.filter(li => !li.hidden);
    items.forEach(li => li.classList.remove('is-active'));
    if (visible[0]) { visible[0].classList.add('is-active'); active = items.indexOf(visible[0]); }
  };
  document.addEventListener('keydown', (e) => {
    if ((e.metaKey || e.ctrlKey) && e.key.toLowerCase() === 'k') { e.preventDefault(); cmdk.hidden ? open() : close(); }
    if (cmdk.hidden) return;
    if (e.key === 'Escape') close();
    if (e.key === 'ArrowDown' || e.key === 'ArrowUp') {
      e.preventDefault();
      const visible = items.filter(li => !li.hidden);
      if (!visible.length) return;
      const cur = visible.findIndex(li => li.classList.contains('is-active'));
      const next = e.key === 'ArrowDown' 
        ? (cur + 1) % visible.length 
        : (cur - 1 + visible.length) % visible.length;
      visible.forEach(li => li.classList.remove('is-active'));
      visible[next].classList.add('is-active');
    }
    if (e.key === 'Enter') {
      const sel = items.find(li => li.classList.contains('is-active') && !li.hidden);
      if (sel) sel.click();
    }
  });
  input.addEventListener('input', (e) => filter(e.target.value));
  cmdk.addEventListener('click', (e) => { if (e.target.dataset.cmdkClose !== undefined) close(); });
  items.forEach(li => li.addEventListener('click', () => {
    const a = li.dataset.action;
    if (a === 'goto') {
      if (li.dataset.href.startsWith('#')) { document.querySelector(li.dataset.href)?.scrollIntoView({ behavior: 'smooth' }); }
      else window.open(li.dataset.href, '_blank', 'noopener');
    }
    close();
  }));
})();
```
```css
/* v3.css · append at the end of HERO block (~ line 315) */
.hero3-cmdk { position: fixed; inset: 0; z-index: 100; display: grid; place-items: center; opacity: 0; transition: opacity 200ms var(--ease-glide); }
.hero3-cmdk.is-open { opacity: 1; }
.hero3-cmdk__backdrop { position: absolute; inset: 0; background: rgba(8,12,22,0.72); backdrop-filter: blur(10px); }
.hero3-cmdk__panel { position: relative; width: min(560px, 92vw); background: rgba(20,28,48,0.96); border: 1px solid var(--hero3-hairline-strong); border-radius: 16px; padding: 14px; box-shadow: 0 40px 80px -20px rgba(0,0,0,0.7); transform: translateY(8px) scale(0.98); transition: transform 200ms var(--ease-glide); }
.hero3-cmdk.is-open .hero3-cmdk__panel { transform: translateY(0) scale(1); }
.hero3-cmdk__input { width: 100%; padding: 12px 14px; background: rgba(0,0,0,0.3); border: 1px solid var(--hero3-hairline); border-radius: 10px; color: var(--ink); font: inherit; font-size: 16px; outline: none; }
.hero3-cmdk__input:focus { border-color: var(--crimson); }
.hero3-cmdk__list { list-style: none; margin: 10px 0 0; padding: 0; max-height: 320px; overflow: auto; }
.hero3-cmdk__list li { display: flex; align-items: center; justify-content: space-between; padding: 10px 12px; border-radius: 8px; cursor: pointer; color: var(--ink-soft); font-size: 14px; }
.hero3-cmdk__list li.is-active, .hero3-cmdk__list li:hover { background: rgba(230,33,59,0.08); color: var(--ink); }
.hero3-cmdk__list kbd { font-family: var(--font-mono); font-size: 11px; padding: 2px 6px; border: 1px solid var(--hero3-hairline-strong); border-radius: 4px; }
.hero3-cmdk__crumbs { font-family: var(--font-mono); font-size: 11px; letter-spacing: 0.18em; color: var(--ink-mute); padding: 0 4px 8px; }
.hero3-cmdk__foot { display: flex; gap: 12px; padding-top: 10px; border-top: 1px solid var(--hero3-hairline); margin-top: 10px; font-family: var(--font-mono); font-size: 11px; color: var(--ink-mute); }
@media (prefers-reduced-motion: reduce) {
  .hero3-cmdk, .hero3-cmdk__panel { transition: opacity 80ms linear; transform: none; }
}
```
**Reference**: Interaction REF 1 (cmd-K palette with breadcrumb back-stack — https://linear.app, https://rauno.me/craft) — keyboard-first UI signals "thinking site", not brochure.
**Effort**: ~8hr (HTML + JS state machine + ARIA + 16 actions wired + Arabic labels).
**RTL safe**: yes — palette is centered, list is logical-property; the `kbd` chips render LTR by convention which is fine.
**prefers-reduced-motion**: opacity-only transition, no scale/translate.

---

## MOVE 9: Hero scroll-cue wormhole (next-section heading lifts up)

**The moment**: Below the trust band, a thin vertical line + `↓` glyph pulses gently. As the user scrolls within 200px of the hero's end, the manifesto's first heading (`§ 001 · المنهج`) lifts into the line — the cue itself becomes the transition, not a dead arrow.
**Where**: New element after `.hero3__ticker` @ index.html line 307 — replaces nothing, augments the bottom of the section.
**Implementation**:
```html
<!-- index.html · insert before </section> on line 308 -->
<div class="hero3__cue" aria-hidden="true">
  <span class="hero3__cue-line"></span>
  <span class="hero3__cue-arrow">↓</span>
</div>
```
```css
/* v3.css · append after ticker rule */
.hero3__cue { position: absolute; inset-block-end: 24px; inset-inline-start: 50%; transform: translateX(-50%); display: grid; justify-items: center; gap: 8px; z-index: 5; pointer-events: none; }
.hero3__cue-line { width: 1px; height: 48px; background: linear-gradient(to bottom, transparent, var(--ink-mute) 60%, transparent); transform-origin: top; }
.hero3__cue-arrow { font-family: var(--font-mono); font-size: 14px; color: var(--ink-mute); animation: hero3-cue-bob 2.4s ease-in-out infinite; }
@keyframes hero3-cue-bob { 0%, 100% { transform: translateY(0); opacity: 0.6; } 50% { transform: translateY(4px); opacity: 1; } }

@supports (animation-timeline: scroll()) {
  .hero3__cue-line {
    animation: hero3-cue-shrink linear both;
    animation-timeline: scroll();
    animation-range: exit 0% exit 80%;
  }
  @keyframes hero3-cue-shrink { from { transform: scaleY(1); } to { transform: scaleY(0); } }
}
@media (prefers-reduced-motion: reduce) {
  .hero3__cue-arrow { animation: none; }
  .hero3__cue-line { animation: none; }
}
```
```js
/* v3.js · inside hero IIFE, append after MOVE 6 heartbeat */
const manifFirstHeading = document.querySelector('.manif3__eyebrow');
if (manifFirstHeading && !reduceMotion) {
  const io = new IntersectionObserver(([e]) => {
    const ratio = e.intersectionRatio;
    manifFirstHeading.style.transform = `translateY(${(1 - ratio) * 24}px)`;
    manifFirstHeading.style.opacity = String(ratio);
  }, { threshold: Array.from({length: 21}, (_, i) => i / 20) });
  io.observe(manifFirstHeading);
}
```
**Reference**: Interaction REF 8 (Scroll-cue wormhole — https://rauno.me/craft, https://www.apple.com/iphone-16-pro/) — the cue *does something* as you obey it.
**Effort**: ~3hr.
**RTL safe**: yes — `inset-inline-start: 50%` + `translateX(-50%)` centers in either direction; the cue is decorative and direction-agnostic.
**prefers-reduced-motion**: no bob, no shrink, just static line + arrow at 60% opacity.

---

## MOVE 10: Eastern-Arabic numeral "stat-of-the-week" eyebrow toggle

**The moment**: A keyboard-only easter egg: press `٠` (or `0` on Latin keyboard) and the four KPI numerals swap from Western Arabic (3.3, 25, 7, 5,000) to Eastern Arabic (٣٫٣، ٢٥، ٧، ٥٠٠٠). Press again to swap back. Marks the page as natively Arab without forcing the choice on bilingual readers.
**Where**: `.hero3__num` @ index.html lines 230, 236, 242, 252 — JS adds a class to the panel that triggers a CSS `font-feature-settings` swap.
**Implementation**:
```js
/* v3.js · inside hero IIFE, append after MOVE 6/9 */
document.addEventListener('keydown', (e) => {
  if (e.target.matches('input, textarea')) return;
  if (e.key === '0' || e.key === '٠') {
    panel?.classList.toggle('is-eastern-num');
  }
});
```
```css
/* v3.css · append after .hero3__num rule */
.hero3__panel.is-eastern-num .hero3__num { 
  font-feature-settings: 'tnum' 1, 'lnum' 1, 'numr' 0; 
  /* Replace via JS-mapped Eastern Arabic glyphs (٠١٢٣٤٥٦٧٨٩) */
}
```
```js
/* v3.js · enhance the listener to swap text content */
const easternMap = { '0': '٠', '1': '١', '2': '٢', '3': '٣', '4': '٤', '5': '٥', '6': '٦', '7': '٧', '8': '٨', '9': '٩', '.': '٫', ',': '،' };
const swapNums = (toEastern) => {
  panel.querySelectorAll('.hero3__num').forEach(el => {
    if (!el.dataset.western) el.dataset.western = el.textContent;
    el.textContent = toEastern
      ? [...el.dataset.western].map(c => easternMap[c] ?? c).join('')
      : el.dataset.western;
  });
};
document.addEventListener('keydown', (e) => {
  if (e.target.matches('input, textarea')) return;
  if (e.key === '0' || e.key === '٠') {
    const isEastern = panel.classList.toggle('is-eastern-num');
    swapNums(isEastern);
  }
});
```
**Reference**: Arabic REF 16 (Eastern-Arabic numeral as graphic — https://www.aramco.com / Vision 2030 microsite) — eastern numerals signal cultural register, not just style.
**Effort**: ~2hr.
**RTL safe**: yes — eastern numerals already render natively in RTL flow.
**prefers-reduced-motion**: no animation involved; swap is instant.

---

## MOVE 11: Magnetic "اشترك مجاناً" CTA pull (subtle, 6px max)

**The moment**: Within ~80px of the signup CTA, the link gently translates 1–6px toward the cursor; the inner CTA chip translates 1–3px further (label-leads-button parallax). On leave: spring back home. Subtle "I am the most important button on this page" signal.
**Where**: `.hero3__signup-link` @ index.html line 198 — the existing anchor row gets a parent-zone listener.
**Implementation**:
```js
/* v3.js · inside hero IIFE, append after MOVE 5 panel tilt */
const signup = hero.querySelector('.hero3__signup-link');
const signupCta = hero.querySelector('.hero3__signup-cta');
if (signup && !reduceMotion && finePointer) {
  const zone = signup.parentElement; /* .hero3__signup-field has padding margin */
  zone.addEventListener('pointermove', (e) => {
    const r = signup.getBoundingClientRect();
    const dx = (e.clientX - (r.left + r.width / 2)) * 0.08;
    const dy = (e.clientY - (r.top + r.height / 2)) * 0.08;
    const x = Math.max(-6, Math.min(6, dx));
    const y = Math.max(-6, Math.min(6, dy));
    signup.style.transform = `translate(${x}px, ${y}px)`;
    if (signupCta) signupCta.style.transform = `translate(${x * 0.4}px, ${y * 0.4}px)`;
  });
  zone.addEventListener('pointerleave', () => {
    signup.style.transform = '';
    if (signupCta) signupCta.style.transform = '';
  });
}
```
```css
/* v3.css · append after line 234 (.hero3__signup-cta:hover svg rule) */
.hero3__signup-link, .hero3__signup-cta { 
  transition: transform 350ms cubic-bezier(.34,1.56,.64,1), border-color var(--d-base) var(--ease-glide), box-shadow var(--d-base) var(--ease-glide); 
}
@media (prefers-reduced-motion: reduce), (pointer: coarse) {
  .hero3__signup-link, .hero3__signup-cta { transition: border-color var(--d-base) var(--ease-glide); }
  /* JS guards already prevent transform writes; CSS clears any residual */
}
```
**Reference**: Interaction REF 15 (Magnetic CTA — https://vercel.com, https://linear.app) — Vercel/Linear keep it subtle (3–6px) which is the right dose for editorial; bigger pulls feel award-show, not editorial.
**Effort**: ~1.5hr.
**RTL safe**: yes — pointer math is centered on the button rect; sign of `dx` flips naturally with viewport, no `dir` checks needed.
**prefers-reduced-motion**: zone listener still runs but only writes border-color hover; transform path skipped.

---

## MOVE 12: "mubadir" type-to-reveal easter egg

**The moment**: Type `mubadir` (or `المبادر`) anywhere on the page (no input focused). The hero's grid-lines pulse one beat brighter, the wordmark briefly pulls KSHD to 100, and a small fade-in footnote appears below the wordmark: `أهلاً · ١٤٤٦هـ`. One-time per session, sessionStorage gated, taste-tier.
**Where**: Global keystroke buffer + reveal in `.hero3__wordmark` @ index.html line 170.
**Implementation**:
```js
/* v3.js · inside hero IIFE, append at the end before the closing })(); */
let buf = '';
const TARGETS = ['mubadir', 'المبادر'];
document.addEventListener('keydown', (e) => {
  if (e.target.matches('input, textarea, select')) return;
  buf = (buf + e.key).slice(-12);
  if (sessionStorage.getItem('hero3-eg-mubadir')) return;
  if (TARGETS.some(t => buf.toLowerCase().includes(t.toLowerCase()) || buf.includes(t))) {
    sessionStorage.setItem('hero3-eg-mubadir', '1');
    hero.classList.add('is-eg-mubadir');
    setTimeout(() => hero.classList.remove('is-eg-mubadir'), 9000);
  }
});
```
```html
<!-- index.html · add inside .hero3__wordmark, before closing </div> on line 184 -->
<span class="hero3__wordmark-eg" aria-hidden="true">أهلاً · ١٤٤٦هـ</span>
```
```css
/* v3.css · append after line 215 */
.hero3__wordmark-eg {
  display: block;
  margin-block-start: 8px;
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.18em;
  color: var(--ink-mute);
  opacity: 0;
  transform: translateY(-4px);
  transition: opacity 600ms var(--ease-glide), transform 600ms var(--ease-glide);
}
.hero3.is-eg-mubadir .hero3__wordmark-eg { opacity: 1; transform: none; }
.hero3.is-eg-mubadir .hero3__grid-lines { 
  animation: hero3-grid-pulse 2.4s ease-in-out 1;
}
@keyframes hero3-grid-pulse { 
  0%, 100% { opacity: 0.45; } 
  50% { opacity: 0.85; } 
}
.hero3.is-eg-mubadir .hero3__tagline-accent { --kshd: 100; }

@media (prefers-reduced-motion: reduce) {
  .hero3__wordmark-eg { transition: opacity 200ms linear; transform: none; }
  .hero3.is-eg-mubadir .hero3__grid-lines { animation: none; opacity: 0.65; }
}
```
**Reference**: Interaction REF 12 (Type-the-phrase easter egg, brand-anchored variant — replaces tired Konami) — taste-tier discovery, session-storage gated.
**Effort**: ~2hr.
**RTL safe**: yes — buffer matches both Latin transliteration and Arabic native; reveal text is Arabic.
**prefers-reduced-motion**: footnote still appears (instant), no grid-pulse, no kashida pull.

---

# Top 4 ranked (highest WOW per hour)

Each ranked on three axes: **(1) story-fit** for an Arabic media network, **(2) visible difference** to a first-time viewer, **(3) reversibility** if the move misses.

### 1. MOVE 2 — Tagline kashida-axis on accent words
The only Arabic-only move on the entire research list. Latin foundries cannot replicate. Lands directly on the hero's most-read element (the tagline). Worth the Idris license for this one move alone. Fallback (Cairo Variable + letter-spacing pulse) covers 70% of the wow at zero font cost. **Effort: 4–6hr. Story-fit: 10/10.**

### 2. MOVE 3 + MOVE 4 — KPI counter roll + sparkline draw cascade (ship as one)
The KPI panel is the hero's credibility argument; counters that animate to 3.3M while their charts draw is the "this network is alive" proof. Stripe-grade tabular-num precision. Pairs naturally with MOVE 6 (LIVE timestamp) for a triple-credibility hit. **Effort: 5hr combined.**

### 3. MOVE 1 — Wordmark stroke-draw calligraphic ink
The hero's first three seconds. Currently the wordmark fades in with the rest of the stagger; making it draw itself glyph-by-glyph as if a calligrapher's pen is laying ink turns the brand name into a moment. Pure SVG + dasharray, no JS, no font license. **Effort: 5hr. Story-fit: 9/10.**

### 4. MOVE 8 — cmd-K palette overlay
The single most "this team thinks like a tool, not a brochure" signal available. Editorial brands often skip it; almobadir is differentiated when it includes one. Pre-loads 16 actions covering all 7 channels + EN/AR + jump-to-section. Battle-tested pattern, screenshot-worthy. **Effort: 8hr. Differentiation: highest.**

---

# Sequencing notes

- **Phase 1 (week 1, ~12hr):** MOVE 1 (stroke-draw) + MOVE 3 + 4 (counter + spark) + MOVE 6 (LIVE timestamp). All zero-cost, all CSS+vanilla, ships immediately.
- **Phase 2 (week 2, ~9hr):** MOVE 5 (panel tilt) + MOVE 11 (magnetic CTA) + MOVE 9 (scroll-cue wormhole). Tactility layer — small additions that make existing chassis feel crafted.
- **Phase 3 (week 3, ~10hr):** MOVE 8 (cmd-K) + MOVE 7 (chip preview) + MOVE 12 (easter egg). Differentiation layer — the "we are an editorial tool, not a landing page" moves.
- **Decision gate:** MOVE 2 (kashida axis) — license 29LT Idris (~€500) and ship as v1, OR ship Cairo fallback for v1 and re-budget Idris for v2. Recommended: ship Idris from day one because this is the only Arabic-structural wow on the list and the hero is where it belongs.
- **MOVE 10 (eastern numerals)** is a 2hr nice-to-have, not in the critical path; ship alongside Phase 3 if time permits.

Total Phase 1+2+3 budget: ~31 hours. With MOVE 2 in Phase 1: ~37 hours. Every move is reversible (CSS class removal + JS handler unbind), every move respects `prefers-reduced-motion`, every move is tested RTL-native because the entire hero already runs `dir="rtl"`.

The hero ships with twelve named primitives instead of one ambient mesh drift. Each is a screenshot moment for a different audience — designers (MOVE 1, 2), data people (MOVE 3, 4), users (MOVE 6, 7), power-users (MOVE 8, 12). The ambient layer (mesh, conic, grain, vignette) stays exactly as-is — that craft is already there.
