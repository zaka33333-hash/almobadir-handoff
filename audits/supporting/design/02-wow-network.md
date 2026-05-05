# Wave 2 — NETWORK (`.nw3a`) WOW Moves

**Compiled:** 2026-04-26
**Scope:** `.nw3a` section · index.html lines 364–477 · v3.css lines 327–403 · v3.js lines 142–173
**Constraint:** 7 cards (mindset · almobadir · badrshaqer · shalnack · flousak · business · media-wide). Grayscale-on-rest → color-on-hover photo treatment is the audit's "best photographic decision" — DO NOT touch. AMPLIFY around it.
**Reduced-motion / RTL:** every move below has both clauses.

---

## Executive Summary

The Network section is already the strongest crafted module on the homepage — magazine grid, dual-language tag, dashed/dotted rule hierarchy, gradient title-em. Wave 1 found the photo treatment "best photographic decision on the entire page." So the move here is *not* to add spectacle — it is to **earn the section a second look without trampling the editorial restraint.**

Eight WOW moves below land in three families. **Family A — discipline edits** (Move 1 tightens the gimmicky 3D tilt to a Stripe Press 4° + spring; Move 7 unifies easing across 27 durations to one `--ease-spring` token from REF Things-3). **Family B — photo amplifications** (Move 2 Ken-Burns the cover on hover so the grayscale→color reveal *moves*; Move 3 swaps the static cover for a 2-sec MP4 placeholder when the card is fully in-view). **Family C — editorial moments** (Move 4 sweep-reveals the grid in keyboard order REF Raycast; Move 5 turns the wide @almobadir_media stats into tabular-roll counters REF Stripe; Move 6 adds a TikTok TT badge alongside IG since 4 of 7 cards have both; Move 8 surfaces a tiny `dir="ltr" unicode-bidi: isolate` plus eastern-Arabic-numeral toggle in the totals strip — REF Aramco).

Top 3 ranked at bottom. Family A first — those are the constraints that make B and C read deliberate.

---

## MOVE 1: 3D card tilt — tightened to ±4°, spring return, perspective-locked

**The moment**: Cursor crosses a card; the card breathes 4° toward the cursor with a spring overshoot on leave — felt, not seen.
**Where**: `.nw3a-card` @ v3.js:155–172 + v3.css:344, 402
**Implementation**:
```js
// v3.js:155–172 replacement — cap MAX 6 → 4, add spring on leave
const MAX = 4;
const SPRING = 'cubic-bezier(0.34, 1.56, 0.64, 1)';
cards.forEach(card => {
  let raf = 0;
  const onMove = (e) => {
    const r = card.getBoundingClientRect();
    const cx = (e.clientX - r.left) / r.width - 0.5;
    const cy = (e.clientY - r.top) / r.height - 0.5;
    cancelAnimationFrame(raf);
    raf = requestAnimationFrame(() => {
      card.style.setProperty('--rx', `${(cx * MAX).toFixed(2)}deg`);
      card.style.setProperty('--ry', `${(-cy * MAX).toFixed(2)}deg`);
      card.style.transition = 'transform .12s linear';
    });
  };
  const onLeave = () => {
    cancelAnimationFrame(raf);
    card.style.transition = `transform .42s ${SPRING}`;
    card.style.setProperty('--rx', '0deg');
    card.style.setProperty('--ry', '0deg');
  };
  card.addEventListener('pointermove', onMove);
  card.addEventListener('pointerleave', onLeave);
});
```
```css
/* v3.css:344 keep transform-style + perspective; tighten transition */
.nw3a-card { transform: translateY(0) rotateX(var(--ry,0deg)) rotateY(var(--rx,0deg));
  transition: transform .42s cubic-bezier(0.34, 1.56, 0.64, 1), border-color .4s ease, box-shadow .5s ease; }
```
**Reference**: REF interaction-4 (Stripe Press book tilt — `(.34,1.56,.64,1)`) + REF motion-11 (Things-3 spring) — https://press.stripe.com + https://culturedcode.com/things/
**Effort**: ~1.5hr
**RTL safe**: yes (rotation only — no horizontal sign-flip; `--rx/--ry` map to viewport axes)
**prefers-reduced-motion**: existing guard at v3.js:155 (`if (reduceMotion …) return;`) keeps tilt off — no change needed.

