## Wave 2 WOW — METHOD section (.mtd3)

**Compiled:** 2026-04-26
**Scope:** 5-frame scroll-pinned scrollytelling. Already cinematic; Commit 2 fixed the icon-numeral collision and HUD occlusion. This document proposes 8 cinematic moves that deepen the choreography without breaking it.
**Source:** `website/index.html:480-852` · `website/assets/v3.css:407-597` · `website/assets/v3.js:175-283`
**References:** `design-audit/wow-research-{arabic,motion,typography,interaction}.md`

---

## Executive Summary (200 words)

The METHOD section is the most ambitious part of the page and, after Commit 2, the choreography finally works the way the comments promised. What's missing now is the layer of *editorial cinema* that separates "scrollytelling demo" from "publication that takes itself seriously." The eight moves below add that layer.

Three are signature: a single SVG numeral path that draws across all five frames as one continuous gesture (the move that turns the section from "5 cards" into "one film reel"); a frame-to-frame color crossfade that interpolates `--mtd3-c` through every adjacent palette pair so cuts become dissolves; and a sticky "current step" pill that slides above the HUD as a magnet for the eye's wayfinding. These are the screenshots people will share.

Three are texture: per-frame paper-grain shift, page-turn shadow under the sliding frame, and `wdth` axis pulsing on the giant outlined numerals. They make every frame feel materially different without changing the layout.

Two are content-prep: hand-drawn editorial illustrations replacing the 5 generic SVGs (single illustrator, one style), and the frame-4 magazine cover becoming a 3D card stack of real Almobadir spreads (when commissioned). Each has a lo-fi shippable interim. All eight respect the existing `prefers-reduced-motion` fallback (frames stack vertically) and none alter the scroll math the choreography depends on.

---

## MOVE 1: Continuous SVG numeral path drawing across all 5 frames

**The moment**: The user reads not 5 separate "01-05" but a single 4500px-tall hand-lettered Arabic-Eastern numeral string that draws itself across the entire pinned stage as they scroll — the section's signature gesture.

**Where**: `website/index.html:506` (`.mtd3__track`), `website/assets/v3.css:438` (track), `website/assets/v3.js:248` (after the sub computation in `update()`).

**Implementation**:
```html
<!-- Add INSIDE .mtd3__track, BEFORE the 5 frames: -->
<svg class="mtd3__numeral-thread" viewBox="0 0 1320 4500" aria-hidden="true" preserveAspectRatio="xMidYMid slice">
  <!-- Single path: stylized eastern-Arabic ٠١ ٠٢ ٠٣ ٠٤ ٠٥ as one continuous gesture -->
  <path id="mtd3-thread"
        d="M 1180 240 Q 1100 380 1180 540 ... [ARTIST-DRAWN, 5 numerals connected]"
        fill="none"
        stroke="var(--mtd3-c, #E6213B)"
        stroke-width="1.4"
        stroke-linecap="round"
        pathLength="1"
        style="stroke-dasharray:1; stroke-dashoffset: var(--mtd3-thread-offset, 1); transition: stroke 600ms var(--ease-glide);"/>
</svg>
```
```css
/* v3.css — append after line 438 */
.mtd3__numeral-thread { position: absolute; inset: 0; width: 100%; height: 100%;
  pointer-events: none; z-index: 0; opacity: .42; mix-blend-mode: screen; }
@media (max-width: 900px) { .mtd3__numeral-thread { display: none; } }
@property --mtd3-thread-offset { syntax: '<number>'; inherits: true; initial-value: 1; }
```
```js
// v3.js — inside update(), after line 247:
const totalProgress = Math.max(0, Math.min(1, scrolled / total));
root.style.setProperty('--mtd3-thread-offset', (1 - totalProgress).toFixed(4));
```

**Reference**: Wave 1 motion REF 13 (Copilot perspective-grid `stroke-dashoffset` reveal) — https://github.com/features/copilot — combined with Wave 1 arabic REF 14 (Iranian vertical kashida-strip marginalia).

