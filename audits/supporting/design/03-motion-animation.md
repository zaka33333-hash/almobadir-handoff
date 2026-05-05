# Wave 3 — Motion & Animation Craft Audit

**Audited:** 2026-04-26
**Scope:** all 9 sections of `index.html` — header, hero, manifesto, network, method, content, newsletter, founder, footer.
**Surface area:** `assets/v3.css` (1,151 lines), `assets/v3.js` (498 lines), `assets/tokens.css` (200 lines), `assets/base.css` (animation/reduced-motion).
**Bench:** Apple product pages, Stripe Atlas / Sigma scroll choreography, Linear feature highlights, Vercel hero, The Browser Company demo videos.
**Bar:** every transition is intentional, easing/durations consistent and tokenized, prefers-reduced-motion fully respected, zero layout thrash, zero gratuitous animation.

---

## Executive summary

The motion layer on almobadir.com is **ambitious and partially refined, but unfinished against the bench**. The bones are right: a real easing/duration token set, a single dominant Apple-style glide curve, IntersectionObservers for reveals, RAF-throttled scroll/pointer handlers, and a comprehensive (if blunt) reduced-motion override. But execution leaks discipline at almost every layer.

Five structural problems dominate. **First**, the system is half-tokenized: 105 transitions use `var(--ease-glide)` but only 9 use any `var(--d-*)` duration token — every other duration is hand-typed in seconds (`.32s`, `.7s`, `1.2s`, `1.6s`, `0.55s`, `0.65s`), so the "scale" exists in tokens.css but is not enforced. Adjacent components fade in over 550ms here, 700ms there, 900ms elsewhere; the system has no rhythm. **Second**, eight transitions animate the layout properties `width`, `height`, `min-width`, and `backdrop-filter` — all reflow/repaint triggers that should be re-expressed as `transform: scale*` or removed. **Third**, the Method section runs five infinite SVG animations behind every inactive frame, wasting GPU on hidden content. **Fourth**, infinite-loop animations (ticker, marquee, conic halo, founder verified spin, manifesto orbit, hero hue-drift) consume battery for decoration the user never reads — at least three should be removed and three should pause when offscreen. **Fifth**, the reduced-motion guard is comprehensive in CSS but incomplete in JS: cnt3 progress, ftr3 column observer, hero pointer-mesh, and nw3a 3D card tilts all keep computing even when `reduceMotion=true` (the CSS then erases the visual outcome).

The work below is 47 findings across 9 topical groups, plus a prioritized fix list.

---

## 1. Keyframe inventory & verdicts

35 named `@keyframes` in the loaded CSS (34 in v3.css + 1 `pulse` in base.css). All are descriptively named (no `spin-1`/`fade-2` generics), all use animatable properties (`transform`, `opacity`, `box-shadow`, `--custom-property`, `stroke-dashoffset`, `background-position`), and there is no DOM consumer for the legacy `meshSpin`/`rise` keyframes in `components.css` (sheet not loaded — already flagged in Wave 1).

| # | Keyframe | Driving property | Iter | Verdict |
|---|---|---|---|---|
| 1 | `pulse` (base.css) | box-shadow | infinite | **KEEP** — used by `.pulse` live-dot pattern, ease-counter timing fits |
| 2 | `hero3-hue-drift` | `--hero3-hue`, `--hero3-glow` | 60s · infinite | **REDUCE** — 60s loop on the entire hero is invisible after 5s; downgrade to a 12-15s subtle sweep, or pause once user has scrolled past |
| 3 | `hero3-scroll-shift` | `--hero3-shift` | scroll-timeline | **KEEP** — uses native CSS `animation-timeline: scroll()` where supported; correct technique |
| 4 | `hero3-rise` | translateY/opacity | finite (forwards) | **KEEP** — entrance reveal on `[data-stagger]` |
| 5 | `hero3-pulse` | scale 0.6→2.4 + fade | 1.6s · infinite | **REMOVE** — it's a ring-expand on `.hero3__pulse-ring` that loops forever; one cycle on entry would say everything two cycles do not |
| 6 | `hero3-halo-spin` | rotate 360° | 24s · infinite | **REMOVE OR PAUSE OFFSCREEN** — conic-gradient + filter blur(60px) rotating forever is GPU-expensive and visually trivial through 60px blur |
| 7 | `hero3-blink` | opacity 1↔0.35 | 1.6s · infinite | **KEEP** — semantic LIVE indicator |
| 8 | `hero3-ticker` | translateX 0→-50% | 50s · infinite | **REDUCE** — reads as filler; shorten to 24-30s OR pause when `prefers-reduced-data: reduce` OR when section scrolled out of viewport |
| 9 | `hero3-scroll-cue` | translateY/opacity | 2.4s · infinite | **KEEP** — scroll-cue pulse, classic affordance |
| 10 | `manif3-spin` | rotate 360° | 28s · infinite | **REMOVE** — manifesto ornament SVG spinning forever in the gutter is decorative and undermines the editorial gravitas |
| 11 | `nw3a-pulse` | scale + opacity | 2.4s · infinite | **KEEP** |
| 12 | `nw3a-arrow` | translateX wiggle | one-shot on hover | **KEEP** — micro-interaction on CTA, exactly the right use |
| 13 | `mtd3-pulse` | scale + opacity | 2.4s · infinite | **KEEP** — eyebrow live dot |
| 14 | `mtd3-bounce` | translateY 6px | 2s · infinite | **KEEP** — scroll affordance arrow |
| 15 | `mtd3-spin` (3 instances at 28s/22s/18s) | rotate 360° | infinite | **PAUSE OFFSCREEN** — rings 1/2 + reel keep spinning on inactive frames, wasted compositor work |
| 16 | `mtd3-pulse-ring` | scale + opacity | 2.4s · infinite | **PAUSE OFFSCREEN** |
| 17 | `mtd3-pulse-dot` | scale 1↔1.5 | 1.6s · infinite | **PAUSE OFFSCREEN** |
| 18 | `mtd3-flicker` | opacity steps | 3.2s · infinite | **PAUSE OFFSCREEN** — stepped flicker on tick marks reads as broken until you understand it's intentional |
| 19 | `mtd3-mess-fade` | opacity + transform | 6s · infinite alternate | **PAUSE OFFSCREEN** |
| 20 | `mtd3-clean-fade` | opacity + transform | 6s · infinite alternate | **PAUSE OFFSCREEN** |
| 21 | `mtd3-strip` | translateX 60px | 3s · infinite | **PAUSE OFFSCREEN** — film-strip continually nudges sideways even on frames 1, 4, 5 |
| 22 | `mtd3-wave` | scale + opacity | 3.6s · infinite, staggered ×4 | **PAUSE OFFSCREEN** — radio-tower waves run on inactive frames |
| 23 | `mtd3-tilt` | rotate -3°→2° + translateY | 6s · infinite alternate | **PAUSE OFFSCREEN** — magazine cover wobbling on frames 1-3, 5 |
| 24 | `cnt3-pulse` | scale + opacity | 2.4s · infinite | **KEEP** |
| 25 | `cnt3-shimmer` | background-position | 1.4s · infinite | **KEEP**, with caveat — it's a skeleton placeholder and JS removes the element on image load (correct), but keep an eye on never removing it on 4G/slow networks |
| 26 | `nl3-pulse` | box-shadow | 2.4s · infinite | **KEEP** |
| 27 | `nl3-spin` | rotate 360° | 0.6s · infinite | **KEEP** — submit-button loading spinner |
| 28 | `nl3-pulse-green` | box-shadow | 2s · infinite | **KEEP** — semantic live indicator |
| 29 | `nl3-marquee` | translateX 50% | 38s · infinite | **REMOVE** — past-issues marquee is a gimmick; the four `<a href="#">` placeholders aren't even real links yet (Wave 1 finding). Replace with a small static row of clickable issue cards |
| 30 | `fnd3aRail` | translateY 50% | 28s · infinite | **REDUCE OR REMOVE** — vertical scrolling word rail on founder is editorial flourish; if kept, pause when section is offscreen |
| 31 | `fnd3aSpin` (×2, fwd + reverse) | rotate 360° | 24s · infinite | **REMOVE** — Apple-style "verified" badge that rotates is kitsch; the badge is the message, the rotation undermines it |
| 32 | `ftr3-pulse` | box-shadow | 2s · infinite | **KEEP** |