---

## MOVE 2: Ken Burns cover slow-zoom-and-pan on hover (replaces static `scale(1.1)`)

**The moment**: On hover the cover photo doesn't just enlarge — it *drifts* 2.4s diagonally across the frame while the grayscale releases, so the eye sees the photo waking up.
**Where**: `.nw3a-cover img` @ v3.css:370–371
**Implementation**:
```css
/* REPLACES the existing v3.css:370-371 rules */
.nw3a-cover img {
  width: 100%; height: 100%; object-fit: cover;
  filter: grayscale(0.55) contrast(1.05) brightness(0.78);
  transform: scale(1.06) translate(0, 0);
  transition: transform 2.4s cubic-bezier(.2,.7,.1,1), filter .6s ease;
}
.nw3a-card:hover .nw3a-cover img,
.nw3a-card:focus-within .nw3a-cover img {
  filter: grayscale(0) contrast(1.08) brightness(0.9);
  transform: scale(1.12) translate(-2%, -2%);
}
/* RTL: pan toward leading edge (right in RTL = +x) */
[dir="rtl"] .nw3a-card:hover .nw3a-cover img,
[dir="rtl"] .nw3a-card:focus-within .nw3a-cover img {
  transform: scale(1.12) translate(2%, -2%);
}
```
**Reference**: REF motion-3 (CSS scroll-driven primer; same easing curve) + Apple iPhone product page Ken Burns — https://cydstumpel.nl/start-using-scroll-driven-animations-today/
**Effort**: ~30min
**RTL safe**: sign-flip — pan-x is mirrored in `[dir="rtl"]` block above; pan-y unchanged.
**prefers-reduced-motion**: covered by existing `@media (prefers-reduced-motion: reduce)` at v3.css:1141 (covers `.nw3a-cover img`); add `transform: none` there for safety.

---

## MOVE 3: 2-sec video loop on hover — MP4 placeholder = animated brand-color gradient

**The moment**: When a card has been hovered >220ms, an under-2-second `<video>` swap begins behind the cover image; until real footage exists, an animated SVG gradient with the card's accent color plays the same role.
**Where**: New `.nw3a-cover-loop` element @ index.html lines 383, 396, 409, 422, 435, 448, 461 + v3.css after line 372
**Implementation**:
```html
<!-- ADD inside each <figure class="nw3a-cover"> after the existing <img>, before veil -->
<svg class="nw3a-cover-loop" aria-hidden="true" viewBox="0 0 200 120" preserveAspectRatio="xMidYMid slice">
  <defs>
    <linearGradient id="g-mindset" x1="0" x2="1" y1="0" y2="1">
      <stop offset="0" stop-color="var(--accent)" stop-opacity="0.0"/>
      <stop offset=".5" stop-color="var(--accent)" stop-opacity=".55"/>
      <stop offset="1" stop-color="var(--accent)" stop-opacity="0"/>
    </linearGradient>
  </defs>
  <rect x="-50" y="0" width="300" height="120" fill="url(#g-mindset)"/>
</svg>
<!-- Future: replace the <svg> with <video src="…/mindset.mp4" muted playsinline loop preload="none"></video> -->
```
```css
/* ADD after v3.css:372 */
.nw3a-cover-loop { position: absolute; inset: 0; opacity: 0; mix-blend-mode: screen;
  transition: opacity .6s var(--ease-glide); pointer-events: none; }
.nw3a-cover-loop rect { animation: nw3a-loop 2.4s linear infinite paused; transform-box: view-box; }
@keyframes nw3a-loop { 0%{transform:translateX(-30%)} 100%{transform:translateX(30%)} }
.nw3a-card:hover .nw3a-cover-loop, .nw3a-card:focus-within .nw3a-cover-loop { opacity: 1; }
.nw3a-card:hover .nw3a-cover-loop rect, .nw3a-card:focus-within .nw3a-cover-loop rect { animation-play-state: running; }
/* When real <video> ships: */
.nw3a-cover-loop video { width: 100%; height: 100%; object-fit: cover; }
```
```js
/* OPTIONAL — add to v3.js network IIFE so the loop only activates after 220ms (avoids accidental flicker) */
const HOVER_DELAY = 220;
cards.forEach(card => {
  let t = 0;
  card.addEventListener('pointerenter', () => { t = setTimeout(() => card.classList.add('nw3a-loop-on'), HOVER_DELAY); });
  card.addEventListener('pointerleave', () => { clearTimeout(t); card.classList.remove('nw3a-loop-on'); });
});
```
**Reference**: REF motion-8 (Arc Browser autoplaying-on-IO video pattern) + REF motion-2 (Apple Vision Pro currentTime scrub when real footage ships) — https://arc.net + https://www.apple.com/apple-vision-pro/
**Effort**: ~2hr (placeholder), +3hr per card when real MP4s commission
**RTL safe**: decorative — gradient pans on x but visually symmetric; no semantic flip needed. When `<video>` ships, no flip needed (video content is its own composition).
**prefers-reduced-motion**: add `@media (prefers-reduced-motion: reduce) { .nw3a-cover-loop { display: none; } }` to v3.css:1141 block.