**Effort**: ~6hr (3hr illustrator commission for the connected numeral path, 2hr engineering, 1hr palette/blend tuning).

**RTL safe**: Yes — path is drawn to read top-down right-edge, which is correct for RTL flow. Decorative; no sign-flip needed.

**prefers-reduced-motion**: Show the path fully drawn at 18% opacity, no scroll binding. Already covered by the existing reduced-motion branch in `v3.js:203` that flattens frames.

**Risk**: The path SVG must be authored against the actual stage height (5×100vh) — if `--hdr-h` or HUD margin changes, the numeral positions drift. Mitigation: use percentage-based path coordinates and one SVG per frame with `viewBox="0 0 100 100"` if commissioning per-frame is preferred.

---

## MOVE 2: Frame-to-frame color crossfade (--mtd3-c interpolation)

**The moment**: Crimson doesn't *cut* to verde — it *bleeds* through olive on the way. Every glow, accent rule, kicker bar, numeral stroke and progress fill interpolates through 240ms during the sub→0 of the next frame.

**Where**: `website/assets/v3.css:447-452` (frame `--mtd3-c` declarations), `website/assets/v3.js:191-201` (`setActive`).

**Implementation**:
```css
/* v3.css — replace lines 447-452 */
@property --mtd3-c { syntax: '<color>'; inherits: true; initial-value: #E6213B; }
.mtd3__frame { transition: --mtd3-c 240ms var(--ease-glide); }
.mtd3__frame[data-color="crimson"] { --mtd3-c: #E6213B; }
.mtd3__frame[data-color="verde"]   { --mtd3-c: #3CC58B; }
.mtd3__frame[data-color="gold"]    { --mtd3-c: #E8B855; }
.mtd3__frame[data-color="azure"]   { --mtd3-c: #5AA8FF; }
.mtd3__frame[data-color="teal"]    { --mtd3-c: #1FB6A6; }

/* HUD progress fill — interpolate via blended gradient driven by --mtd3-sub */
.mtd3__progress-fill { transition: width 550ms var(--ease-glide), background 240ms var(--ease-glide); }
```
```js
// v3.js — replace setActive() lines 191-201:
const setActive = (idx) => {
  const i = Math.max(0, Math.min(frames.length - 1, idx));
  if (i === lastIdx) return;
  lastIdx = i;
  frames.forEach((f, j) => f.classList.toggle('is-active', j === i));
  if (current) current.textContent = arabicNumeral(i + 1);
  if (fill) {
    fill.style.width = `${(i + 1) * (100 / frames.length)}%`;
    // Interpolate fill gradient: previous → current → next color triad
    const prev = colors[Math.max(0, i - 1)];
    const next = colors[Math.min(colors.length - 1, i + 1)];
    fill.style.background = `linear-gradient(90deg, ${prev} 0%, ${colors[i]} 50%, ${next} 100%)`;
  }
};
```

