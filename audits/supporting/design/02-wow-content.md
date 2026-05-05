# Wave 2 WOW Moves — `.cnt3` Content Section

**Section:** `<section class="cnt3" id="cnt3-latest">` — "أحدث المحتوى من شبكة المُبادر"
**Source:** `website/index.html:855-927`, `website/assets/v3.css:601-661, 974, 1066-1074, 1170-1172, 1201`, `website/assets/v3.js:285-336`
**Bar:** Bloomberg, The Verge, Substack, Stripe Press, NYT homepage.
**Wave 1 inputs read:** `wow-research-motion.md`, `wow-research-typography.md`, `wow-research-interaction.md`, `wow-research-arabic.md`.

---

## Executive Summary (200 words)

The `.cnt3` block is the most editorially competent piece on the page — accent-per-sub-brand, skeleton-to-photo reveal, hover accent-bar grow, and a staggered IO entrance are all in place. But three cards on a 1320px shell, a stretched featured cell that leaks ~300px of dead space at 1080–1440px, and a static "العدد № 184" line undersell a network claiming +5,000 منشور across 6 platforms. The work is not adding chrome — it is restoring the editorial volume and making the resting state earn its space.

The eight moves below pull from Wave 1's strongest portable techniques: **REF 18 (Framer sticky-stack)**, **REF 11 (Things spring overshoot)**, **REF 3 (Stripe digit roll)**, **REF 1 (Stripe Press marginalia)**, **REF 14 (Frere-Jones live specimen)**, **REF 13 (Rauno hover-earned reveal)**, **REF 4 (Linear staggered rows)**, and Arabic **REF 1 (Aramco oversized numeral)** + **REF 17 (Khatt counter reveal)**. Every move has a `prefers-reduced-motion` fallback that preserves the underlying state, every move is RTL-safe (sign-flip noted where it matters), and the top three combined ship in roughly 11 engineering hours and turn the section from "competent grid" to "publication front page that someone will screenshot."

---

## MOVE 1: Sticky-Card Deck Stack (the marquee move)

**The moment**: As the reader scrolls past `.cnt3`, each card pins at top, the next card slides up over it, the receding card scales to 0.95 and dims — the section behaves like a deck of editorial spreads being dealt out.