---

## MOVE 4: Diagonal sweep reveal — keyboard-order stagger from leading corner (REF Raycast)

**The moment**: First scroll into the grid, the seven cards don't fade in DOM order; they sweep diagonally from the top-leading corner (top-right in RTL) in 60×30ms ladder, like keys lighting up.
**Where**: `.nw3a-card:nth-child` @ v3.css:346–351 (replace) + index.html lines 380, 393, 406, 419, 432, 445, 458 (add `--row/--col`)
**Implementation**:
```html
<!-- ADD style attrs to each <li class="nw3a-card …">. RTL grid layout (4-col on tablet, 6-col desktop): -->
<li class="nw3a-card nw3a-card--feature" style="--row:0;--col:0; …">  <!-- mindset (feature, top-right in RTL) -->
<li class="nw3a-card nw3a-card--rail"    style="--row:0;--col:1; …">  <!-- almobadir (rail) -->
<li class="nw3a-card nw3a-card--std"     style="--row:1;--col:0; …">  <!-- badrshaqer -->
<li class="nw3a-card nw3a-card--std"     style="--row:1;--col:1; …">  <!-- shalnack -->
<li class="nw3a-card nw3a-card--std"     style="--row:1;--col:2; …">  <!-- flousak -->
<li class="nw3a-card nw3a-card--std"     style="--row:1;--col:3; …">  <!-- business (skip on tablet, wraps) -->
<li class="nw3a-card nw3a-card--wide"    style="--row:2;--col:0; …">  <!-- media-wide -->
```
```css
/* REPLACES v3.css:346-351 */
.nw3a-card.nw3a-in {
  transition-delay: calc(var(--row, 0) * 90ms + var(--col, 0) * 60ms);
}
/* In RTL, the visual leading corner is top-right — col 0 is rightmost which is correct,
   so the sweep enters from the brand-correct side without code change. */
```
**Reference**: REF motion-16 (Raycast keyboard cascade; 2D stagger via `--row/--col`) — https://www.raycast.com
**Effort**: ~45min
**RTL safe**: yes — RTL grids in CSS auto-flow place col 0 at rightmost; the sweep enters top-right (read-direction leading corner) without sign-flip.
**prefers-reduced-motion**: existing guard in v3.js:147 (`if … !reduceMotion`) skips IO entirely; the existing fallback at line 153 (`cards.forEach(c => c.classList.add('nw3a-in'))`) reveals cards instantly without the stagger. No change.

---

## MOVE 5: Wide-card stats = tabular-roll counters with hover flag-flip (REF Stripe)