**Reference**: Wave 1 motion REF 14 (Notion three-tier duration discipline — 240ms is the "cards/forms" tier, perfect for a chromatic transition that's neither instant nor cinematic). Backed by Wave 1 typography REF 12 (Are.na restraint: same family/object, gradient through it, not new noise).

**Effort**: ~2hr (CSS `@property` registration, JS gradient triad, regression on the 4 frame-pair transitions).

**RTL safe**: Yes — color, not direction. Decorative.

**prefers-reduced-motion**: Skip the 240ms transition; instant color swap (current behavior).

**Risk**: `@property --mtd3-c` with `<color>` syntax has full support on Chromium 99+ and Safari 16.4+. Firefox 128+ supports it as of March 2025 but older Firefox falls back to instant cut — graceful, not broken.

---

## MOVE 3: Sticky "current step" pill above the HUD

**The moment**: Above the HUD, at the top of the pinned viewport, a single pill that reads `٠٢ · تبسيط` slides between positions matching the rail steps as the user scrolls — the closest the page comes to a "you are here" beacon.

**Where**: `website/index.html:489` (after `<header class="mtd3__hud">`), `website/assets/v3.css:425` (HUD), `website/assets/v3.js:191` (`setActive`).

**Implementation**:
```html
<!-- index.html: insert AFTER line 488, BEFORE the <header class="mtd3__hud">: -->
<div class="mtd3__step-pill" aria-live="polite" aria-atomic="true">
  <span class="mtd3__step-pill-num">٠١</span>
  <span class="mtd3__step-pill-sep">·</span>
  <span class="mtd3__step-pill-label">اختيار</span>
</div>
```
```css
/* v3.css — append in the @media (min-width: 901px) block near line 510 */
.mtd3__step-pill {
  position: absolute; inset-block-start: calc(var(--hdr-h, 104px) - 18px); inset-inline-start: 50%;
  transform: translateX(-50%);
  display: inline-flex; align-items: center; gap: 10px;
  padding: 8px 16px; font-family: var(--font-mono); font-size: 11px; letter-spacing: .24em;
  text-transform: uppercase; color: var(--mtd3-c, var(--ink));
  background: rgba(10,15,30,.92); backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border: 1px solid color-mix(in oklab, var(--mtd3-c, var(--hairline)) 35%, transparent);
  border-radius: var(--r-pill); z-index: 6;
  transition: color 240ms var(--ease-glide), border-color 240ms var(--ease-glide);
  box-shadow: 0 8px 32px -12px rgba(0,0,0,.6), 0 0 0 1px rgba(255,255,255,.04) inset;
}
.mtd3__step-pill-num { font-feature-settings: "tnum" 1; opacity: .9; }
.mtd3__step-pill-sep { opacity: .4; }
.mtd3__step-pill-label { font-family: var(--font-ar-display); letter-spacing: 0; font-size: 13px; }
@media (max-width: 900px) { .mtd3__step-pill { display: none; } }
```
```js
// v3.js — inside setActive(), append after line 200:
const pill = root.querySelector('.mtd3__step-pill');
if (pill) {
  const labels = ['اختيار','تبسيط','سرد','تصميم','إصغاء'];
  pill.querySelector('.mtd3__step-pill-num').textContent = arabicNumeral(i + 1);
  pill.querySelector('.mtd3__step-pill-label').textContent = labels[i];
}
```

**Reference**: Wave 1 motion REF 7 (Supabase Launch Week sticky "TODAY" pill on the timeline rail) — https://supabase.com/launch-week — and Wave 1 motion REF 10 (Linear's micro-stagger for pill text refresh).

**Effort**: ~2hr (markup + CSS pill + JS label sync; verify against the existing HUD's z-index stack).

**RTL safe**: Yes — `inset-inline-start: 50%` and `translateX(-50%)` produce identical centering in RTL.

**prefers-reduced-motion**: Pill renders static at the top centered; no `transition` triggers since `setActive` is invoked once at init.

**Risk**: Pill must sit above the HUD's z-index (5) but below modal layers — set explicitly to 6. Verify it doesn't collide with the global `.hdr3` sticky header at scroll position 0; the `--hdr-h` offset above accounts for that.

---

## MOVE 4: Page-turn shadow under the entering frame

**The moment**: As a frame slides in, a soft directional shadow precedes it from the inside-edge, like a fresh page lifting from beneath the previous one. Adds 8px of paper-stack physics to a section that is currently weightless.

**Where**: `website/assets/v3.css:514-516` (`.mtd3__frame` desktop transition rules).

**Implementation**:
```css
/* v3.css — replace .mtd3__frame transition declaration around line 514 */
.mtd3__frame {
  position: absolute; inset: 0; min-height: 0;
  display: grid; grid-template-columns: minmax(0,1.15fr) minmax(0,1fr);
  align-items: center; padding: clamp(48px,6vw,96px) clamp(48px,8vw,140px);
  gap: clamp(48px,6vw,96px); background: transparent;
  opacity: 0; visibility: hidden;
  transform: translateY(36px) scale(.985);
  transition: opacity .7s var(--ease-glide), transform .8s var(--ease-glide), visibility 0s linear .7s;
  pointer-events: none; will-change: opacity, transform;
}
.mtd3__frame::before {
  /* Page-turn shadow: rises with the frame, peaks at sub≈0.2, fades by sub≈0.6 */
  content: ''; position: absolute; inset-inline: -2vw; inset-block-start: -32px;
  height: 64px; pointer-events: none; z-index: 1;
  background: linear-gradient(180deg,
    rgba(0,0,0,.42) 0%,
    rgba(0,0,0,.18) 35%,
    rgba(0,0,0,0) 100%);
  filter: blur(18px);
  opacity: clamp(0, calc((var(--mtd3-sub, 0)) * 3.5 - calc(var(--mtd3-sub, 0) * var(--mtd3-sub, 0) * 5)), 0.85);
  transform: translateY(calc((1 - clamp(0, calc(var(--mtd3-sub, 0) * 2.2), 1)) * -16px));
}
```

**Reference**: Wave 1 motion REF 18 (Framer's sticky-card stack — drop-shadow plus scale to fake depth without WebGL) — https://framer.com — adapted for a single-frame entry instead of a card deck.

**Effort**: ~1.5hr (the shadow gradient is trivial; tuning the opacity curve to peak at sub≈0.2 is the work).

**RTL safe**: Yes — `inset-inline` is direction-aware. Decorative shadow has no semantic side.

**prefers-reduced-motion**: `::before` opacity stays at 0 (the curve evaluates to 0 because `--mtd3-sub` is never set in reduced-motion branch). Already covered.

**Risk**: The pseudo-element sits above the frame background (z:1) but below copy (z:2). Verify the `.mtd3__num` outlined numeral (z:0) doesn't get clipped — it shouldn't, the shadow is positioned `inset-block-start: -32px` so it extends *above* the frame's content box.

---

## MOVE 5: SF Pro `wdth` axis pulse on the giant outlined numerals

**The moment**: The 220px outlined "01"–"05" don't sit static — they breathe. Width axis pulses 95→112 over the frame's `--mtd3-sub` 0→1, then settles at 105. The numeral exhales as the copy enters.

**Where**: `website/assets/v3.css:453` (`.mtd3__num`).

**Implementation**:
```css
/* v3.css — replace .mtd3__num declaration around line 453 */
@property --mtd3-num-wdth { syntax: '<number>'; inherits: true; initial-value: 95; }
.mtd3__num {
  position: absolute; inset-block-start: 50%; inset-inline-end: 4%;
  transform: translateY(-50%) scale(0.7);
  font-family: "Roboto Flex", var(--font-mono);
  font-variation-settings: "wdth" var(--mtd3-num-wdth, 95), "wght" 580, "opsz" 144;
  font-size: clamp(140px,16vw,220px); line-height: .85;
  color: transparent; -webkit-text-stroke: 1.5px var(--mtd3-c, #E6213B);
  letter-spacing: -.04em; user-select: none; z-index: 0; opacity: 0;
  transition: opacity 1.1s var(--ease-glide), transform 1.1s var(--ease-glide);
}
.mtd3__frame.is-active .mtd3__num {
  opacity: .55; transform: translateY(-50%) scale(1);
  /* width axis follows sub: 95 → 112 (peak) → 105 (settle) */
  --mtd3-num-wdth: calc(95 + var(--mtd3-sub, 0) * 17 - max(0, (var(--mtd3-sub, 0) - .6)) * 17.5);
  transition-delay: 0.1s;
}
```
```html
<!-- index.html — add to <head>, near line 14: -->
<link rel="preload" href="https://fonts.gstatic.com/s/robotoflex/v27/NaN4epOXO_NexZs0b5QrzlOHb8wCikXpYqmZsWI-__OGgsh3.woff2" as="font" type="font/woff2" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Roboto+Flex:opsz,wdth,wght@8..144,25..151,100..1000&display=swap" rel="stylesheet">
```

**Reference**: Wave 1 typography REF 7 (Apple WWDC SF Pro `wdth` axis on hero year — same numeral pinned wide for editorial gravity) — https://developer.apple.com/wwdc/ — using free open analog **Roboto Flex** which has the `wdth` axis 25–151 baked in.

**Effort**: ~2hr (font load + `@property` + curve tuning; cache-bust check).

**RTL safe**: Yes — the latin numerals "01-05" are decorative; the rail and progress already render eastern-Arabic ٠١-٠٥. Numerals have `direction: ltr` implicitly via Roboto Flex.

**prefers-reduced-motion**: Numeral renders at static `wdth: 105`, no animation. Wrap the calc in `@media (prefers-reduced-motion: no-preference)` block or rely on the existing branch in `v3.js:203` that flattens `--mtd3-sub`.

**Risk**: Roboto Flex `.woff2` adds ~28KB compressed; verify the existing brand display face (Mostaqbali / Cairo Variable) doesn't conflict. If brand rejects Latin sans for the editorial numerals, swap to Cairo Variable's `wght` axis (200→880) for an analogous pulse — same move, Arabic-native font.

---

## MOVE 6: Per-frame paper-grain shift

**The moment**: Each of the 5 frames carries a different paper-grain texture: cold press for crimson (rough), hot press for verde (smooth), kraft for gold (warm), cold press 2 for azure, and rice-paper for teal. As frames cross-dissolve, so do the grains. The bg never feels generic.

**Where**: `website/assets/v3.css:447-452` (frame `--mtd3-c` block — extend to grain), `website/assets/v3.css:411` (existing grid texture on `::after`).

**Implementation**:
```css
/* v3.css — extend frame data-color blocks */
.mtd3__frame[data-color="crimson"] { --mtd3-c: #E6213B; --mtd3-grain: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="240" height="240"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.85" numOctaves="2"/><feColorMatrix values="0 0 0 0 .89 0 0 0 0 .85 0 0 0 0 .79 0 0 0 .12 0"/></filter><rect width="240" height="240" filter="url(%23n)"/></svg>'); }
.mtd3__frame[data-color="verde"]   { --mtd3-c: #3CC58B; --mtd3-grain: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="240" height="240"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="1.4" numOctaves="1"/><feColorMatrix values="0 0 0 0 .89 0 0 0 0 .85 0 0 0 0 .79 0 0 0 .08 0"/></filter><rect width="240" height="240" filter="url(%23n)"/></svg>'); }
.mtd3__frame[data-color="gold"]    { --mtd3-c: #E8B855; --mtd3-grain: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="240" height="240"><filter id="n"><feTurbulence type="turbulence" baseFrequency="0.6" numOctaves="3"/><feColorMatrix values="0 0 0 0 .89 0 0 0 0 .85 0 0 0 0 .79 0 0 0 .14 0"/></filter><rect width="240" height="240" filter="url(%23n)"/></svg>'); }
.mtd3__frame[data-color="azure"]   { --mtd3-c: #5AA8FF; --mtd3-grain: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="240" height="240"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.95" numOctaves="2"/><feColorMatrix values="0 0 0 0 .89 0 0 0 0 .85 0 0 0 0 .79 0 0 0 .10 0"/></filter><rect width="240" height="240" filter="url(%23n)"/></svg>'); }
.mtd3__frame[data-color="teal"]    { --mtd3-c: #1FB6A6; --mtd3-grain: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="240" height="240"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="2.1" numOctaves="1"/><feColorMatrix values="0 0 0 0 .89 0 0 0 0 .85 0 0 0 0 .79 0 0 0 .07 0"/></filter><rect width="240" height="240" filter="url(%23n)"/></svg>'); }

.mtd3__frame::after {
  content: ''; position: absolute; inset: 0; pointer-events: none; z-index: 0;
  background-image: var(--mtd3-grain);
  background-size: 240px 240px;
  mix-blend-mode: overlay;
  opacity: 0;
  transition: opacity 320ms var(--ease-glide);
}
.mtd3__frame.is-active::after { opacity: .55; }
```

**Reference**: Wave 1 arabic REF 5 (Diriyah City typeface "manuscript-to-wordmark" — vellum texture as semantic register) — https://www.atrissi.com/diriyah-city-typeface-saudi-arabia/ — and Wave 1 motion REF 5 (Stripe minigl pattern — programmatic noise as ambient layer, but in a CSS-only key).

**Effort**: ~3hr (grain SVG curation per palette + opacity matching — the opacity coefficients above are eyeballed; need on-screen validation against the actual `--paper` ink color).

**RTL safe**: Yes — texture has no direction.

**prefers-reduced-motion**: The opacity transition is a 320ms cross-fade, which is inside the WCAG 2.3.3 "small motion" carve-out. To be conservative, wrap the `::after` opacity in `@media (prefers-reduced-motion: no-preference)`.

**Risk**: Inline SVG-as-background with `feTurbulence` is supported on all evergreen browsers and Safari 14+, but the same baseFrequency renders slightly differently between Chromium and Gecko — accept ±5% variance. Don't ship at >0.55 opacity or the grain becomes the figure instead of the ground.

---

## MOVE 7: Replace 5 SVG icons with hand-drawn editorial illustrations

**The moment**: Frames 1–5 stop relying on generic SVG (radar, mess/clean abstraction, film reel, magazine cover, broadcast tower). One illustrator, one ink-on-cream style, five custom plates that each tell their step's verb in a single image — a hallmark of editorial seriousness.

**Where**: `website/index.html:520-596,607-660,670-753,763-774,784-844` (each `<div class="mtd3__art">` block).

**Implementation**:
```html
<!-- Replace contents of each .mtd3__art div with a single <img> -->
<!-- Frame 1: index.html:520 -->
<div class="mtd3__art mtd3__art--illustrated" aria-hidden="true">
  <img src="/assets/illustrations/mtd3-01-select.svg" alt="" loading="lazy"
       class="mtd3__illustration" width="400" height="400">
  <span class="mtd3__art-tag">TARGETING · ٠١</span>
</div>
<!-- Repeat for frames 2-5 with mtd3-02-simplify.svg, mtd3-03-narrate.svg,
     mtd3-04-design.svg, mtd3-05-listen.svg -->
```
```css
/* v3.css — append */
.mtd3__art--illustrated { aspect-ratio: 1/1; }
.mtd3__illustration {
  width: 100%; height: 100%; max-width: 280px; object-fit: contain;
  filter: drop-shadow(0 24px 48px rgba(0,0,0,.45));
  transition: filter 320ms var(--ease-glide), transform 800ms var(--ease-glide);
}
.mtd3__frame.is-active .mtd3__illustration {
  /* Subtle ink-bloom on entry: scale+brightness lift via sub */
  transform: scale(calc(0.98 + var(--mtd3-sub, 0) * 0.04));
  filter: drop-shadow(0 24px 48px rgba(0,0,0,.55))
          drop-shadow(0 0 32px color-mix(in oklab, var(--mtd3-c) 30%, transparent));
}
@media (prefers-reduced-motion: reduce) {
  .mtd3__illustration { transition: none; transform: none !important; }
}
```

**Reference**: Wave 1 arabic REF 7 (Islamic Arts Biennale by Atrissi — single illustrator, one custom logographic system, infinite editorial variation from one type investment) — https://www.atrissi.com/islamic-arts-biennale-branding/ — and Wave 1 typography REF 10 (NYT Magazine cover treatments — illustration as cultural-register marker).

**Effort**: ~14hr total (1hr per illustration brief × 5 = 5hr direction; 8hr illustrator commission for 5 plates in a unified style; 1hr engineering swap). **Lo-fi shippable interim**: keep the existing SVGs but unify their stroke widths (currently inconsistent: 0.9px/1.0px/1.4px/1.5px/1.8px — pick 1.4px global), color (use `var(--mtd3-c)` only, drop the rgba whites that read as "engineer-art"), and grid (snap all to 16-unit grid). 4hr effort and reads 60% as polished.

**RTL safe**: Yes — illustrations are bilateral by intent (the brief is "no embedded text in the image; copy lives in `.mtd3__copy`"). Decorative.

**prefers-reduced-motion**: Static drop-shadow only; no scale on `:hover` or via sub. Already gated.

**Risk**: Illustration commission has a long pipeline. Ship the unified-stroke-width interim as a Phase 1 (4hr); dispatch the commission in parallel for Phase 2 swap. Don't conflate the two; either is independently shippable.

---

## MOVE 8: Frame 4 magazine cover becomes 3D rotating card stack of real Almobadir spreads

**The moment**: At sub≈0.4 of frame 4, the single editorial-placeholder cover ("عنوان يُمسك العين") fans into 3 actual Almobadir issue covers in a 3D card-stack — front card foregrounded, two angled behind. On further sub increment, the stack rotates one position. The reader sees real evidence, not a placeholder.

**Where**: `website/index.html:763-774` (`.mtd3__art--cover` block), `website/assets/v3.css:490-499` (cover styles).

**Implementation**:
```html
<!-- index.html: replace lines 763-774 -->
<div class="mtd3__art mtd3__art--cover-stack" aria-hidden="true" style="--stack-rot: 0">
  <div class="mtd3-stack" style="perspective: 1200px;">
    <div class="mtd3-stack__card mtd3-stack__card--3" style="background-image: url('/assets/covers/almobadir-issue-3.jpg');">
      <span class="mtd3-cover__masthead">المبادر · ٠٣</span>
    </div>
    <div class="mtd3-stack__card mtd3-stack__card--2" style="background-image: url('/assets/covers/almobadir-issue-2.jpg');">
      <span class="mtd3-cover__masthead">المبادر · ٠٢</span>
    </div>
    <div class="mtd3-stack__card mtd3-stack__card--1" style="background-image: url('/assets/covers/almobadir-issue-1.jpg');">
      <span class="mtd3-cover__masthead">المبادر · ٠١</span>
    </div>
  </div>
  <span class="mtd3__art-tag">EDITORIAL · ٠٤</span>
</div>
```
```css
/* v3.css — replace .mtd3-cover blocks */
.mtd3-stack {
  position: relative; width: 100%; aspect-ratio: 3/4; max-width: 380px;
  transform-style: preserve-3d; transform: rotateY(-6deg) rotateX(2deg);
}
.mtd3-stack__card {
  position: absolute; inset: 0; background-size: cover; background-position: center;
  border-radius: 2px; box-shadow: 0 50px 120px -40px rgba(0,0,0,.7), 0 24px 60px -30px rgba(90,168,255,.35);
  transition: transform 700ms var(--ease-glide), opacity 500ms var(--ease-glide);
  display: flex; flex-direction: column-reverse; padding: 18px;
}
.mtd3-stack__card .mtd3-cover__masthead {
  font-family: var(--font-ar-display); font-weight: 900; color: var(--paper); font-size: 22px;
  letter-spacing: -.02em; mix-blend-mode: difference;
}
.mtd3-stack__card--1 { transform: translate3d(0, 0, 0) rotate(-2deg); z-index: 3; }
.mtd3-stack__card--2 { transform: translate3d(14px, -8px, -40px) rotate(1.5deg); z-index: 2; opacity: .9; }
.mtd3-stack__card--3 { transform: translate3d(28px, -16px, -80px) rotate(4deg); z-index: 1; opacity: .7; }

/* Sub-driven rotation: at sub > .55 fan the cards apart further */
.mtd3__frame[data-color="azure"].is-active .mtd3-stack__card--1 {
  transform: translate3d(calc(var(--mtd3-sub, 0) * -20px), 0, 0) rotate(calc(-2deg + var(--mtd3-sub, 0) * -3deg));
}
.mtd3__frame[data-color="azure"].is-active .mtd3-stack__card--2 {
  transform: translate3d(calc(14px + var(--mtd3-sub, 0) * 12px), -8px, -40px) rotate(calc(1.5deg + var(--mtd3-sub, 0) * 1.5deg));
}
.mtd3__frame[data-color="azure"].is-active .mtd3-stack__card--3 {
  transform: translate3d(calc(28px + var(--mtd3-sub, 0) * 24px), -16px, -80px) rotate(calc(4deg + var(--mtd3-sub, 0) * 2deg));
}
@media (prefers-reduced-motion: reduce) {
  .mtd3-stack { transform: none; }
  .mtd3-stack__card { transition: none; }
  .mtd3-stack__card--2, .mtd3-stack__card--3 { display: none; }
}
```

**Reference**: Wave 1 motion REF 18 (Framer sticky-card stack — perspective + scale to fake depth without WebGL) — https://framer.com — combined with Wave 1 interaction REF 4 (Stripe Press book tilt — physical-feeling cards).

**Effort**: ~5hr engineering once 3 cover images exist. **Asset gate**: needs 3 finished issue cover JPGs at 600×800 (≈80KB each WebP). Until commissioned, ship the Phase 1 fallback below — same code, but the 3 cards are the same editorial placeholder rendered from `.mtd3-cover` markup at three z-positions, with Lorem-ipsum-quality cover lines replaced by real headline drafts from the editorial team. 2hr work and reads 70% as polished.

**RTL safe**: Yes — `transform: translate3d(Xpx, ...)` uses physical X. In RTL context the deck appears to fan toward the *left* (visually toward the body copy on the right), which is correct: cards fan toward the reader's eye. If the editorial team prefers them fanning rightward, swap the X signs.

**prefers-reduced-motion**: Show only the front card (cards 2 and 3 hidden). Already gated.

**Risk**: 3D-perspective transforms compose with the existing `.mtd3__frame.is-active` `transform: translateY(36px) scale(.985)` — there could be jank at the moment of frame entry when both transforms compete. Mitigate by isolating the stack in its own stacking context (`isolation: isolate` on `.mtd3__art--cover-stack`). Smoke-test on iOS Safari 17 — older WebKit struggles with `preserve-3d` plus `mix-blend-mode`.

---

## Ranked Top 3

### #1 — MOVE 1: Continuous SVG numeral path (the signature)
This is the move people will screenshot. It transforms the section's metaphor from "5 stacked frames" to "one continuous editorial gesture." Effort is moderate (illustrator + 2hr engineering). Risk is contained (path drawn once at fixed coordinates, no scroll math touched). The eastern-Arabic ٠١→٠٥ flowing as one ink stroke is unmistakably Arab-editorial — Apple PDPs can't ship this; it's ours. Lands most strongly because no other competitor in the audit set (Stripe Press, Linear, Bloomberg long-form) has script-native numeral choreography.

### #2 — MOVE 2: Frame-to-frame color crossfade
The cheapest 10/10-quality-bar move on the list. 2 hours, ~4 lines of CSS via `@property`, instant perceptual difference: cuts become dissolves, the pinned stage stops feeling like 5 slides and starts feeling like one continuous strip of film. Makes every other move below it look better. Ship this even if nothing else from this document lands.

### #3 — MOVE 3: Sticky "current step" pill
The audit's HUD-occlusion fix in Commit 2 freed the wayfinding layer; this move makes the wayfinding *cinematic*. The pill is the equivalent of a film's chapter card — a tiny editorial signal that earns the 4500px scroll budget. Costs 2hr. Reads as "this team is rigorous" without flashing "look at our scrollytelling." Pairs symbiotically with MOVES 1 and 2 (the pill's color animates through the same crossfade triad).

---

## Source files referenced

- `website/index.html:480-852` (`.mtd3` block + 5 frames + HUD)
- `website/assets/v3.css:407-597` (`.mtd3__*` desktop + mobile)
- `website/assets/v3.js:175-283` (IIFE with `setActive`, `setupDesktop`, `setupMobile`)
- `design-audit/wow-research-arabic.md` (REF 5, 7, 14)
- `design-audit/wow-research-motion.md` (REF 7, 10, 13, 14, 18)
- `design-audit/wow-research-typography.md` (REF 7, 10, 12)
- `design-audit/wow-research-interaction.md` (REF 4)
- `design-audit/02-method.md` (Wave 2 audit, Commit 2 fixes for icon-numeral and HUD-occlusion)