**Where**: `.cnt3-grid > .cnt3-card` @ `index.html:868-917`; CSS @ `v3.css:614-619`. Engages on a parallel layout `.cnt3-grid--stack` that turns on at ≥6 cards (the editorial-volume fix from `02-content.md` finding #2).

**Implementation**:
```css
/* Opt-in stack mode — used when grid is converted to a vertical deck */
.cnt3-grid--stack { display: block; max-width: 920px; margin-inline: auto; }
.cnt3-grid--stack .cnt3-card {
  position: sticky;
  top: calc(var(--header-h, 72px) + 24px);
  margin-block-end: 24px;
  transition: transform .55s var(--ease-glide), opacity .55s var(--ease-glide), filter .55s var(--ease-glide);
  --depth: 0;
  transform: scale(calc(1 - 0.04 * var(--depth))) translateY(calc(var(--depth) * -8px));
  opacity: calc(1 - 0.18 * var(--depth));
  filter: blur(calc(var(--depth) * 1.5px));
  z-index: calc(10 - var(--depth));
}
@media (prefers-reduced-motion: reduce) {
  .cnt3-grid--stack .cnt3-card { position: static; transform: none; opacity: 1; filter: none; }
}
```
```js
// v3.js inside the cnt3 IIFE — depth tracker (only when .cnt3-grid--stack present)
const stack = root.querySelector('.cnt3-grid--stack');
if (stack && !reduceMotion) {
  const stackCards = [...stack.querySelectorAll('.cnt3-card')];
  const io2 = new IntersectionObserver(() => {
    const vh = window.innerHeight;
    stackCards.forEach((c, i) => {
      const r = c.getBoundingClientRect();
      // depth = how many cards have already pinned above this one
      const passed = stackCards.filter((o, j) => j < i && o.getBoundingClientRect().top <= 96).length;
      c.style.setProperty('--depth', Math.min(passed, 3));
    });
  }, { threshold: [0, 0.25, 0.5, 0.75, 1] });
  stackCards.forEach(c => io2.observe(c));
  window.addEventListener('scroll', () => io2.takeRecords && io2.takeRecords(), { passive: true });
}
```

**Reference**: Wave 1 motion REF 18 — Framer.com sticky stack — https://framer.com (fallback: motion REF 3 native `animation-timeline: view()`).
**Effort**: ~4hr.
**RTL safe**: yes — sticky/scale/blur are direction-agnostic; no horizontal translate.
**prefers-reduced-motion**: cards revert to static block flow with the existing IO entrance reveal — content still legible, no parallax or pin.

---

## MOVE 2: Spring-Lift + Saturated Brand-Color Shadow on Hover

**The moment**: Pointer enters a card, the card lifts 6px with the Things-3 overshoot curve, casts a colored shadow tinted by its `--accent` (crimson / verde / teal), and the cover image gently zooms — a single coordinated animation instead of three siblings firing in sequence.

**Where**: `.cnt3-card:hover` @ `v3.css:620, 629, 639, 642`. Replaces the current diffuse hover (border + bg + image-scale on different durations).

**Implementation**:
```css
:root { --ease-spring: cubic-bezier(.34, 1.56, .64, 1); }
.cnt3-card {
  transition:
    transform .42s var(--ease-spring),
    box-shadow .42s var(--ease-glide),
    border-color .3s var(--ease-glide);
  will-change: transform;
}
.cnt3-card:hover {
  transform: translateY(-6px);
  border-color: color-mix(in oklab, var(--accent) 55%, var(--hairline-strong));
  box-shadow:
    0 1px 0 0 color-mix(in oklab, var(--accent) 30%, transparent),
    0 18px 40px -12px color-mix(in oklab, var(--accent) 38%, transparent),
    0 32px 80px -20px rgba(0,0,0,.55);
}
.cnt3-card:hover .cnt3-img.is-loaded { transform: scale(1.06); }
@media (prefers-reduced-motion: reduce) {
  .cnt3-card { transition: border-color .3s linear, box-shadow .3s linear; }
  .cnt3-card:hover { transform: none; box-shadow: 0 0 0 1px color-mix(in oklab, var(--accent) 50%, transparent); }
}
```

**Reference**: Wave 1 motion REF 11 — Things-3 spring overshoot — https://culturedcode.com/things; complemented by interaction REF 13 (hover-earned reveal pattern) — https://rauno.me.
**Effort**: ~1hr.
**RTL safe**: yes — transform/shadow are mirror-symmetric.
**prefers-reduced-motion**: drops the lift; replaces the colored shadow with a 1px accent-colored ring so the affordance is preserved.

---

## MOVE 3: Stripe-Style Live Read-Time Counter (per featured card)

**The moment**: Card scrolls into the viewport; the read-time digit (e.g. "9 دقائق قراءة") rolls from 0 → 9 like a tabular slot reel, then sits still. The number that stares back already feels read.

**Where**: `.cnt3-read` @ `index.html:882, 898, 914`. Wrap the digit in `<span class="cnt3-read-num" data-target="9">0</span>` and run on first IO entry.

**Implementation**:
```html
<span class="cnt3-read"><span class="cnt3-read-num" data-target="9">0</span> دقائق قراءة</span>
```
```css
.cnt3-read-num {
  display: inline-block;
  font-variant-numeric: tabular-nums;
  font-feature-settings: "tnum" 1, "lnum" 1;
  min-width: 1ch;
  color: var(--ink-2);
  font-weight: 600;
}
```
```js
// inside cnt3 IIFE, after the entrance IO. Runs once per card.
const rollDigit = (el, target, dur = 700) => {
  if (reduceMotion) { el.textContent = target; return; }
  const start = performance.now();
  const ease = t => 1 - Math.pow(1 - t, 3);
  (function tick(now) {
    const p = Math.min((now - start) / dur, 1);
    el.textContent = Math.round(ease(p) * target);
    if (p < 1) requestAnimationFrame(tick);
  })(start);
};
const numIO = new IntersectionObserver((es) => {
  es.forEach(e => {
    if (!e.isIntersecting) return;
    const num = e.target;
    rollDigit(num, parseInt(num.dataset.target, 10));
    numIO.unobserve(num);
  });
}, { threshold: 0.6 });
root.querySelectorAll('.cnt3-read-num').forEach(n => numIO.observe(n));
```

**Reference**: Wave 1 interaction REF 3 — Stripe-style animated number tabular roll — https://stripe.com/pricing (Arabic-script note: digits are Western "9" — locked tabular so RTL flow doesn't shift width).
**Effort**: ~1.5hr.
**RTL safe**: yes — `tabular-nums` + Western digits + `min-width: 1ch` = no reflow regardless of direction.
**prefers-reduced-motion**: number is set instantly to its target; no animation.

---

## MOVE 4: "العدد ١٨٤" — Aramco-Style Oversized Issue Numeral (the editorial signature)

**The moment**: The currently shy `.cnt3-issue` line (`العدد № 184`, 12px mono) is replaced by a confident bilingual masthead: tiny Arabic label "العدد" stacked over a 64–96px Eastern-Arabic "١٨٤" set in the display face. Reads "issue 184" but looks like a printed quarterly's spine.

**Where**: `.cnt3-head-right` @ `index.html:862-866`; CSS @ `v3.css:611-613`.

**Implementation**:
```html
<div class="cnt3-head-right">
  <div class="cnt3-issue-mark" aria-label="العدد رقم مئة وأربعة وثمانين">
    <span class="cnt3-issue-eyebrow">العدد</span>
    <span class="cnt3-issue-num" aria-hidden="true">١٨٤</span>
  </div>
  <span class="cnt3-rule" aria-hidden="true"></span>
  <span class="cnt3-week">أسبوع 17 / 2026</span>
</div>
```
```css
.cnt3-issue-mark { display: inline-flex; flex-direction: column; align-items: center; gap: 2px; line-height: 1; }
.cnt3-issue-eyebrow {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--ink-mute);
}
.cnt3-issue-num {
  font-family: var(--font-ar-display);
  font-weight: 900;
  font-size: clamp(48px, 6vw, 88px);
  letter-spacing: -0.03em;
  color: var(--ink);
  background: linear-gradient(180deg, var(--ink) 0%, color-mix(in oklab, var(--ink) 40%, transparent) 100%);
  -webkit-background-clip: text; background-clip: text; -webkit-text-fill-color: transparent;
  font-variant-numeric: tabular-nums;
  font-feature-settings: "ss01" 1; /* if Arabic font has stylistic-set numerals */
}
@media (max-width: 680px) { .cnt3-issue-num { font-size: clamp(36px, 12vw, 56px); } }
```

**Reference**: Wave 1 arabic REF 1 — Aramco Annual Report 2024 (single Arabic numeral as full-bleed page) and arabic REF 16 — Eastern-Arabic numeral as graphic.
**Effort**: ~1.5hr.
**RTL safe**: yes — Eastern-Arabic-Indic digits ١٨٤ flow naturally in RTL with `unicode-bidi: isolate` inherited from the section.
**prefers-reduced-motion**: no animation involved — pure typesetting.

---

## MOVE 5: Editorial DateTime with Live Relative-Time ("منذ ٤ أيام")

**The moment**: Each card's date now reads "22 أبريل · منذ ٤ أيام" — the calendar date sits beside a live relative-time string that re-computes on visibility-change and every 30 minutes. Card recency becomes self-evident without the user calculating.

**Where**: `.cnt3-time` @ `index.html:882, 898, 914`; new sibling span next to existing `<time>`.

**Implementation**:
```html
<span class="cnt3-time">
  <time datetime="2026-04-22">22 أبريل</time>
  <span class="cnt3-rel" aria-hidden="true">·</span>
  <span class="cnt3-rel-text" data-iso="2026-04-22">منذ ٤ أيام</span>
</span>
```
```css
.cnt3-rel { color: var(--hairline-strong); margin-inline: 6px; }
.cnt3-rel-text { color: var(--ink-mute); font-feature-settings: "tnum" 1; }
```
```js
// add to cnt3 IIFE
const rtfAr = new Intl.RelativeTimeFormat('ar', { numeric: 'auto' });
const updateRel = () => {
  const now = Date.now();
  root.querySelectorAll('.cnt3-rel-text').forEach(el => {
    const iso = el.dataset.iso;
    if (!iso) return;
    const diffDays = Math.round((new Date(iso).getTime() - now) / 86400000);
    if (diffDays > -1) { el.textContent = 'اليوم'; return; }
    if (diffDays === -1) { el.textContent = 'أمس'; return; }
    if (diffDays > -30) { el.textContent = rtfAr.format(diffDays, 'day'); return; }
    el.textContent = rtfAr.format(Math.round(diffDays / 30), 'month');
  });
};
updateRel();
document.addEventListener('visibilitychange', () => { if (!document.hidden) updateRel(); });
setInterval(updateRel, 30 * 60 * 1000);
```

**Reference**: Wave 1 typography REF 12 — Are.na content-first typesetting (date/byline rhythm) and interaction REF 11 — live presence cues. Built on `Intl.RelativeTimeFormat` (native Arabic locale).
**Effort**: ~1hr.
**RTL safe**: yes — `Intl.RelativeTimeFormat('ar')` outputs correct Arabic plurals and direction.
**prefers-reduced-motion**: no animation — text-only update.

---

## MOVE 6: Cover Photo Ken Burns + Diagonal Sweep on Reveal

**The moment**: The instant a card's photo finishes loading, it does a slow 8-second Ken Burns zoom-and-pan from `scale(1.06) translate(0,0)` to `scale(1.0) translate(-1.5%, 0.8%)`. On hover, the pan reverses and a diagonal sheen wipes once across the cover.

**Where**: `.cnt3-img` @ `v3.css:627-629`; `.cnt3-cover` @ `v3.css:622`.

**Implementation**:
```css
@keyframes cnt3-kb {
  from { transform: scale(1.07) translate3d(0, 0, 0); }
  to   { transform: scale(1.00) translate3d(-1.5%, 0.8%, 0); }
}
.cnt3-img.is-loaded {
  opacity: 1;
  animation: cnt3-kb 9s var(--ease-glide) forwards;
}
.cnt3-cover::after {
  content: '';
  position: absolute; inset: 0; z-index: 5; pointer-events: none;
  background: linear-gradient(115deg, transparent 30%, rgba(245,234,210,.16) 50%, transparent 70%);
  transform: translateX(-110%); /* RTL: visually starts off-screen at the trailing edge */
  transition: transform .9s var(--ease-glide);
}
.cnt3-card:hover .cnt3-cover::after { transform: translateX(110%); }

/* RTL: sheen sweeps from start-edge to end-edge; in RTL the 115deg + translateX still reads as "left-to-right cinematic wipe" — flip if brand prefers a right-to-left sweep */
[dir="rtl"] .cnt3-cover::after { background: linear-gradient(245deg, transparent 30%, rgba(245,234,210,.16) 50%, transparent 70%); transform: translateX(110%); }
[dir="rtl"] .cnt3-card:hover .cnt3-cover::after { transform: translateX(-110%); }

@media (prefers-reduced-motion: reduce) {
  .cnt3-img.is-loaded { animation: none; transform: scale(1.00); }
  .cnt3-cover::after { display: none; }
}
```

**Reference**: Wave 1 motion REF 8 — Arc Browser autoplaying video UI (subtle in-viewport motion); motion REF 12 — Figma Config '25 parallax cover.
**Effort**: ~1.5hr.
**RTL safe**: sign-flip — the sheen direction is decorative; the `[dir="rtl"]` override flips the angle and translation vector. Ken Burns pan is mirror-flipped via the same override (drop the `-1.5%` X if visually unwanted).
**prefers-reduced-motion**: animation suppressed; image renders at scale(1.0); sheen disabled.

---

## MOVE 7: Card Click Reveals First Paragraph In-Page (no link-out)

**The moment**: User clicks the card body (not the cover); the body height auto-expands and the first 80–120-word paragraph fades in below the excerpt. A small "متابعة على إنستغرام ↗" link replaces the standard CTA. The reader gets payoff without losing the page.

**Where**: `.cnt3-link` @ `index.html:870, 887, 903`. Convert from anchor-only to anchor + expand affordance via a sibling `<button>` and a `<details>` or JS-toggled section under `.cnt3-body`.

**Implementation**:
```html
<!-- inside each .cnt3-body, after .cnt3-meta -->
<details class="cnt3-expand">
  <summary class="cnt3-expand-toggle" aria-label="اقرأ مقدّمة المقال هنا">
    <span class="cnt3-expand-label">قراءة المقدّمة</span>
    <svg class="cnt3-expand-chev" viewBox="0 0 16 16" aria-hidden="true"><path d="M4 6l4 4 4-4" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg>
  </summary>
  <div class="cnt3-expand-body">
    <p class="cnt3-expand-lede">القاعدة الأولى ليست في الميزانية — بل في كيف تنظر إلى المال.
    حين تتعامل مع الدينار الواحد كأنه بذرة، يبدأ كل قرار بالتحوّل من «إنفاق» إلى «استثمار»…</p>
    <a class="cnt3-expand-link" href="https://instagram.com/almobadir_flousak" target="_blank" rel="noopener">متابعة القراءة على إنستغرام <span aria-hidden="true">↗</span></a>
  </div>
</details>
```
```css
.cnt3-expand { margin-top: 14px; border-top: 1px dashed var(--hairline); padding-top: 14px; }
.cnt3-expand-toggle {
  list-style: none; cursor: pointer;
  display: inline-flex; align-items: center; gap: 8px;
  font-family: var(--font-mono); font-size: 12px; letter-spacing: .08em;
  color: var(--accent, var(--crimson));
  background: none; border: 0; padding: 0;
}
.cnt3-expand-toggle::-webkit-details-marker { display: none; }
.cnt3-expand-chev { width: 12px; height: 12px; transition: transform .3s var(--ease-glide); }
.cnt3-expand[open] .cnt3-expand-chev { transform: rotate(180deg); }
.cnt3-expand-body {
  overflow: hidden;
  max-height: 0;
  transition: max-height .5s var(--ease-glide), opacity .35s var(--ease-glide);
  opacity: 0;
}
.cnt3-expand[open] .cnt3-expand-body { max-height: 480px; opacity: 1; padding-top: 12px; }
.cnt3-expand-lede { font-family: var(--font-ar-body); font-size: 16px; line-height: 1.85; color: var(--ink-soft); margin: 0 0 10px; }
.cnt3-expand-link { font-family: var(--font-mono); font-size: 12px; color: var(--accent); text-decoration: none; border-bottom: 1px solid color-mix(in oklab, var(--accent) 35%, transparent); }
.cnt3-expand-link:hover { border-bottom-color: var(--accent); }

@media (prefers-reduced-motion: reduce) {
  .cnt3-expand-body, .cnt3-expand-chev { transition: none; }
}
```

**Reference**: Wave 1 typography REF 11 — It's Nice That article-page editorial pull-quote/expand; interaction REF 13 — Rauno hover-earned reveal pattern (click variant for touch).
**Effort**: ~2hr (per-card lede copy adds editorial work; shipping mechanism alone is ~45min).
**RTL safe**: yes — `<details>`/`<summary>` honor inline-direction; chevron rotation is angle-symmetric.
**prefers-reduced-motion**: instant snap-open instead of height-tween; native `<details>` already supports this; transitions short-circuited via media query.

---

## MOVE 8: View-Toggle (LIST / GRID / TIMELINE) — Live Specimen for the Network

**The moment**: A small mono-font segmented control above the grid lets the reader switch viewport: **GRID** (current 1-featured + 2-standard layout), **LIST** (compact rows with handle, title, date, read-time), **TIMELINE** (vertical chronological feed with month dividers). Switch is animated via View Transitions API where supported.

**Where**: New `.cnt3-views` block above `.cnt3-grid` @ `index.html:867`. Toggling adds `data-view="list"` / `data-view="timeline"` to the section root.

**Implementation**:
```html
<!-- after .cnt3-head, before .cnt3-grid -->
<div class="cnt3-views" role="tablist" aria-label="طريقة عرض المحتوى">
  <button class="cnt3-view is-active" role="tab" aria-selected="true" data-view="grid">شبكة</button>
  <button class="cnt3-view" role="tab" aria-selected="false" data-view="list">قائمة</button>
  <button class="cnt3-view" role="tab" aria-selected="false" data-view="timeline">خط زمني</button>
</div>
```
```css
.cnt3-views { display: inline-flex; gap: 4px; padding: 4px; border: 1px solid var(--hairline); border-radius: 999px; background: rgba(245,234,210,.02); margin-bottom: 28px; font-family: var(--font-mono); }
.cnt3-view { background: transparent; border: 0; color: var(--ink-mute); font-size: 12px; letter-spacing: .12em; padding: 8px 14px; border-radius: 999px; cursor: pointer; transition: background .25s var(--ease-glide), color .25s var(--ease-glide); }
.cnt3-view:hover { color: var(--ink-2); }
.cnt3-view.is-active { background: var(--ink); color: var(--void); }
.cnt3-view:focus-visible { outline: 2px solid var(--crimson); outline-offset: 3px; }

[data-view="list"] .cnt3-grid { display: flex; flex-direction: column; gap: 0; max-width: 920px; margin-inline: auto; }
[data-view="list"] .cnt3-card { display: grid; grid-template-columns: 80px 1fr auto; gap: 18px; padding: 18px 8px; border: 0; border-bottom: 1px solid var(--hairline); border-radius: 0; background: transparent; }
[data-view="list"] .cnt3-cover { aspect-ratio: 1; height: 64px; width: 64px; border-radius: 12px; }
[data-view="list"] .cnt3-tag, [data-view="list"] .cnt3-cover-meta, [data-view="list"] .cnt3-excerpt, [data-view="list"] .cnt3-accent-bar { display: none; }
[data-view="list"] .cnt3-headline { font-size: 18px; -webkit-line-clamp: 2; }

[data-view="timeline"] .cnt3-grid { display: block; max-width: 760px; margin-inline: auto; padding-inline-start: 32px; border-inline-start: 1px solid var(--hairline); }
[data-view="timeline"] .cnt3-card { position: relative; margin-block-end: 32px; background: transparent; border: 0; border-radius: 0; }
[data-view="timeline"] .cnt3-card::before { content: ''; position: absolute; inset-inline-start: -38px; top: 24px; width: 12px; height: 12px; border-radius: 50%; background: var(--accent); box-shadow: 0 0 0 4px var(--void); }
```
```js
const viewBtns = root.querySelectorAll('.cnt3-view');
const setView = (v) => {
  root.dataset.view = v;
  viewBtns.forEach(b => {
    const on = b.dataset.view === v;
    b.classList.toggle('is-active', on);
    b.setAttribute('aria-selected', on ? 'true' : 'false');
  });
  try { localStorage.setItem('cnt3-view', v); } catch (_) {}
};
viewBtns.forEach(b => b.addEventListener('click', () => {
  const v = b.dataset.view;
  if (document.startViewTransition && !reduceMotion) {
    document.startViewTransition(() => setView(v));
  } else {
    setView(v);
  }
}));
try { const saved = localStorage.getItem('cnt3-view'); if (saved) setView(saved); } catch (_) {}
```

**Reference**: Wave 1 motion REF 20 — View Transitions API native morph (Chrome 111+); typography REF 12 — Are.na content-first multi-density toggle.
**Effort**: ~3hr.
**RTL safe**: yes — `inset-inline-start`, `border-inline-start`, and `padding-inline-start` flip cleanly; the timeline rail will sit on the visual right in RTL.
**prefers-reduced-motion**: View Transitions API is bypassed; layout snaps without crossfade. State, content, and ordering preserved.

---

## Top 3 (ranked by punch ÷ effort)

1. **MOVE 1 — Sticky-Card Deck Stack.** Single highest-impact move on the section. Reframes "three articles" into "an editorial deck being dealt out one spread at a time." Pairs perfectly with the editorial-volume fix (6 cards) from `02-content.md`. RTL-clean, ~4hr, zero JS dependencies, full reduced-motion fallback. Ship first.
2. **MOVE 4 — Aramco-Style "العدد ١٨٤" Oversized Numeral.** The single typographic gesture that turns "competent grid" into "Arab publication front page." Replaces the imported `№` glyph with an Eastern-Arabic numeral set in the display face. ~1.5hr. Lowest risk, biggest cultural-credibility return on the page.
3. **MOVE 2 — Spring-Lift + Saturated Brand-Color Shadow.** Repairs the audit's "27 distinct durations" finding inside this section by collapsing 3 hover transitions into one coordinated spring. Adds the colored shadow as a direct extension of the existing `--accent` token system. ~1hr. Every hover on the page benefits from this `--ease-spring` token; this section is the showroom for it.

---

## Implementation Order (suggested batching)

| Day | Moves | Hours | Outcome |
|---|---|---|---|
| 1 | MOVE 4 (oversized numeral) + MOVE 2 (spring-lift) + MOVE 5 (relative-time) | ~3.5 | Editorial signature + crafted hover + live recency. Section already feels different. |
| 2 | MOVE 1 (sticky stack) + MOVE 3 (digit roll) | ~5.5 | Marquee scroll moment + tabular animation. Section now "screenshot-able." |
| 3 | MOVE 6 (Ken Burns) + MOVE 7 (in-page lede) + MOVE 8 (view toggle) | ~6.5 | Photo cinematics + reader payoff + density flexibility. Section reaches publication-grade. |

Total all eight: ~15.5 engineering hours. Top three alone: ~6.5 hours.

---

## Deferred (intentionally not shipped)

- **REF 6 / X-Ray Alt-key reveal** — better fit for the Founder section (already proposed elsewhere); would clutter the editorial card.
- **REF 7 / Cursor-as-Character** — too playful for an editorial publication; reserve for case-study pages.
- **Card stripe animating through brand palette on first reveal** (suggested input) — superseded by the per-card `--accent` already in HTML; animating it dilutes the "this card belongs to this sub-brand" signal. Keep the stripe deterministic.