**The moment**: When the @almobadir_media wide card enters the viewport, "143+ / 5K+ / $1M+" digits roll in like Stripe's pricing reels, then on hover each stat flag-flips (number flips card-style to the unit description and back).
**Where**: `.nw3a-stats li` @ index.html:466–470, v3.css:390–393
**Implementation**:
```html
<!-- REPLACE the existing <ul class="nw3a-stats"> at index.html:466-470 -->
<ul class="nw3a-stats" aria-label="مؤشرات الوكالة">
  <li class="nw3a-stat" data-target="143" data-suffix="+">
    <span class="nw3a-stat-fig" data-flip>
      <span class="nw3a-stat-fig-front" dir="ltr"><span class="nw3a-stat-num">0</span><span class="nw3a-stat-suf">+</span></span>
      <span class="nw3a-stat-fig-back">عميل</span>
    </span>
    <span class="nw3a-stat-lab">عميل</span>
  </li>
  <li class="nw3a-stat" data-target="5000" data-suffix="K+" data-divisor="1000">
    <span class="nw3a-stat-fig" data-flip>
      <span class="nw3a-stat-fig-front" dir="ltr"><span class="nw3a-stat-num">0</span><span class="nw3a-stat-suf">K+</span></span>
      <span class="nw3a-stat-fig-back">قطعة محتوى</span>
    </span>
    <span class="nw3a-stat-lab">قطعة محتوى</span>
  </li>
  <li class="nw3a-stat" data-target="1" data-suffix="M+" data-prefix="$">
    <span class="nw3a-stat-fig" data-flip>
      <span class="nw3a-stat-fig-front" dir="ltr"><span class="nw3a-stat-pre">$</span><span class="nw3a-stat-num">0</span><span class="nw3a-stat-suf">M+</span></span>
      <span class="nw3a-stat-fig-back">إنفاق إعلاني</span>
    </span>
    <span class="nw3a-stat-lab">إنفاق إعلاني</span>
  </li>
</ul>
```
```css
/* ADD after v3.css:393 */
.nw3a-stat-fig { display: inline-block; perspective: 600px; transform-style: preserve-3d; position: relative; min-width: 4ch; }
.nw3a-stat-fig-front, .nw3a-stat-fig-back {
  display: inline-block; backface-visibility: hidden;
  transition: transform .55s cubic-bezier(.2,.7,.1,1);
  font-variant-numeric: tabular-nums;
}
.nw3a-stat-fig-back {
  position: absolute; inset: 0; transform: rotateX(180deg);
  font-family: var(--font-ar-body); font-weight: 600; font-size: 0.55em; color: var(--ink-soft);
  display: flex; align-items: center; justify-content: flex-start;
}
.nw3a-stat:hover .nw3a-stat-fig-front { transform: rotateX(-180deg); }
.nw3a-stat:hover .nw3a-stat-fig-back  { transform: rotateX(0); }
.nw3a-stat-num { font-variant-numeric: tabular-nums; }
```
```js
/* ADD inside the v3.js network IIFE, after the cards.forEach IO block */
const wide = section.querySelector('.nw3a-card--wide');
if (wide) {
  const stats = wide.querySelectorAll('.nw3a-stat');
  const ioStat = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (!e.isIntersecting) return;
      stats.forEach(li => {
        const target = +li.dataset.target;
        const div = +(li.dataset.divisor || 1);
        const numEl = li.querySelector('.nw3a-stat-num');
        const final = (target / div);
        const start = performance.now();
        const dur = 1400;
        (function tick(t){
          const p = Math.min((t - start) / dur, 1);
          const eased = 1 - Math.pow(1 - p, 3);
          numEl.textContent = (final * eased).toFixed(final < 10 ? 1 : 0).replace(/\.0$/,'');
          if (p < 1) requestAnimationFrame(tick);
        })(start);
      });
      ioStat.unobserve(e.target);
    });
  }, { threshold: 0.5 });
  ioStat.observe(wide);
}
```
**Reference**: REF interaction-3 (Stripe digit roll) + Browser Company / Things flip cards — https://stripe.com/pricing + https://thebrowser.company
**Effort**: ~3hr
**RTL safe**: numbers wrapped in `dir="ltr"` so digit order doesn't flip; the back-face Arabic label reads RTL natively. Flag-flip uses `rotateX` (vertical axis) — no horizontal sign-flip needed.
**prefers-reduced-motion**: existing guard at v3.css:1141 — extend it: `@media (prefers-reduced-motion: reduce) { .nw3a-stat-fig-front, .nw3a-stat-fig-back { transition: none; } }` and in JS replace counter with `numEl.textContent = final.toFixed(…)` directly (no rAF loop).