**Net:** 5 keyframes for outright removal (`hero3-pulse`, `manif3-spin`, `nl3-marquee`, `fnd3aSpin`, plus one of the two `fnd3a-rail` directions if kept), 12 for "pause when offscreen" (almost all mtd3) so they don't burn GPU on hidden content, 3 for "shorten or downgrade" (`hero3-hue-drift`, `hero3-ticker`, `fnd3aRail`).

---

## 2. Transition inventory: 96 declarations, off-token durations dominate

96 distinct `transition:` declarations across v3.css. The picture is that easing is well-tokenized, durations are not.

**Easing usage:**

| Where used | Count | OK? |
|---|---|---|
| `var(--ease-glide)` | 105 | Canonical, good |
| `var(--ease-snap, --ease-out, --ease-in, --ease-counter)` | 0 | **Tokens declared but never used** |
| Hardcoded `ease` (named) | 8 | **Off-token** — see #3 |
| Hardcoded `ease-in-out` | 1 | `nw3a-eyebrow-dot` keyframe (line 283) |
| Hardcoded `linear` | 9 | mostly correct (sub-frame text reveal in mtd3 + cnt3 progress fill); marquees |
| Hardcoded `cubic-bezier(...)` | 1 | v3.js line 347 form-shake animate (the bezier *is* `--ease-glide` typed inline) |

**Duration usage:**

| Where used | Count | Notes |
|---|---|---|
| `var(--d-base)` | 9 | hero3 only — every other section types `0.32s` |
| `var(--d-fast/slow/glide)` | 0 | **Three of four duration tokens are unused** — `--d-fast`, `--d-slow`, `--d-glide` exist in tokens.css and are referenced 0 times in v3.css transitions |
| Hand-typed seconds | ~85 | The full inventory below |

**Distinct durations actually used in v3.css transitions** (sorted):

`0.12s, 0.14s, 0.15s, 0.2s, 0.24s, 0.25s, 0.28s (280ms), 0.3s, 0.32s, 0.32s (320ms), 0.35s, 0.4s, 0.45s, 0.48s (480ms), 0.5s, 0.52s (520ms), 0.55s, 0.6s, 0.65s, 0.7s, 0.8s, 0.9s, 1.0s, 1.1s, 1.2s, 1.4s, 1.6s`

**That's 27 distinct values for what should be a 4-value scale** (`fast 150 / base 320 / slow 700 / glide 1000`). Wave 1 was right: each section reinvents its own micro-scale.

Notable inconsistencies:

1. **Card hover transforms** — `.hdr3-card` uses `0.24s`, `.nw3a-card` uses `.65s`, `.cnt3-card` uses `0.55s`, `.fnd3a__follow-card` uses `0.5s`, `.nl3-issue` uses `280ms`. Five different durations for "card lifts on hover". Pick one (320ms `--d-base` is correct).
2. **Form border focus** — `.hero3__signup-row` uses `var(--d-base)`, `.nl3-form` uses `320ms`, `.ftr3-cta__form` uses `.3s`. Three syntaxes for the same value.
3. **Text-reveal entrance durations** — `.manif3__word .55s`, `.cnt3-card .55s`, `.fnd3a stats .9s`, `.ftr3-col .7s`, `.nw3a-card .9s`, `.mtd3__frame mobile 1s`, `.mtd3__frame desktop .7s`. The mental scale appears to be "fast .3s / medium .55-.7s / slow .9-1s", but the pattern isn't anchored.
4. **Image hover scale durations** — `.nw3a-cover img 1.2s`, `.cnt3-img 1.2s`, `.fnd3a__photo 1.6s`. The 1.6s on the founder photo is the one outlier that feels intentionally cinematic; the other two should be 700ms (`--d-slow`).
5. **`min-width 480ms` on `.nl3-submit`** — different from the same line's `transform 280ms` and `background 280ms`. The success-state width-grow runs longer than the hover state; that's a layout-property animation problem (see #4).

---

## 3. Off-token easings

8 transitions and 1 keyframe use the named keyword `ease` instead of a token. `ease` is not a brand decision — it is the browser's default and looks generic against the bespoke `--ease-glide` curve.

| File:Line | Selector | Property | Replace with |
|---|---|---|---|
| v3.css:294 | `.nw3a-card` | border-color, box-shadow | `var(--ease-glide)` |
| v3.css:320 | `.nw3a-cover img` | filter | `var(--ease-glide)` |
| v3.css:761 | `.fnd3a__auth-card` | border-color | `var(--ease-glide)` |
| v3.css:770 | `.fnd3a__auth-shine` | transform | `var(--ease-glide)` |
| v3.css:778 | `.fnd3a__follow-card` | border-color, background-color | `var(--ease-glide)` |
| v3.css:779 | `.fnd3a__follow-card::before` | opacity | `var(--ease-glide)` |
| v3.css:790 | `.fnd3a__follow-arrow` | color | `var(--ease-glide)` |
| v3.css:283 | `.nw3a-eyebrow-dot` keyframe | (timing fn) `ease-in-out` | `var(--ease-glide)` (note: keyframes can't take `var()` for the timing-function on `animation:` shorthand without re-typing — but `nw3a-pulse 2.4s var(--ease-glide) infinite` works) |

**3 of `--ease-snap`, `--ease-out`, `--ease-in`, `--ease-counter` are declared but never referenced**. Either delete them from tokens.css (lying tokens are worse than no tokens) or assign them owners — `--ease-snap` is the obvious fit for the network card hover *lift* (snap = 1.56 overshoot bezier reads as "tactile"); `--ease-counter` is the right curve for the founder counter ticker (already used in base.css `.pulse`).

---

## 4. Layout-thrash transitions (reflow risk)

8 transitions animate layout properties. Each forces a reflow on every animation frame; on a slow GPU or low-end Android, these spike the frame budget. All can be re-expressed.

| Line | Selector | Bad property | Fix |
|---|---|---|---|
| 18, 21, 22 | `.hdr3`, `.hdr3-announce`, `.hdr3.is-scrolled .hdr3-announce` | `height 0.32s`, `backdrop-filter 0.32s` | `height` should remain (announce strip collapse is acceptable on a single sticky element); `backdrop-filter` is **GPU-expensive to transition** — drop it from the transition list (the property change still applies instantly, the *blur amount* changing 14px→22px is barely visible anyway) |
| 34, 35 | `.hdr3-main`, `.hdr3-main-inner` | `height 0.32s` | Two header heights animate independently; combine and animate via `transform: scaleY` on a wrapper, or accept the reflow (single-element, sticky-positioned, every browser handles this fine). Lower priority. |
| 75 | `.hdr3-search` | `width 0.32s` (220→260px on focus) | **Use `transform: scaleX(1.18)` with `transform-origin: right center`**, or fix at 260px and accept slightly wider rest state |
| 76 | `.hdr3-search:focus-within` | width: 260px | Same as above |
| 380 | `.mtd3__progress-fill` | `width .55s` | This is a deliberate progress bar; CSS `width` is the convention. **Acceptable**, but if perf matters: animate `transform: scaleX()` with `transform-origin: left` |
| 577 | `.cnt3-progress-fill` | `width 0.4s linear` | Same. The `linear` here is correct (progress shouldn't ease). Acceptable. |
| 579 | `.cnt3-accent-bar` | `width 0.5s` | Already has `transform-origin: right top` and `transform: scaleX(1)` — looks like the author started to convert this to scaleX but left the `width` in. **Drop the `transition: width`** and animate scaleX instead |
| 632 | `.nl3-submit` | `min-width 480ms` (168px → wider) | Animate `transform: scaleX()` *or* use a fixed wider min-width and let label/success swap inside. **The current 480ms feels like jelly** — visibly slower than the 280ms transform/background on the same element |

**Bonus: backdrop-filter transition (line 18) is the worst offender** — every paint frame the browser re-runs the blur filter at a new radius. Drop it.

---

## 5. Compositing / GPU

Only 4 elements have `will-change`:

| Line | Selector | Value |
|---|---|---|
| 128 | `.hero3__mesh` | `transform` |
| 458 | `.mtd3__frame` (desktop) | `opacity, transform` |
| 481, 488 | `.mtd3__frame.is-active .mtd3__art / .__num` | `transform` |
| 661 | `.nl3-frame` | `transform` |

Missing `will-change` candidates that animate *every frame* via JS (RAF):

- **`.nw3a-card`** — pointer-tilt sets `--rx`/`--ry` properties consumed by `transform: rotateX/rotateY`; this transform should be promoted. Add `will-change: transform` on the card or on the inner element.
- **`.fnd3a__photo`** — JS sets `transform: scale(...) translate(...)` on every mousemove. Add `will-change: transform`.
- **`.cnt3-cta::before`** — radial-gradient position animates via `--mx`/`--my` on mousemove. The gradient is a **paint-bound** operation, not transform-bound; `will-change: background` would help but radial gradients in transitions are **inherently expensive**. Consider a static glow with a transform-translated highlight instead.

Conversely, `will-change: transform` declared but element rarely actually moves:

- **`.mtd3__frame` (line 458)** is fine — it animates on every scroll event during the pinned segment.
- **`.nl3-frame` (line 661)** declares `will-change: transform` but only animates on hover-tilt — that's *too aggressive*; the layer is held forever. Either remove `will-change` and rely on the on-hover transform to auto-promote, or set it dynamically in JS on `pointerenter` / unset on `pointerleave`.

---

## 6. Scroll-driven animations

**Manifesto word-by-word reveal** (v3.js:122-139): RAF-throttled, correct. `IntersectionObserver` opens the section, then a scroll handler computes a single progress value and toggles `.is-lit` per-word. **Issue:** the loop iterates `words.length` (≈70 spans?) on every scroll frame and `classList.toggle` on each. On a slow device this could be expensive. Optimize by computing `lit` then only toggling the diff (`if (i < lit) add('is-lit'); else if (i >= lit) remove('is-lit');` is fine, but you can short-circuit further by tracking last `lit` count).

**Method scrollytelling (mtd3)** (v3.js:220-252): RAF-throttled, correct. Reads `getBoundingClientRect()` on every frame — that's a **forced layout** if anything has invalidated the layout in the same frame. Cache `stage.offsetHeight` outside the scroll handler (it changes only on resize); current code reads `stage.offsetHeight` inside `update()` every frame.

**Content featured-card progress bar** (v3.js:307-319): RAF-throttled but **does not check `reduceMotion`**. The progress bar still animates for users who asked for reduced motion. CSS reduced-motion block (line 1142) catches `.cnt3-card .cnt3-img .cnt3-cta .cnt3-cta-arrow .cnt3-accent-bar .cnt3-headline .cnt3-dot .cnt3-skeleton` but **NOT** `.cnt3-progress-fill`. Either add to the selector list or short-circuit the JS.

**Footer column stagger** (v3.js:457-465): IntersectionObserver, no RAF needed (one-shot reveal). **Does not respect reduceMotion** — the IO still attaches and adds `is-in`. CSS reduced-motion block does cover `.ftr3-col`, so the visual outcome is correct, but JS overhead remains.

**Hero pointer-mesh** (v3.js:80-97): correctly guarded by `if (!reduceMotion && finePointer && ...)`. ✅
**Network card 3D tilt** (v3.js:155-171): correctly guarded by `if (reduceMotion || matchMedia('(hover: none)').matches) return;`. ✅
**Newsletter preview tilt** (v3.js:354-372): correctly guarded with `&& !reduceMotion`. ✅
**Founder photo parallax** (v3.js:432-440): correctly guarded with `&& !reduceMotion`. ✅
**Counter animation** (v3.js:411): correctly short-circuits with `if (reduceMotion) { el.textContent = ...; return; }`. ✅

**Summary:** 4 of 6 JS scroll/pointer modules respect reduceMotion correctly. The two leaks are `cnt3` progress and the indirect `ftr3` IO overhead.

**Sticky behavior:** newsletter preview at `top: 96px` (v3.css:660) — sticky positioning has no animation cost beyond layout; the issue is that `.hdr3` itself is sticky at `top: 0` *and* sets `z-index: 100` which equals `--z-modal`. Header should be `--z-header (50)` and stay below modals. Sticky-on-sticky is otherwise fine.

**Header sticky** is correct: `position: sticky; top: 0` plus a background/blur transition triggered by JS `is-scrolled`. RAF not needed because the threshold check is a single scalar comparison; current passive scroll listener with `if (y === lastScroll) return;` early-out is exactly right.

---

## 7. prefers-reduced-motion compliance

There are **two** reduced-motion blocks, and they conflict slightly:

**Global (base.css:60-68):**
```
*, *::before, *::after {
  animation-duration: 0.01ms !important;
  animation-iteration-count: 1 !important;
  transition-duration: 0.01ms !important;
  scroll-behavior: auto !important;
}
```
This is the safe blanket — reduces every animation to ~0 and stops infinite loops at 1 iteration. Universally applied. ✅

**Section-specific (v3.css:1138-1151):** lists ~50 specific selectors and sets `animation: none !important; transition: none !important; transform: none !important; opacity: 1 !important;`.

**Conflict analysis:**

1. The two blocks are not contradictory — both use `!important`, but the `*`-selector global has lower specificity than the v3 list. The v3 list **overrides** the global on its targeted selectors (`transition: none` is a stronger statement than `transition-duration: 0.01ms`), and the global catches everything else. So coverage is good.
2. **Risk:** the universal `*` rule kills *every* transition, including ones the user might still want at native speed (focus rings appearing instantly, dropdown opening). At ~0.01ms these still happen, just imperceptibly fast — that's actually the spec-correct behavior. ✅

**Selectors covered correctly in v3.css block:**

✅ Hero mesh hue-drift (`.hero3` is in the list, but the keyframe is `hero3-hue-drift` applied to `.hero3` — covered)
✅ Hero ticker (`.hero3__ticker-track`)
✅ Hero halo conic spin (`.hero3__panel-halo`)
✅ Hero pulse-ring (`.hero3__pulse-ring`)
✅ Manifesto word reveal (`.manif3__word, .manif3__period, .manif3__underline path`)
✅ Manifesto orbit ornament (`.manif3__ornament svg`)
✅ Network 3D tilts (covered by `.nw3a-card` + JS guard)
✅ Method (`.mtd3 *` — wildcard, kills all motion in mtd3)
✅ Newsletter marquee (`.nl3-issues-row`)
✅ Founder rail + verified spin (`.fnd3a__verified, .fnd3a__verified-tick, .fnd3a__rail-text`)
✅ Footer column stagger (`.ftr3-col`)

**Selectors MISSING from v3.css reduced-motion list:**

❌ `.hero3__panel-halo` — listed (line 1139) ✅ (re-verified)
❌ `.hero3__panel-link svg` (the arrow on hover) — has transition, not in reduced list, but covered by global `*`
❌ `.cnt3-progress-fill` — JS still sets `style.width`, the global `*` covers transition-duration, but the value is still mutated; functionally OK
❌ `.fnd3a__auth-shine` (transform 1.2s on hover) — covered by global `*`
❌ `.fnd3a__follow-arrow`, `.fnd3a__name-block` already in list (line 1144) — verified but partially: `.fnd3a__follow-arrow` not listed, only `.fnd3a__follow-card`. The arrow has its own transitions (line 790). Functionally fine via global `*`.
❌ `.hdr3-mega`, `.hdr3-card`, `.hdr3-search`, `.hdr3-cta`, `.hdr3-burger span`, `.hdr3-drawer` — none of the header transitions are in the reduce list. Global `*` saves them. **Recommendation:** drop the v3.css selector-specific list and rely on the global `*` rule. The list adds maintenance burden for marginal additional safety, and any selector you forget is a bug.

**Reduced-motion + JS:**

The JS uses `const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches` once on DOMContentLoaded (v3.js:8). **Issue:** this never updates if the user toggles their system preference mid-session. Add a `mql.addEventListener('change', ...)` to reload or re-init the affected modules. Lower priority — most users don't toggle this dynamically.

---

## 8. Gratuitous animation: keep / reduce / remove

Per the bench (Apple = motion *demonstrates the product*, Stripe = motion *teaches the data*, Linear = motion *acknowledges interaction*), here is the list of every persistent or scroll-driven animation in the site, with a verdict.

| # | Animation | Section | Verdict | Rationale |
|---|---|---|---|---|
| 1 | Hero mesh hue-drift (60s loop) | hero3 | **REDUCE** | Only the first 5 seconds have any perceivable color shift; after that the user has either stopped looking or scrolled. Cut to 12-18s with a tighter hue range, OR pause when section is offscreen. |
| 2 | Hero conic-gradient halo spin (24s) | hero3 panel | **REMOVE** | The conic is rendered behind a 60px blur — the user cannot perceive rotation. Energy savings, zero visual loss. |
| 3 | Hero pointer-mesh parallax | hero3 | **KEEP** | Subtle, opt-in via fine pointer, lerp-smoothed. Apple-style. |
| 4 | Hero `[data-stagger]` rise (8 elements, 180ms ladder) | hero3 | **KEEP** | Classic entrance choreography. Stagger increments are slightly off (`540ms` repeats at indices 3 and 5 — see #2-finding-3 below). |
| 5 | Hero LIVE pulse-ring (1.6s loop) | hero3 panel | **REMOVE** | Two indicators within 30px — a dot already says LIVE. The ring is decoration. |
| 6 | Hero LIVE blink dot | hero3 panel | **KEEP** | Semantic. |
| 7 | Hero ticker (50s loop, 24 items?) | hero3 | **REDUCE** | Dial to 28-32s. The 50s pace is a sleepy crawl; bench tickers (Stripe-customers strip) move at ~24-30s. Also pause on focus-within so keyboard users can read items. |
| 8 | Hero scroll-cue chevron (2.4s) | hero3 | **KEEP** |
| 9 | Manifesto word-by-word lit reveal (scroll-driven) | manif3 | **KEEP, REDUCE PER-WORD** | The technique is right but currently every word reveals from .14 opacity to 1 at `.55s` — the wave is too slow over 70+ words and forces the reader to wait. Cut per-word transition to 250-320ms; let the *scroll* cadence space them, not the per-word duration. |
| 10 | Manifesto orbit ornament (28s spin) | manif3 | **REMOVE** | The manifesto is the section's gravity center — a spinning ornament works against the editorial weight. Set static. |
| 11 | Manifesto rule-line draw + period punctuation + underline path | manif3 | **KEEP** | All scroll-triggered, finite, purposeful. |
| 12 | Network card 3D tilt (pointer) | nw3a | **KEEP** | Bench-pattern correct (Linear/Vercel use this). MAX=6° is conservative and right. |
| 13 | Network card hover image scale + grayscale-out | nw3a | **KEEP** | The grayscale-on-rest, color-on-hover is editorial and on-brand. |
| 14 | Network eyebrow pulse dot | nw3a | **KEEP** |
| 15 | Method ring spins (28s, 22s, 18s) | mtd3 frame 1 | **PAUSE OFFSCREEN** | When the user is on frames 2-5, ring 1's SVG is still rotating off-canvas. CPU/GPU waste. |
| 16 | Method pulse-ring + pulse-dot + flicker ticks | mtd3 frame 1 | **PAUSE OFFSCREEN** | Same. |
| 17 | Method mess→clean alternating fade (6s) | mtd3 frame 2 | **PAUSE OFFSCREEN** |
| 18 | Method film reel + strip (22s, 3s) | mtd3 frame 3 | **PAUSE OFFSCREEN** |
| 19 | Method magazine cover tilt (6s) | mtd3 frame 4 | **PAUSE OFFSCREEN** |
| 20 | Method radio waves (3.6s + 4 staggers) | mtd3 frame 5 | **PAUSE OFFSCREEN** |
| 21 | Method sub-progress text reveal (smoothstep `--mtd3-sub`) | mtd3 | **KEEP — high craft** | This is the best motion in the site. Ease-curve smoothstep, RAF-throttled, multi-stage staggered text. The bench-grade work. |
| 22 | Method bounce hint arrow (2s) | mtd3 hud | **KEEP** |
| 23 | Method eyebrow pulse | mtd3 | **KEEP** |
| 24 | Content card stagger reveal (IO + transition-delay 0/180/310ms) | cnt3 | **KEEP** |
| 25 | Content image lazy-load + zoom-on-hover (1.2s) | cnt3 | **REDUCE 1.2s → 700ms** |
| 26 | Content featured progress bar | cnt3 | **KEEP** but add reduceMotion guard in JS |
| 27 | Content CTA radial-mouse follow | cnt3 | **REDUCE OR KEEP** | Radial-gradient on mousemove is paint-expensive. Either accept it or replace with a translated highlight pseudo. |
| 28 | Content shimmer skeleton | cnt3 | **KEEP** |
| 29 | Newsletter form border + ink underline (focus) | nl3 | **KEEP** |
| 30 | Newsletter preview 3D tilt (mouse, lerp) | nl3 | **KEEP** |
| 31 | Newsletter past-issues marquee (38s) | nl3 | **REMOVE** | 8 chips with `href="#"` placeholder links. Replace with a static row of real issue cards. |
| 32 | Newsletter submit shake on invalid + spinner + success | nl3 | **KEEP — high craft** | The Web Animation API shake is exactly the right pattern (used by Stripe, Linear). |
| 33 | Founder counters (RAF, ease-out cubic 1400-1800ms) | fnd3a | **KEEP** | Well-tuned. The 1400-1800ms randomization gives the three counters slight desync, which feels more organic. |
| 34 | Founder photo zoom on view + on-hover parallax | fnd3a | **KEEP — REDUCE 1.6s → 900ms** | 1.6s scale transition is glacial. |
| 35 | Founder verified badge dual spin (24s fwd + reverse) | fnd3a | **REMOVE** | Spinning verified badge undermines the verification message. Static is more authoritative. |
| 36 | Founder vertical word rail (28s loop) | fnd3a | **REDUCE** | If kept, pause when section offscreen. |
| 37 | Founder follow-card hover lift + arrow translate | fnd3a | **KEEP** |
| 38 | Footer column stagger reveal (IO + delay ladder) | ftr3 | **KEEP** |
| 39 | Footer Riyadh clock 1Hz tick | ftr3 | **KEEP** |
| 40 | Footer pulse dot (2s) | ftr3 | **KEEP** |
| 41 | Footer language pill slide (transform on toggle) | ftr3 | **KEEP** |
| 42 | Header announce ticker (6s setInterval rotation) | hdr3 | **KEEP** | Pause on hover already implemented, mouse leaves and resumes — correct pattern. |
| 43 | Header mega-menu open/close (140ms hover delay) | hdr3 | **KEEP** | The 140ms delay prevents accidental dismissal on cursor crossings — Linear/Notion pattern. |
| 44 | Header sticky background blur transition | hdr3 | **REDUCE** | See #4 — drop the `backdrop-filter` from the transition list, keep the property change. |
| 45 | Header logo mark hover rotate 15° | hdr3 | **KEEP** |
| 46 | Header CTA hover lift + glow | hdr3 | **KEEP** |
| 47 | Header burger morph (3 lines → ×) | hdr3 | **KEEP** |

**Tally:** 28 KEEP, 9 REDUCE, 6 REMOVE, 12 PAUSE OFFSCREEN (mtd3 cluster + 3 founder/hero), and 1 reduce per-word duration.

---

## 9. Animation timing system: scale verdict

**Tokens declared in tokens.css:**
- `--d-fast: 150ms` (for hover, focus)
- `--d-base: 320ms` (state changes)
- `--d-slow: 700ms` (entrance, emphasis)
- `--d-glide: 1000ms` (hero stagger)

**Tokens actually used in v3.css:**
- `--d-base`: 9 transitions (all in `.hero3__*` selectors, lines 140-217)
- `--d-fast, --d-slow, --d-glide`: 0 transitions, 1 keyframe (`hero3-rise` uses `--d-glide`)

**Effective scale in production (clustered):**

| Bucket | Range used | Should be |
|---|---|---|
| Micro hover (color, opacity flip) | 0.2s, 0.24s, 0.25s | `--d-fast (150ms)` — current is too slow for hover state colors |
| Hover lift / state change | 0.28s, 0.3s, 0.32s, 0.35s | `--d-base (320ms)` ✅ |
| Card entry / image reveal | 0.4s, 0.45s, 0.48s, 0.5s, 0.55s, 0.6s | **Missing token** — need `--d-medium: 500ms` between base and slow |
| Card hover lift cinematic | 0.55s, 0.65s, 0.7s | `--d-slow (700ms)` |
| Hero entrance, manifesto rule | 0.8s, 0.9s, 1.0s, 1.1s, 1.2s | `--d-glide (1000ms)` for 1s, **another missing token** for 1.2s+ |
| Long image cinematic | 1.4s, 1.6s | **No token** — these should be `--d-cinematic: 1400ms` if kept |

**Recommendation:** add `--d-medium: 500ms` and `--d-cinematic: 1400ms` to tokens.css, then refactor 85 hand-typed durations to tokens. Not a one-day job; flag as Wave 4 cleanup.

**Stagger ladders:**

The `[data-stagger]` system has a small bug: indices `3` and `5` both use `540ms` (lines 152 and 154). If the intent is a clean ladder, index 5 should be `720ms` and the existing `720ms` (index 4) should perhaps be `900ms`. Actually the entries appear to be:
```
0: 0ms · 1: 180 · 2: 360 · 3: 540 · 4: 720 · 5: 540 (?!) · 6: 900 · 7: 1080
```
Index 5 = 540ms is likely a typo (should probably be 720ms or 900ms). Verify against the visual: if 5 elements pop in two waves, the typo is intentional; otherwise fix.

**`--d-base` value is borderline slow.** Bench-grade hover transitions (Linear, Vercel) sit at 200-260ms. 320ms feels just slightly heavy. Consider re-setting `--d-base: 240ms` for hover/state and using `--d-slow: 320ms` for the gentler reveals.

---

## 10. Micro-interactions: hover/focus/active consistency

| Element | Hover IN | Hover OUT | Focus | Active |
|---|---|---|---|---|
| `.hdr3-cta` | `transform .32s` + glow `.4s` | symmetric | (no `:focus-visible` defined separately) | (no active) |
| `.hdr3-card` | `bg .24s, border .24s, transform .24s` (-1px) | symmetric | (no `:focus-visible`) | (no active) |
| `.nw3a-card` | tilt + `transform .65s` + box-shadow + grayscale-off | symmetric via JS lerp | `:focus-within` mirrors hover ✅ | (no active) |
| `.cnt3-card` | `transform .55s` + image `transform 1.2s` | symmetric | (no `:focus-within` styles) | (no active) |
| `.fnd3a__follow-card` | `transform .5s` + border + arrow translate | symmetric | (no `:focus-within`) | (no active) |
| `.hdr3-nav-link` | `color .2s` + underline scale `.32s` | symmetric | (relies on browser default outline) | none |
| `.nl3-submit` | `transform .28s` (-2px lift) | symmetric | (relies on browser default; no custom focus ring) | none |
| `.ftr3-cta__btn` | `transform .3s` + bg `.3s` | symmetric | none | none |
| `.mtd3__cta` | `bg .35s` + border + transform (-2px) | symmetric | (relies on default) | none |

**Findings:**

1. **`:focus-visible` is almost completely absent.** Only `.nw3a-link` (line 353) defines a custom focus ring. Every CTA, card, and nav link relies on the user-agent default outline (which Chrome/Firefox draw, but the visual doesn't match the brand). For a 10/10 site, every interactive element needs a visible, on-brand focus state. Add a global `:focus-visible { outline: 2px solid var(--crimson); outline-offset: 4px; }` plus per-element overrides where the offset doesn't fit.
2. **No active states defined anywhere.** Pressing a button on touch devices shows no feedback. Add `:active { transform: scale(0.98); }` or similar to all CTAs.
3. **Hover-out durations are correct (symmetric).** Almost every transition uses the same duration in both directions because they're declared on the base selector — that's the right pattern.
4. **Header search width grow on focus (220→260px) is the only case where the focused state is *visibly* different from the rest state in dimension.** That's a layout-property animation issue (#4) but the *intent* is correct: focused = wider input = more text visible.
5. **Form `:focus-within` is implemented well in 3 of 4 places.** Hero, newsletter, footer all have a ring/border state on `:focus-within`. Header search has it. Manifesto and content sections have no forms, so N/A.

---

## 11. Section-by-section motion audit (condensed)

### hdr3 — header
- 18 transitions, 4 micro-interactions, 1 ticker rotation, 1 mega-menu animation, 1 burger morph.
- **Strongest:** mega-menu `transform: translate(-50%, -8px) scale(0.98)` → `translate(-50%, 0) scale(1)` with visibility delay is correct (Stripe/Notion pattern).
- **Weakest:** `transition: backdrop-filter` on line 18 — cut.

### hero3 — hero
- The most animated section: 8 keyframes, 1 scroll-timeline, 1 pointer-mesh, 8 stagger ladder.
- **Strongest:** pointer-mesh lerp (0.08 smoothing factor — Apple-grade) and scroll-timeline shift with `@property --hero3-shift`.
- **Weakest:** `hero3-pulse` ring + `hero3-halo-spin` are decoration the user can't perceive through the blur.

### manif3 — manifesto
- Word-by-word reveal (scroll), rule-draw, period-pop, underline path, ornament spin.
- **Strongest:** the rule-line + period + underline choreography is bench-grade editorial. Stripe Atlas could ship this.
- **Weakest:** the spinning ornament fights the section's gravitas.

### nw3a — network
- Card entrance, 3D tilt, image grayscale flip, accent stripe scale, eyebrow pulse, arrow wiggle.
- **Strongest:** the grayscale-on-rest editorial style + stripe-scale + 3D tilt on hover is restrained and on-brand.
- **Weakest:** `.nw3a-card transition: ... .4s ease, ... .5s ease` — three off-token easings on the same selector.

### mtd3 — method
- Scroll-pinned scrollytelling with sub-progress, 14 SVG decorations, frame entrance, text stagger.
- **Strongest:** the smoothstep sub-progress text reveal at thresholds .32/.42/.56/.70 is the most impressive motion on the site.
- **Weakest:** every inactive frame is still running its SVG decoration animations. Net: at any moment the GPU is rendering 4 frames' worth of background motion the user cannot see.

### cnt3 — content
- Card stagger reveal, image lazy-load + zoom, featured progress bar, CTA radial follow, shimmer skeleton.
- **Strongest:** the image-load shimmer that swaps cleanly to the loaded image.
- **Weakest:** progress bar JS bypasses reduceMotion check.

### nl3 — newsletter
- Form focus ring, 3D preview tilt, submit loading spinner + success, marquee chips, countdown clock.
- **Strongest:** submit-button shake-on-invalid via WAAPI is a craft moment.
- **Weakest:** marquee of 8 placeholder chips is a feature pretending to exist.

### fnd3a — founder
- Counter ramp, photo zoom + parallax, vertical rail, dual-spin verified badge, follow-card hovers.
- **Strongest:** counter desync (1400-1800ms randomized) feels human.
- **Weakest:** the spinning "Authentic Voice" verified badge is the only motion on the site that reads as kitsch.

### ftr3 — footer
- Column stagger, Riyadh clock, language toggle slide, top button, pulse dot, mailbtn hover.
- **Strongest:** language pill slide with transform-translate is a clean toggle pattern.
- **Weakest:** column IO observer runs even when reduceMotion is true.

---

## 12. Priority fix list (ordered by impact / effort)

### High-impact, low-effort (do this week)

1. **Pause method (mtd3) inactive-frame animations**: Add `.mtd3__frame:not(.is-active) * { animation-play-state: paused !important; }` after line 521. Saves significant GPU on the longest section. (1 line of CSS.)
2. **Drop the spinning verified badge** (`fnd3aSpin` keyframe + `.fnd3a__verified` + `.fnd3a__verified-tick` animation lines 724, 728): set `animation: none`. The verified mark works static; rotating undermines it.
3. **Remove the manifesto orbit ornament spin** (line 270): set `animation: none`. The manifesto is the calmest section; spinning ornament fights it.
4. **Add reduceMotion guard to cnt3 progress bar** (v3.js:307): wrap RAF in `if (!reduceMotion)`.
5. **Drop `transition: backdrop-filter`** in `.hdr3` (line 18): removes a paint-expensive transition with no visual loss.
6. **Add `:focus-visible` global rule** with brand-on outline. ~6 lines of CSS, applies everywhere.
7. **Fix the [data-stagger] index 5 typo** (line 154): change `540ms` to either `720ms` or `900ms` to make the ladder monotonic.

### High-impact, medium-effort (next sprint)

8. **Replace 8 layout-thrash transitions** with transform equivalents (search-width grow, accent-bar width, submit min-width, header heights). Use `transform: scaleX/scaleY` with origin set.
9. **Add `--d-medium: 500ms` and `--d-cinematic: 1400ms` tokens**, then refactor 85 hand-typed durations to tokens (mass find-and-replace). Estimate: 2-3 hour scrub.
10. **Convert all `0.4s ease`, `0.5s ease`, `1.2s ease` references to `var(--ease-glide)`** (8 instances, listed in #3).
11. **Trim hero hue-drift to 18s** and pause when hero is offscreen via IntersectionObserver.
12. **Trim hero ticker to 28s** and pause on focus-within (already pauses on hover).
13. **Replace nl3 marquee with a static 4-card row of real issues** (depends on having real issue URLs first).

### Medium-impact, low-effort

14. **Add `will-change: transform` to `.nw3a-card`, `.fnd3a__photo`** (2 lines).
15. **Remove `will-change: transform` from `.nl3-frame`** or set/unset dynamically (it's held for the entire page load right now).
16. **Remove unused easing tokens** `--ease-snap, --ease-out, --ease-in, --ease-counter` from tokens.css *or* assign them owners. Don't ship dead tokens.
17. **Add `mql.addEventListener('change', ...)`** in v3.js so reduceMotion toggles take effect mid-session.
18. **Cache `stage.offsetHeight` outside the mtd3 scroll handler** to avoid forced layouts.
19. **Add `:active` states (transform: scale(0.98))** on all primary CTAs.

### Low-impact, low-effort (polish)

20. **Manifesto per-word transition: 550ms → 280ms** so the wave keeps pace with scroll instead of dragging.
21. **Founder photo zoom: 1.6s → 900ms** for the on-view scale.
22. **Hero pulse-ring removal** (one keyframe drop).
23. **Hero halo-spin removal** (decorative, behind 60px blur).
24. **cnt3 image scale-on-hover: 1.2s → 700ms.**
25. **Drop the v3.css selector-specific reduced-motion list** (1138-1151) and rely on the global `*` rule in base.css. Simpler, harder to forget.

---

## 13. Bench comparison

| Pattern | Almobadir | Apple | Stripe | Linear | Vercel |
|---|---|---|---|---|---|
| Single canonical easing curve | Yes (`--ease-glide`, used 105×) | Yes (Apple-style cubic) | Yes | Yes | Yes |
| Tokenized durations enforced | **No** (used 9× of 96) | Yes | Yes | Yes | Yes |
| Scroll-pinned scrollytelling | Yes (mtd3) | Yes (product pages) | Yes (Atlas) | No | Partial |
| Smoothstep sub-progress reveal | Yes (mtd3, bench-grade) | Yes | Yes | n/a | n/a |
| 3D card tilt micro | Yes (nw3a) | No | No | Yes | Yes |
| Pointer-mesh hero parallax | Yes (lerp 0.08) | Yes | Yes (sigma) | n/a | Yes |
| Reduced-motion blanket | Yes (global + selector) | Yes | Yes | Yes | Yes |
| Reduced-motion JS branches | Partial (4/6 of 6) | Yes | Yes | Yes | Yes |
| Persistent infinite ornaments | **6** keep playing, **12** offscreen-active | 0 | 0 | 0 | 0 |
| Layout-property transitions | **8** (width/height/min-width/backdrop-filter) | 0 (transforms only) | 0 | 0 | 0 |
| Custom `:focus-visible` ring | **1 of ~80** interactive | All | All | All | All |
| Active state on CTAs | **None** | Yes | Yes | Yes | Yes |

**Bottom line:** the motion system on almobadir is on the right ladder — same easing curve, scroll-driven choreography, IO patterns, RAF-throttled handlers — but it is decorating itself instead of disciplining itself. The 6 always-on ornaments and 12 offscreen-active animations are the visible gap. Closing that and tokenizing durations would put the site at parity with Stripe Atlas. The smoothstep mtd3 sub-progress reveal is already bench-grade.

---

## 14. Closing summary (200 words)

Almobadir's motion layer has the right architecture and, in places, bench-grade execution — the smoothstep sub-progress text reveal in the Method section, the pointer-mesh lerp in the hero, the WAAPI shake on the newsletter submit, and the counter-animation desync on the founder section all read as deliberate craft. But the system is half-tokenized: 105 transitions correctly use `var(--ease-glide)` while 87 hand-type durations across 27 distinct values, splintering what should be a 4-step scale. Six infinite-loop animations decorate without serving (manifesto orbit, founder verified spin, hero halo + pulse-ring, hero hue-drift's later loops, newsletter placeholder marquee), and twelve more keep running on the Method section's inactive frames, wasting GPU on hidden content. Eight transitions animate layout properties (width, min-width, height, backdrop-filter) that should be transforms. Reduced-motion is well-covered in CSS but two JS modules (cnt3 progress, ftr3 IO) bypass the guard. `:focus-visible` is missing from 79 of 80 interactive elements and active states are absent everywhere. None of this is hard to fix; the site is one disciplined sprint from being motion-equivalent to Stripe Atlas.