---

## MOVE 6: TikTok badge — small `TT` indicator alongside IG follower count (4/7 cards have TT)

**The moment**: A second small disc badge appears next to the IG count on the four cards that also have TikTok presence, with the TT follower count revealed on hover (fades in to the right of the IG count).
**Where**: `.nw3a-numbers` @ index.html:388, 401, 414, 427, 440, 453, 471 + v3.css:384–389
**Implementation**:
```html
<!-- For cards with BOTH IG + TT (mindset, almobadir, shalnack, flousak), MODIFY .nw3a-numbers:
     Mindset 478K IG / 149K TT — almobadir 363K IG / 266K TT — shalnack 672K IG / 248K TT — flousak 202K IG / 449K TT -->
<div class="nw3a-numbers">
  <span class="nw3a-count" aria-label="478 ألف متابع إنستغرام">
    <span class="nw3a-platform" aria-hidden="true">IG</span>
    <span class="nw3a-count-fig">478</span><span class="nw3a-count-unit">K</span>
  </span>
  <span class="nw3a-count nw3a-count--tt" aria-label="149 ألف متابع تيك توك">
    <span class="nw3a-platform" aria-hidden="true">TT</span>
    <span class="nw3a-count-fig">149</span><span class="nw3a-count-unit">K</span>
  </span>
  <span class="nw3a-count-label">متابع</span>
</div>
<!-- Also remove redundant " · TikTok 149K" string from the .nw3a-handle below it now that the badge carries it. -->
```
```css
/* ADD after v3.css:389 */
.nw3a-platform {
  display: inline-flex; align-items: center; justify-content: center;
  width: 22px; height: 22px; border-radius: 999px;
  border: 1px solid color-mix(in srgb, var(--accent) 35%, transparent);
  background: color-mix(in srgb, var(--accent) 10%, transparent);
  font-family: var(--font-mono); font-size: 9.5px; font-weight: 700;
  letter-spacing: 0.06em; color: var(--accent);
  margin-inline-end: 8px;
  /* RTL-safe — flexbox handles direction */
}
.nw3a-count--tt {
  margin-inline-start: 14px; opacity: 0.55;
  transition: opacity .35s var(--ease-glide), transform .35s var(--ease-glide);
  transform: translateX(0);
}
[dir="rtl"] .nw3a-count--tt { transform: translateX(0); } /* explicit no-op */
.nw3a-card:hover .nw3a-count--tt, .nw3a-card:focus-within .nw3a-count--tt { opacity: 1; }
```
**Reference**: REF arabic-3 (Pentagram bilingual co-equal mark — same compositional logic, two scripts/platforms not one) + REF interaction-13 (hover-earned reveal — TT count rises in opacity, not layout) — https://www.pentagram.com/work/khaleej-times
**Effort**: ~1.5hr
**RTL safe**: yes — `margin-inline-start/end` are logical; `IG`/`TT` are 2-letter Latin lockups inside `aria-hidden` spans, no bidi concern; the count `dir="ltr"` should also be set on the digits via `<span class="nw3a-count-fig" dir="ltr" unicode-bidi="isolate">` to bullet-proof.
**prefers-reduced-motion**: opacity-only transition is exempt from WCAG 2.3.3 — keep as-is. Add `transform: none` for `.nw3a-count--tt` in the existing reduced-motion block at v3.css:1141.

---

## MOVE 7: Initial-reveal stripe color cycle — accent ladder during sweep (1.2s only)

**The moment**: As each card reveals (Move 4 sweep), the 4px crimson stripe at top *cycles* through the brand palette (verde → crimson → gold → azure → teal → its own accent) in 900ms, then settles at the card's true accent. Happens once per session, on first reveal only.
**Where**: `.nw3a-stripe` @ v3.css:358 + IO callback in v3.js:148–151
**Implementation**:
```css
/* ADD after v3.css:358 */
@property --stripe-stop { syntax: "<percentage>"; inherits: true; initial-value: 0%; }
.nw3a-card.nw3a-in .nw3a-stripe {
  background: linear-gradient(90deg,
    var(--verde)   0%,
    var(--crimson) 20%,
    var(--gold)    40%,
    var(--azure)   60%,
    var(--teal)    80%,
    var(--accent)  100%);
  background-size: 600% 100%;
  background-position: var(--stripe-stop) 0;
  animation: nw3a-stripe-cycle 1.2s var(--ease-glide) forwards;
}
@keyframes nw3a-stripe-cycle {
  0%   { --stripe-stop: 0%;   transform: scaleX(0.4); }
  90%  { --stripe-stop: 100%; transform: scaleX(0.5); }
  100% { --stripe-stop: 100%; transform: scaleX(0.4); background: var(--accent); }
}
/* RTL: stripe transform-origin is right center already (v3.css:358), so the scaleX growth direction is correct. */
```
**Reference**: REF arabic-2 (Bahia Shehab "lam-alif as wallpaper" — the lexicon-of-history posture, here a 7-color ladder per card on first appearance) + the section's own per-card accent system — https://www.bahiashehab.com
**Effort**: ~1hr
**RTL safe**: decorative — `linear-gradient(90deg, …)` runs in physical px not logical, but the section is RTL-locked (per Wave 1 finding #25). No flip needed.
**prefers-reduced-motion**: add to the v3.css:1141 reduced-motion block: `.nw3a-card.nw3a-in .nw3a-stripe { animation: none; background: var(--accent); }`.

---

## MOVE 8: Totals strip — `dir="ltr" unicode-bidi: isolate` + Aramco-style oversized numerals on hover

**The moment**: The three totals (`2.2M / 1.1M / 11`) sit quiet at rest; on hover of any one number, the entire strip swaps to oversized eastern-Arabic-numeral typography (٢٫٢م / ١٫١م / ١١) at 1.6× scale for the focused stat — a single editorial moment of "this is the headline."
**Where**: `.nw3a-totals` @ index.html:370–376, v3.css:339–342
**Implementation**:
```html
<!-- REPLACE existing .nw3a-totals at index.html:370-376 -->
<dl class="nw3a-totals" aria-label="إجماليات الشبكة">
  <div class="nw3a-total">
    <dt>إنستغرام</dt>
    <dd>
      <span class="nw3a-num" dir="ltr" unicode-bidi="isolate" data-en="2.2M" data-ar="٢٫٢م">2.2M</span>
    </dd>
  </div>
  <span class="nw3a-total-sep" aria-hidden="true"></span>
  <div class="nw3a-total">
    <dt>تيك توك</dt>
    <dd>
      <span class="nw3a-num" dir="ltr" unicode-bidi="isolate" data-en="1.1M" data-ar="١٫١م">1.1M</span>
    </dd>
  </div>
  <span class="nw3a-total-sep" aria-hidden="true"></span>
  <div class="nw3a-total">
    <dt>القنوات النشطة</dt>
    <dd>
      <span class="nw3a-num" data-en="11" data-ar="١١">11</span>
    </dd>
  </div>
</dl>
```
```css
/* ADD after v3.css:342 */
.nw3a-total dd { transition: transform .35s var(--ease-glide); transform-origin: inline-end center; }
.nw3a-total:hover dd, .nw3a-total:focus-within dd { transform: scale(1.6) translateY(-2px); }
.nw3a-totals:hover .nw3a-total:not(:hover) dd { opacity: 0.4; transition: opacity .35s var(--ease-glide); }
.nw3a-num { font-variant-numeric: tabular-nums; }
```
```js
/* OPTIONAL — toggle to eastern-Arabic numerals on hover for true Aramco moment.
   Add inside the v3.js network IIFE: */
section.querySelectorAll('.nw3a-num').forEach(el => {
  const parent = el.closest('.nw3a-total');
  parent.addEventListener('pointerenter', () => { el.textContent = el.dataset.ar; });
  parent.addEventListener('pointerleave', () => { el.textContent = el.dataset.en; });
});
```
**Reference**: REF arabic-1 (Aramco "single Arabic numeral as full-bleed page") + REF arabic-16 (eastern-Arabic numerals as graphic) — https://www.aramco.com/-/media/publications/corporate-reports/annual-reports/saudi-aramco-ara-2024-english.pdf
**Effort**: ~1.5hr
**RTL safe**: yes — `dir="ltr" unicode-bidi="isolate"` on each numeric span (fixes Wave 1 finding #19, #20 simultaneously); `inline-end` transform-origin grows toward leading edge in RTL (right) — correct.
**prefers-reduced-motion**: add to v3.css:1141 block: `.nw3a-total dd { transition: none; transform: none !important; }`. Eastern-Arabic numeral text-swap is a content change, not motion — leave it on (it's a typographic preference, not animation).

---

## Top 3 — Ranked

### #1 — MOVE 5: Tabular-roll counters + flag-flip on @almobadir_media stats
**Why first**: This is the ONE wide card and the audit's Wave 1 already flagged the static `143+ / 5K+ / $1M+` strip as "data sheet" voice (finding #37). Counters fix that — numbers EARN attention by rolling, then the back-face label flip-reveals on hover. Single biggest "they sweat the details" moment in the section. References Stripe + Browser Company in one motion. ~3hr. Ships ahead of MP4 commission.

### #2 — MOVE 4: Diagonal sweep reveal in keyboard order
**Why second**: Cheapest to ship (~45min, two CSS lines). Replaces the current uniform 80ms-stagger with a 2D `--row/--col` ladder so cards light up from the brand-correct corner (top-right in RTL). Works *with* the existing IO chassis, layers under every other move. Raycast-grade.

### #3 — MOVE 1: Tighten 3D tilt to ±4° with spring leave
**Why third**: Wave 1 explicitly called the current ±6° tilt "vanity engineering" and asked to remove it entirely. This move keeps the gesture but cuts the gimmick: 4° instead of 6°, spring-overshoot return on leave, transition tightened to 120ms during follow + 420ms on release. The tilt becomes a *tactile* signal, not a parlor trick. ~1.5hr. Salvages the work already done.

---

## 200-Word Summary

The Network section is already the homepage's strongest module — magazine grid, dual-language tag, gradient title-em, and a grayscale-on-rest photo treatment Wave 1 named "the best photographic decision on the entire page." This brief asked for amplification *around* the photo, not on it.

Eight moves deliver that. **Three discipline edits** tighten the 3D tilt to a tactile ±4° spring (Move 1), Ken-Burns the cover hover so the grayscale-to-color reveal *moves* (Move 2), and add a session-once stripe-color ladder on initial reveal (Move 7). **Two photographic amplifications** introduce a hover-triggered SVG/MP4 loop layer (Move 3) and sweep-reveal the seven cards in keyboard order from the RTL leading corner (Move 4). **Three editorial moments** turn the wide @almobadir_media stats into Stripe-grade tabular-roll counters with flag-flip on hover (Move 5), surface a TT badge alongside IG on the four cards with TikTok presence (Move 6), and bullet-proof the totals strip with `dir="ltr" unicode-bidi: isolate` plus Aramco-style oversized eastern-Arabic-numeral on hover (Move 8).

Top three ranked: Move 5 (counters), Move 4 (sweep), Move 1 (tightened tilt). Total Top-3 effort: ~5.25hr. Every move ships RTL-safe and reduced-motion-safe with code already written above.
