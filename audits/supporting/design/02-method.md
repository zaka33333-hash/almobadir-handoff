# Wave 2 Audit — METHOD section (.mtd3)

**Audited:** 2026-04-26
**Live URL:** http://localhost:8765/index.html
**Source:** `website/index.html` lines 458–830 · `website/assets/v3.css` lines 356–540 · `website/assets/v3.js` lines 175–279
**Bar:** Billion-dollar website. Apple iPhone PDP / Stripe homepage / Linear / Bloomberg long-form scrolly.

**Live captures used in this audit:** `design-audit/screenshots/mtd3-frame1-sub0.png` · `mtd3-frame1-sub5.png` · `mtd3-frame1-sub1.png` · `mtd3-frame2-sub0.png` · `mtd3-frame2-sub1.png` · plus wave-1 `mtd3-{1440,768,390}.png`. Frames 3, 4, 5 live-state captures were blocked by a Playwright browser lock mid-session — those frames are audited from static code only and that limitation is flagged where relevant.

---

## Executive Summary

This is the most ambitious section on the page. It is also the most flawed. The core promise — "icon enters from the right, text reveals as the reader settles into the frame" — does not actually deliver in the live build. Three structural defects cripple the choreography:

1. **The sliding numeral and the sliding SVG icon collide.** Both `.mtd3__num` (the giant outlined "01") and `.mtd3__art` (the targeting reticle) are translated by `+42vw` simultaneously and end at overlapping X-positions. At sub=0 they are stacked in the centre. At sub=1 the numeral sits inside the art region instead of behind it on the opposite side. This is visible at every settled frame in the captures and reads as a layout bug, not a deliberate move.
2. **The slide is decoupled from the entrance.** At sub=0 (frame just activated) the icon appears at `translateX(42vw)` *from its CSS-grid natural left position*. Because the natural position is column-2 in RTL (visually LEFT half of the stage), `+42vw` lands the icon roughly in the centre of the viewport — not on the right edge as the prompt intends. The "from the right" gesture never reads.
3. **HUD pill is half-cut by the sticky header.** The `.mtd3__hud` sits at `margin: clamp(20px,2vw,32px) auto 0` inside the pin, but the `.hdr3` header is sticky at the top of the document. At 1440×900 the HUD sits at y≈25 and the header backdrop (height 105px) covers most of it. Visible in `mtd3-frame1-sub0.png`, `mtd3-frame1-sub5.png`, `mtd3-frame2-sub0.png`. The progress bar — the one navigational anchor — is unreadable.

Beyond these three, there are 50+ smaller issues that compound: a 4500px fixed-height stage on a page already 14,809px tall (a third of the page is one section), 8 always-running CSS animations even in inactive frames, two color palettes (CSS + JS) duplicated, the magazine cover headline "قرار يُعيد الاقتصاد" reads more as a placeholder than a real cover line, and the meta-list "الكدخل" is a typo in the source (should be "المُدخل").

The intent is right and the section is genuinely on the path to being a hero moment. But in its current state it is the weakest part of an otherwise strong page — visitors will read this as "the engineer was excited about scrollytelling" rather than "the editorial method is so confident the site shows its mechanics".

---

## Vanity-Engineering Lens

| Pattern | Severity | Note |
|---|---|---|
| 5×100vh pinned stage = 4500px scroll for 5 short text blocks | V2 Structural | Apple 16 Pro PDP uses 1×100vh per "moment" with hard cuts, not 5 stacked. The reading payload here (5 kickers + 5 h3 + 5 paragraphs + 15 meta items) fits in 1500px static. The 3000px overhead is decoration. |
| Per-frame `--mtd3-sub` updated every rAF for 5 staggered text reveals | V1 Drag | Browsers under-utilize this on a desktop. On a 60Hz mid-tier laptop it's fine. On a Saudi-3G-throttled 2GB Android over a hot afternoon — measurable. |
| `colors` array hardcoded in JS (line 184) duplicating CSS `[data-color="..."]` (lines 398-402) | V2 Structural | Two sources of truth for 5 brand colors. If anyone adjusts crimson in tokens.css, the HUD gradient stays stale. Read `data-color` and resolve via `getComputedStyle(...).getPropertyValue('--mtd3-c')` instead. |
| 8 always-running CSS animations (`mtd3-spin` ×3, `mtd3-pulse-ring`, `mtd3-pulse-dot`, `mtd3-flicker`, `mtd3-mess-fade`, `mtd3-clean-fade`, `mtd3-strip`, `mtd3-wave` ×5, `mtd3-tilt`) | V1 Drag | All 5 art SVGs animate continuously even when the frame is `opacity:0; visibility:hidden`. Browsers don't always pause them. Should be gated on `.mtd3__frame.is-active` only. |
| `@property --mtd3-progress` and `@property --mtd3-active` declared, never used | V0 Cosmetic | Lines 357-358. Dead. The actual driver is `--mtd3-sub` declared on 451. |
| `.mtd3__rail` markup (HTML 475-483) rendered with `display:none` (CSS 382) | V2 Structural | 8 lines of HTML × 6 elements = pure dead weight in the DOM. The intended vertical writing-mode rail was abandoned but the markup wasn't deleted. |

**Requirement-to-Complexity Ratio: 7/10.** This is editorial scrollytelling with the budget of a NASA mission control. The user need is "convey that there is a five-step editorial method and signal that the team is rigorous". A static numbered list with one accent illustration would deliver 80% of the impact. The remaining 20% requires craftsmanship that the current execution has not yet earned.

---

## Findings (58)

### A. CHOREOGRAPHY (12 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| A1 | V3 Critical | v3.css:476-489 | **Both `.mtd3__art` AND `.mtd3__num` translate by the SAME `(1 - slide) * 42vw`.** They end at overlapping X-positions. Captured: `mtd3-frame1-sub0.png` shows numeral and reticle stacked centre. `mtd3-frame1-sub1.png` shows the "01" numeral occupying the same X-band as the targeting circle, the numeral's right edge clips into the kicker text. | Numeral should NOT slide. Pin `.mtd3__num` to `inset-inline-end: 4%` always (its natural CSS-grid background position) and only fade its opacity 0→.55 over sub 0→.3. Only the `.mtd3__art` slides. |
| A2 | V3 Critical | v3.css:467-468 | **42vw slide distance is wrong direction in RTL.** Comment in code admits the confusion: "`negative X moves rightward visually in flipped origins; translation done physically below`". In RTL CSS-grid column 2 places `.mtd3__art` on the LEFT half. Translating it `+42vw` (physical right) at sub=0 puts it CENTRE, not RIGHT. The "from the right" gesture never lands. | At sub=0, art's left edge should be at viewport's right edge. Compute precisely: art is 32vw wide and grid-justified to the left of column 2 (≈25%–55% of stage X). To place it at right edge requires `+50vw` not `+42vw` at this layout. Fix by making the slide distance equal to `(viewport_right - art_natural_left)` in JS, not a fixed vw value in CSS. |
| A3 | V2 Structural | v3.css:479,486 | **Slide transition is `transform .14s linear`** — 140ms linear feels mechanical. Combined with rAF-driven sub updates this gives 30Hz step animation on 60Hz monitors. | Use `transform: translateX(...)` driven by the eased `--mtd3-sub` (already eased server-side via smoothstep) with `transition: none`. The smoothstep on sub IS the easing. Keeping a 140ms linear transition on top introduces lag without smoothing. |
| A4 | V2 Structural | v3.js:238 | Smoothstep `s*s*(3-2*s)` is applied to `--mtd3-sub`, then linear thresholds (kicker .32→.52 etc.) read from the eased value. This means the eased curve is sampled non-uniformly by clamp() — kicker reveals slower at start, faster at end. | Either ease the *thresholds* by inverse-smoothstep, or apply smoothstep *only* to slide and leave text reveal on raw sub. Today's curves don't compose correctly. |
| A5 | V2 Structural | v3.css:493-510 | **Text reveal thresholds (kicker .32→.52, h .42→.64, body .56→.81, meta .70→.95)** start AFTER slide ends (.55). Reader spends scroll 0→.55 watching only the icon move. At 720 px/frame on the typical scroll, that's ~400px of pure decorative motion before the title even appears. | Begin kicker reveal at sub=.15 (overlap with slide), h at .28, body at .45, meta at .65. Total reveal ends at .9. Hold zone shortens from .45 to .1 — much more punchy. |
| A6 | V2 Structural | v3.css:467 | **Hold zone (sub 0.55→1.0) = 45% of frame scroll with no visual change** once text is revealed. Reader reaches "all visible" at sub=.95 and then has to scroll a viewport more to advance. Boring. | Either compress: have the active-frame segment finish at sub=.85 and use .85→1.0 as transition-out window. Or: add a subtle "exhale" — let the icon scale back down to .96 over sub .85→1.0 so the eye registers something happening. |
| A7 | V2 Structural | v3.css:458-459 | **Frame-to-frame transition is opacity .7s + transform 36px** but the JS swaps `is-active` instantly on `Math.floor(progress * 5)` boundaries. The result is a hard cut overlaid with a fade — looks slightly broken. | Either fade-cross between frames over a 5-10% sub overlap zone, or eliminate the transform displacement and use opacity-only swap. Apple's PDP uses opacity-only. |
| A8 | V2 Structural | v3.css:380 | **HUD progress fill uses `transition: width .55s var(--ease-glide)`** so the bar lags the actual frame change. When the user scrolls fast it lurches. | Drop the transition; let the width be driven by `--mtd3-progress` continuously (which is already declared but unused, line 357). Replace the JS step-based fill update with `root.style.setProperty('--mtd3-progress', progress)`. |
| A9 | V1 Drag | v3.js:194 | **Progress fill width** = `(i+1) * 20%`. At frame 1 settled, fill = 20% — but the user has just barely entered the section. Unintuitive. | Use continuous `progress` value (0→1) for the fill. Show step count separately. |
| A10 | V2 Structural | v3.js:195 | **HUD gradient transitions across two adjacent frame colors** (e.g. crimson→verde for frame 1). Visible: `mtd3-frame1-sub0.png` shows red-to-gold gradient on a 32px-wide bar — colors don't read at this size. | Either use solid frame color for the fill, or extend bar width to 240+px. Two-color gradient on 32px is illegible. |
| A11 | V1 Drag | v3.js:222 | `update()` reads `getBoundingClientRect()` and `offsetHeight` every scroll frame. Layout thrash. | Cache `docTop` and `total` via a single ResizeObserver on `<body>`; only recompute on resize. |
| A12 | V0 Cosmetic | v3.css:514-520 | Leaving-frame fade-out (kicker/h/body/meta opacity 0 over .5s) competes with the new frame's fade-in. Two crossfades simultaneously is unnecessary. | Pin opacity 0 instantly on `.mtd3__frame:not(.is-active)`. The container handles the fade. |

### B. VISUAL — PER-FRAME ICON QUALITY (8 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| B1 | V2 Structural | index.html:498-572 | **Frame 1 Targeting reticle.** Recently improved but still busy. 12 radial ticks + corner brackets + 5 concentric rings + sweep arc + crosshair lines + inner reticle bracket + cardinal markers + HUD readout text + center pulse = ~40 elements at 400×400. At the live render it reads as "tactical UI" rather than "editorial precision". The `text x="48" y="60">N 24°42'`-style fake coordinates feel cosplay. | Cut to: 1 outer ring, 1 inner ring, 4 corner brackets, crosshair, center dot. Drop "RNG 1.2km" / "LOCK ✓" — they reference a domain (military/sniper) at odds with editorial credibility. |
| B2 | V1 Drag | index.html:594-609 | **Frame 2 mess strokes** are at `rgba(255,255,255,.35)` to `.7` opacity on `.mtd3-mess-fade` 0→1. At sub=1 they fade out per the keyframe, but the `infinite alternate` cycle means they re-appear at random intervals while reading. Visible in `mtd3-frame2-sub1.png` — the mess paths are still drawn behind the clean ring at settled state. The before/after metaphor needs deterministic timing tied to scroll, not a 6s loop. | Bind mess opacity to `1 - var(--mtd3-sub)` and clean opacity to `var(--mtd3-sub)`. The simplification metaphor must follow the user, not run independently. |
| B3 | V1 Drag | index.html:638-731 | **Frame 3 film reel** has 6 spokes + 6 reel windows + 3 hub layers + film strip with 6 dividers + 16 sprocket holes (8 top, 8 bottom) + 6 frame numbers + REEL · 03 + 24 FPS labels = ~50 elements. Detail is appropriate for the metaphor BUT the strip animation `translateX(60px)` over 3s linear has no relation to the rest of the section's scroll-driven motion. | Bind `mtd3-strip` to `--mtd3-sub` so the film advances as the reader scrolls. Or pause when frame is inactive. |
| B4 | V3 Critical | index.html:741-749 | **Frame 4 magazine cover** uses HTML/CSS instead of SVG (legitimate — typography rendering). But the cover headline `قرار يُعيد الاقتصاد` reads as a placeholder. "A decision that re-economy" is grammatically incomplete in Arabic. Issue label `عدد ٠٤ · ٢٠٢٦` and stub list "تقرير مُصوَّر · ٢٤ صفحة · ٧ مصادر" feel made-up. The cover is the ONLY frame whose content is a fictional object — it has to feel real or the metaphor breaks. | Use a real or near-real upcoming/recent المبادر cover. Even better: rotate among 3 real covers per page-load. The placeholder headline is the single most damaging detail in the section because it is the only frame asserting "this is what we make". |
| B5 | V1 Drag | v3.css:440 | **Cover tilt animation** `transform: rotate(-3deg) → rotate(2deg) translateY(-6px)` over 6s alternate is independent of scroll. Magazine "floats". OK in isolation but combined with the slide-in this creates compound motion that reads as instability. | Lock cover at `rotate(-3deg)` until frame is active; only animate tilt while `.is-active`. |
| B6 | V1 Drag | index.html:763-819 | **Frame 5 broadcast tower** has 3 antenna crossbars + tower body + 6 horizontal cross-braces + zigzag diagonal bracing + 2 base feet + top-of-mast pulsing light + 4 signal-strength bars + frequency readout (98.6 MHz STEREO −42 dBm). Detail is overwrought for an icon. The "98.6 MHz STEREO" particularly reads as fake-radio cosplay (المبادر is digital media — there is no radio). | Cut signal-strength bars and frequency readout. Tower + waves + a single "LIVE" label with a dot is enough. |
| B7 | V2 Structural | All 5 frames | **No coherent illustration system.** Frame 1 has a glow gradient + ticks + readout text; Frame 2 has clean nested geometry; Frame 3 has a reel + sprocket strip; Frame 4 has a typography-rendered cover; Frame 5 has a tower + pulsing waves. Every frame uses a DIFFERENT visual vocabulary. The only shared element is the outlined numeral. | Define a system: every frame uses (1) a single primary glyph centred, (2) the outlined numeral as a watermark behind it, (3) one mono-spaced label below. Strip all secondary detail. The 5 glyphs become: bullseye / cube / clapperboard / cover-card / waves. |
| B8 | V0 Cosmetic | index.html:566-571, 633-634, 717-727, 814-818 | **In-SVG mono text** (HUD readouts) is rendered at font-size 7-11 with letter-spacing 1.5-3 and opacity .55-.7. Readable but not crisp on retina (SVG `<text>` doesn't get sub-pixel kerning the way HTML does). | Move mono labels OUT of SVG into HTML siblings positioned via CSS. Better rendering, easier to RTL-correct, keeps the SVG to drawn elements only. |

### C. TYPOGRAPHY (10 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| C1 | V1 Drag | v3.css:366 | `.mtd3__title` is `clamp(32px,4.6vw,68px)` line-height **1.0**. Arabic display fonts (Reem Kufi, fallback Tajawal) have descenders on ع ج ح خ that clip below the baseline at lh:1.0. Already known issue per prior section audits. The title here uses كلمة "معقّدة" with a ق descender. | Bump to line-height 1.05 minimum. Even 1.08 looks tighter than 1.0 with descenders. |
| C2 | V1 Drag | v3.css:408 | `.mtd3__h` line-height **1.18** is correct, but `padding-block: 0.04em` is the same fragile workaround used elsewhere on the page to "rescue" clipping. If the line-height were 1.22 the padding hack wouldn't be needed. | Drop padding-block, raise line-height to 1.22. |
| C3 | V1 Drag | v3.css:403,461 | `.mtd3__num` font-size `clamp(180px, 22vw, 320px)` desktop. At 1440 viewport that's 316px. Visual mass at this size, in `-webkit-text-stroke: 1.5px`, is enormous — competes with the h3 (44px). The numeral upstages the title at every settled frame. | Reduce stroke to 1px, font-size to clamp(140px, 18vw, 240px). Numeral should support, not dominate. |
| C4 | V1 Drag | v3.css:403 | `letter-spacing: -.04em` on the outlined numeral — at this size the negative tracking causes "01" to fuse into "0)" visually. | Use letter-spacing: -.02em or 0. |
| C5 | V0 Cosmetic | v3.css:413 | `.mtd3__meta b` font-size **14px** while `.mtd3__meta span` (label) is **10px** with `letter-spacing: .22em`. The label is wider per character than the value below it. Visual hierarchy inverted. | Either label 11px lh 1.4 letter-spacing .18em, or value 16px. |
| C6 | V0 Cosmetic | v3.css:412 | Meta `<span>` labels in mono (Latin font fallback) above Arabic `<b>` values — mixing scripts in a label/value pair. Should both be in Arabic OR labels stay mono Latin. The current mix reads as English left-aligned over right-aligned Arabic — slightly off. | Choose: all Arabic (Almarai) for both with letter-spacing 0; or all mono Latin labels with English-localized labels. |
| C7 | V1 Drag | v3.css:524 | `.mtd3__cta` font-size **12px** with `letter-spacing: .24em`. Effective tracking pushes Arabic letters apart, breaking ligatures. Rule: never letter-space Arabic. The CTA is Arabic ("شاهد المنهج تطبيقاً"). | Drop letter-spacing for Arabic CTAs. Reserve mono+spaced for Latin labels only. |
| C8 | V0 Cosmetic | v3.css:443 | `.mtd3-cover__masthead` font-weight 900, font-size clamp(28px, 3.4vw, 44px), line-height **0.9**. Same lh:1.0 issue as title. | line-height 1.0+. |
| C9 | V1 Drag | v3.css:445 | `.mtd3-cover__head` font-size clamp(22px, 2.8vw, 38px) line-height **1.0** with multi-line content. Text "قرار يُعيد الاقتصاد" sits on 2-3 lines; descenders on ق will clip. | line-height 1.1. |
| C10 | V0 Cosmetic | index.html:489, 580, 643, 736, 757 | All 5 kickers use the same template: "المرحلة [الأولى/الثانية/...] · [الاختيار/التبسيط/...]". The phase number is given THREE times: Arabic ordinal in kicker, outlined Latin numeral, و٠١-٠٥ in the art-tag and HUD. Triplicated. | Drop the ordinal from the kicker. Read just "الاختيار", etc. The numeral and HUD already say which phase. |

### D. SPACING / LAYOUT (8 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| D1 | V3 Critical | v3.css:454-456, observed | **HUD pill is hidden under the sticky `.hdr3` header.** At 1440×900 viewport: hdr3 height ≈105px, hdr3 is `position:sticky; top:0`. mtd3 pin starts at the top of the stage, HUD has `margin: clamp(20px,2vw,32px) auto 0` = 28px top margin. HUD lands at y≈28-58. Header overlaps y=0-105. **HUD is fully hidden.** Visible: ALL captures show no readable HUD pill — only a dark sliver at y≈28. | Either: (a) add `margin-top: 105px` to .mtd3__hud (or `var(--hdr3-height)` if defined elsewhere), (b) hide the sticky header during the pinned scroll via JS toggling a class on body, or (c) move HUD to bottom of the pin. Best: (a). |
| D2 | V2 Structural | v3.css:454 | Stage `height: calc(100vh * 5)` = 4500px on 900vh. The .mtd3 section's TOTAL height with intro + outro is ~5108px (per discovery doc). On a 14,809px page, .mtd3 is **34.5% of the page**. Reading time exceeds reading payload. | Reduce stage height to `calc(100vh * 3.5)` — 3150px. Each frame's hold zone shortens; section feels punchier. Or move to 5×60vh (1800px). Apple's iPhone PDP uses 1×100vh per moment with hard cuts. |
| D3 | V2 Structural | v3.css:458 | Frame padding `clamp(48px,6vw,96px) clamp(48px,8vw,140px)` — 96 vertical, 140 horizontal at 1440. Combined with `align-items: center` on a 1.15fr/1fr grid, the meta list sits at y≈540 (out of 768 visible after HUD/header chrome). Below the fold on shorter viewports. Visible: `mtd3-frame1-sub1.png` shows meta at y~480 of 731 visible region, partially below typical above-the-fold expectations. | Reduce vertical padding to clamp(40px,4vw,72px). Consider `align-items: start` with explicit margin-top on copy. |
| D4 | V1 Drag | v3.css:362 | `.mtd3__intro` `margin: clamp(48px,6vw,80px) auto clamp(32px,4vw,56px)` — 80px above the scrollytelling. But because the section is pinned, this 80px is the only "breathing room" the user gets between the intro headline and the frame 1 art entrance. At sub=0 the icon is centred (per A2) and immediately visible. The intro doesn't feel separate from the stage. | Either insert a deliberate "scroll to begin" empty viewport between intro and stage (Apple does this), or extend the intro vertical padding to clamp(96px,10vw,160px) so the user reads the intro on its own viewport before pinning starts. |
| D5 | V1 Drag | v3.css:373 | `.mtd3__stage` `max-width: 1320px` desktop, but `padding: 0` inside the `@media (min-width: 901px)` rule overrides outer 32px padding. This makes the stage edge-to-edge at 901-1319 viewports and inset at 1320+. Inconsistent. | Add explicit `padding-inline: clamp(20px,3vw,32px)` on the stage at all sizes. |
| D6 | V1 Drag | v3.css:410 | Meta grid `grid-template-columns: repeat(3, 1fr)` with a `border-block-start` separator. At tighter widths the 3 columns squeeze and the labels (mono, .22em letter-spacing) wrap to 2 lines. | At <1100px, switch to 2 columns; <760px, 1 column. The meta list is the densest text element in the frame — needs more breathing room. |
| D7 | V0 Cosmetic | v3.css:522 | `.mtd3__outro` is `display: flex; justify-content: space-between; flex-wrap: wrap` — at 900px-ish it wraps the CTA below the text but doesn't add gap. Looks broken when wrapped. | Add `gap: 24px` and `align-items: center` always. |
| D8 | V0 Cosmetic | v3.css:374 | `.mtd3__pin` non-desktop says `height: auto; min-height: 0; overflow: visible; display: block` and the `@media (min-width: 901px)` block resets it. The base properties are dead; could be moved entirely into `@media (max-width: 900px)`. | Inline the desktop properties as default and put mobile override in `@media (max-width: 900px)`. Half the rules go away. |

### E. COLOR (5 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| E1 | V2 Structural | v3.css:398-402 + v3.js:184 | **Color palette duplicated** in CSS (`[data-color="crimson"] { --mtd3-c: #E6213B }` etc.) and JS (`const colors = ['#E6213B', ...]`). Both reference the same physical hex codes. If `var(--crimson)` in tokens.css drifts, the JS stays stale. Already flagged in audit prompt. | JS should resolve at runtime: `getComputedStyle(frames[i]).getPropertyValue('--mtd3-c').trim()`. CSS owns the source of truth. |
| E2 | V1 Drag | v3.css:397 | Frame-bg radial gradient `opacity: .18` `filter: blur(40px)` — at sub=0 (frame just active) this glow appears immediately at full opacity. There's no buildup. | Bind glow opacity to `var(--mtd3-sub)`. Glow fades in with the slide. |
| E3 | V1 Drag | index.html:732 | Frame 4 uses `data-color="azure"` (#5AA8FF) but the cover itself is var(--paper) cream with crimson accents. Azure appears only in the frame-bg radial glow and the text-stroke on the numeral. Inconsistent — every other frame's primary color saturates the icon. | Either change frame 4 to `data-color="crimson"` so the cover, accent, and numeral all match (the cover already uses crimson internally), or change the cover internals to azure. The current crimson-cover-on-azure-bg is the only frame where icon and frame don't share color. |
| E4 | V1 Drag | v3.css:380 | Progress fill linear-gradient `var(--crimson), #E8B855` — hardcoded gold-yellow on the right. Doesn't update for non-frame-1 contexts (the JS overwrites this on frame change but the initial state on page load is wrong if user lands on the section directly via #method anchor). | Initial fill should use the active frame's color or be neutral. |
| E5 | V1 Drag | v3.css:524, 525 | CTA hover background → `var(--crimson)` with `var(--ink)` text. `var(--ink)` is presumably cream — crimson against cream needs verification. From tokens, crimson #E6213B against cream #FBF7EE: contrast 4.78:1, passes AA for large text but borderline for 12px small text. With letter-spacing breaking ligatures (C7), legibility further degrades. | Either use `var(--ink-on-crimson)` token explicitly, or darken crimson on hover to #C71428 for better contrast. |

### F. MOTION (5 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| F1 | V2 Structural | v3.css:417-439 | **8 always-running CSS animations** (`mtd3-spin` × 3 ring-1/ring-2/reel, `mtd3-pulse-ring`, `mtd3-pulse-dot`, `mtd3-flicker`, `mtd3-mess-fade`, `mtd3-clean-fade`, `mtd3-strip`, `mtd3-wave` × 5 layered, `mtd3-tilt`). All run `infinite` regardless of frame visibility. | Gate with `.mtd3__frame.is-active &` selector or use `animation-play-state: paused` on inactive frames. |
| F2 | V2 Structural | v3.css:418-419 | `mtd3-spin 28s linear infinite` AND `mtd3-spin 18s linear infinite reverse` on Frame 1 rings — two counter-rotating circles. Visually "decorative tactical" but adds nothing to "we choose the topic". The metaphor is "we lock onto the right thing" — locking is static, not spinning. | Cut the spin. Static rings + center pulse is the metaphor. |
| F3 | V1 Drag | v3.css:431 | `mtd3-reel` rotation `22s linear` — film reels DO spin. This is appropriate. But it spins continuously and never accelerates with scroll. Should it? | Tie rotation speed to scroll velocity for delight. Optional V0; keep as is if simplicity wins. |
| F4 | V2 Structural | v3.js:199-203 | `if (reduceMotion) { frames.forEach(f => f.classList.add('is-active')); setActive(0); return; }` — sets ALL frames `.is-active` simultaneously, returns. But the desktop CSS positions `.mtd3__frame { position: absolute; inset: 0 }` — all 5 frames absolutely-positioned at the same coordinates means they STACK. Reduced-motion users see 5 frames overlapping at the same X/Y. | In reduce-motion path, also disable the `position: absolute` styling (e.g. add a `.mtd3--reduced` class on root that resets to `position: relative` flex-column). |
| F5 | V1 Drag | v3.css:434 | `.mtd3-waves circle` `transform-origin: 200px 260px` — the SVG has cy="270" not 260. Off by 10px. Waves emanate slightly above the tower base instead of from it. | Match: `transform-origin: 200px 270px`. |

### G. RTL (5 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| G1 | V3 Critical | v3.css:467-468 | **The slide direction comment in CSS admits confusion:** "in flipped origins; translation done physically below". A senior developer doesn't write apologetic comments — they fix the abstraction. The current code physically translates `+42vw` (rightward in screen pixels) regardless of `dir`. In an LTR mirror of the same component, the icon would also slide from the right — which is wrong, because LTR naturally puts the icon on the right. | Use `translate(var(--shift-inline), 0)` where `--shift-inline: calc((1 - var(--mtd3-slide)) * 42vw)` is set on `[dir="rtl"]` and `calc((1 - var(--mtd3-slide)) * -42vw)` on `[dir="ltr"]`. CSS logical properties don't help with translate; an explicit dir-aware variable does. |
| G2 | V2 Structural | v3.css:526 | `.mtd3__cta svg { transform: scaleX(-1) }` — flipping the arrow icon horizontally. Reasonable RTL adjustment for an arrow-right SVG. BUT the SVG path itself is `<path d="M23 6H1m0 0l6-5..." />` — already drawn rightward. After scaleX(-1) it points left, which is "forward" in RTL. OK. But: `transform-origin` defaults to center, so flipping is fine. Confirmed correct. | No fix — this is correctly handled. (Documenting that it works.) |
| G3 | V1 Drag | v3.css:407 | `.mtd3__kicker::before` is a `width: 28px; height: 1px` decorative line. With Flexbox `gap: 10px` it lands BEFORE the kicker text in source order, which in RTL is to the RIGHT of the text. The kicker reads "—— المرحلة الأولى · الاختيار". Looks correct. | No fix — confirmed correct. |
| G4 | V2 Structural | v3.css:535 | Mobile (≤900px) `.mtd3__num` has `text-align: start; margin-block-end: -30px` — overlaps content below it. The `-30px` margin pulls subsequent content up into the numeral's bottom. At small sizes, art SVG below it can collide. Visible in `mtd3-390.png` — the numeral "01" sits cleanly above kicker, but at viewports between 600-900 the numeral's descender (the `1`) intersects the meta list. | Drop the negative margin. Use grid or explicit gap instead. |
| G5 | V0 Cosmetic | v3.css:377 | HUD `direction: ltr` forced on `.mtd3__progress`. Necessary because Arabic numerals ٠١-٠٥ need to read left→right (start→end). But the HUD as a whole is in an RTL section. Visual alignment of "٠١ —— bar —— ٠٥" reads correctly as 1-on-left, 5-on-right. | No fix — correct. |

### H. ACCESSIBILITY (6 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| H1 | V3 Critical | v3.css:454-455, browser behavior | **5×100vh pinned scroll = 5 viewport heights of mandatory scroll to pass through.** No skip mechanism. Keyboard users (Tab/Page-Down) navigating to the next section after `#method` must scroll through 4500px. There is no "skip" link, no anchor at the outro. | Add `<a class="visually-hidden focusable" href="#content">تخطّى المنهج</a>` immediately before the stage. Must be focusable from keyboard. |
| H2 | V2 Structural | index.html:469-473 | HUD progress lacks `role="progressbar"`, `aria-valuenow`, `aria-valuemin`, `aria-valuemax`, `aria-valuetext`. Currently `aria-hidden="true"` (line 469). For a sighted-keyboard user the progress bar is the only feedback during the long scroll. | Either: keep aria-hidden and accept the bar is decorative; OR remove aria-hidden, add `role="progressbar"` + `aria-valuenow={frame index}` + `aria-valuetext="المرحلة [n] من ٥"` updated by JS. |
| H3 | V2 Structural | v3.js:199-203 | **Reduce-motion handling is broken** (see F4) — sets all frames active but they're absolutely-positioned overlapping. Users with `prefers-reduced-motion: reduce` see a stack of 5 invisible/overlapping frames at the same X/Y. | Per F4 fix: also add a reduce-motion stylesheet that resets `position: absolute` to `relative` and stage height to auto. |
| H4 | V2 Structural | index.html:484 | `<div class="mtd3__track" role="list">` then `<article role="listitem">` × 5 — semantically OK. But `<article>` already implies its own document landmark; pairing with `role="listitem"` confuses screen readers (NVDA reads "list, article, listitem"). | Use `<ol>` with `<li>` for the track, OR drop `<article>` and use `<div role="listitem">`. Article is wrong here — these aren't standalone documents. |
| H5 | V1 Drag | index.html:475-483 | `.mtd3__rail` `<nav aria-label="مراحل المنهج">` is rendered with `display:none` (CSS:382). Nav element with `display:none` is still announced by some screen readers as "navigation: phase navigation" then nothing else. Confusing. | Remove the nav from the DOM (delete lines 475-483). It's dead markup per vanity-engineering pattern. |
| H6 | V1 Drag | index.html:498, 585, 648, 762 | All `.mtd3__art` SVGs have `aria-hidden="true"` ✓. But the `.mtd3__art-tag` text inside them ("TARGETING · ٠١") is not aria-hidden and announces in screen readers as "TARGETING zero one" without context. | Add `aria-hidden="true"` to `.mtd3__art-tag` spans, OR move the tag into the visually-hidden screen-reader-only label. |

### I. MOBILE (≤900px) (4 findings)

| # | Severity | File:line | Finding | Fix |
|---|---|---|---|---|
| I1 | V1 Drag | v3.css:534-538 | Mobile stack: each frame is `padding: clamp(48px,9vw,72px) clamp(20px,5vw,28px)` with `gap: clamp(24px,4vw,40px)`. 5 frames stacked = ~2700-3700px of vertical content. Discovery shows mtd3 height @390 = 3772px. Long boring scroll. Visible: `mtd3-390.png` shows clean intro + frame 1 — adequate, but multiplied by 5. | Reduce padding by 25%. Use `padding: clamp(36px,7vw,56px) clamp(16px,4vw,24px)`. Reading payload doesn't justify the breathing room here. |
| I2 | V1 Drag | v3.css:534 | `border-block-end: 1px solid var(--hairline)` between mobile frames. Last frame also gets the border (no `:last-child` exception) — creates a hairline above the outro that's slightly stronger than the outro's own `border-block-start` separator. Doubled rule. | `.mtd3__frame:last-child { border-block-end: 0 }`. |
| I3 | V0 Cosmetic | v3.css:537 | `.mtd3__art { max-width: 320px; justify-self: center; margin-block-start: 24px }` — at 390px viewport, 320 + 56px frame padding = exceeds container. Art touches the side rule. | `max-width: clamp(220px, 70vw, 320px)`. |
| I4 | V0 Cosmetic | v3.css:533 | Mobile track has its own `border: 1px solid var(--hairline); border-radius: var(--r-xl)` — adds an outer card border around all frames. Combined with per-frame `border-block-end` (I2) creates visible doubled lines at the seams. | Drop track outer border; keep per-frame separators. Or vice-versa. |

---

## What Works Well (preserve)

- The intro `.mtd3__title` typography, max-width: 18ch, and red `<em>` on "إلى قصّة لا تُنسى" — clean editorial pattern.
- Color discipline: 5 distinct hues for 5 phases, one per frame, used consistently across kicker, glow, stroke, and HUD gradient. The PRINCIPLE is correct — execution has the duplication issue (E1).
- The magazine-cover frame's *concept* — using HTML/CSS instead of SVG so Arabic typography renders naturally — is right. (Just needs real content, B4.)
- HUD pill design language (mono-spaced, all-caps, .28em letter-spacing, ٠١ ⎯⎯ ٠٥) is appropriate for a "method panel". When it actually shows (after fixing D1) it will work.
- Mobile stacked layout is clean and legible (`mtd3-390.png`). The vertical reading order works.
- The intent of "icon enters from the right, settles, text reveals" is correct for a story-led scrollytelling pattern. The choreography just isn't delivering it (A1, A2).

---

## Priority Recommendations (for build agent)

**P0 — Critical (ship-blockers):**
1. **Fix HUD overlap with sticky header** (D1). Add `margin-top: var(--hdr3-height, 105px)` to `.mtd3__hud` desktop block.
2. **Fix slide collision between numeral and art** (A1). Pin `.mtd3__num` to its grid position; only `.mtd3__art` slides.
3. **Fix slide direction so icon actually starts off-screen right** (A2). Compute slide offset in JS as `(viewport_right - art_natural_left)` per frame, set as `--mtd3-slide-x` custom property.
4. **Fix reduce-motion stack** (F4 / H3). Add `.mtd3--reduced` class branch that resets stage to flow layout.
5. **Replace placeholder cover content** (B4). Use real cover content or 3-cover rotation.

**P1 — Refinement (significant elevation):**
6. **Single source of truth for color** (E1). Replace JS `colors[]` with `getComputedStyle(...).getPropertyValue('--mtd3-c')`.
7. **Reduce stage height** (D2). 5×100vh → 5×70vh = 3500px, less reading fatigue.
8. **Tighten reveal thresholds** (A5). Begin kicker at sub=.15, end meta at sub=.85.
9. **Bind animations to active frame** (F1). Pause `mtd3-spin/pulse/wave/strip/mess/clean/tilt` on inactive frames.
10. **Strip illustrative noise** (B1, B6, C10). Cut tactical readouts, frequency labels, redundant ordinals.

**P2 — Polish:**
11. Skip link to outro (H1).
12. Progress bar a11y (H2).
13. Frame 4 color match (E3).
14. Drop dead `.mtd3__rail` markup (H5).
15. Delete unused `@property --mtd3-progress` and `--mtd3-active` (vanity table).
16. Line-height fixes on Arabic titles (C1, C2, C8, C9).
17. Cover tilt only when active (B5).
18. Continuous progress fill (A8, A9).

---

## Hard Question

If you remove the scrollytelling entirely and replace this section with a 3-column horizontal card layout (5 cards, each: numeral, kicker, h3, body, meta) at 1×100vh — how much editorial value do you lose?

Hypothesis: zero. The reader knows the method has 5 phases either way. The scroll-pinned format costs 3000px of page height, ~80kb of CSS+JS, debugging time on every browser update, and keyboard-user friction. The static layout costs nothing and reads in 5 seconds instead of 30. The only thing it loses is the "look how clever we are" engineering moment — which is precisely what vanity-engineering review exists to surface.

The recommendation isn't necessarily to delete the scrollytelling. It's to confirm the choice is editorial, not engineering. If the team can't articulate what scroll-pinning communicates that a static layout cannot, the section should be cut down to one viewport.

---

## Summary (200 words)

The METHOD section is the most ambitious work on the page and currently the most broken. Three structural defects — HUD pill hidden by the sticky header, the giant outlined numeral colliding with the SVG icon during the slide, and the slide starting from centre rather than from the right — undermine the entire choreography in every live capture. Beneath these, vanity-engineering accumulates: 8 always-running animations regardless of frame visibility, two sources of truth for the 5-color palette (CSS and JS duplicating hex codes), dead `<nav class="mtd3__rail">` markup hidden via display:none, two unused @property declarations, an apologetic CSS comment confessing the RTL math is hand-rolled. The 5×100vh stage occupies 34.5% of total page height for a reading payload that fits in a single static viewport. The magazine-cover frame contains placeholder Arabic that breaks the only frame asserting "this is what we make". The reticle, film reel, and broadcast tower SVGs are visually inconsistent — each in its own vocabulary, none cohering as a system. The intent — a confident editorial-method reveal — is right. The execution needs P0 fixes before this section can read as billion-dollar craft instead of engineer-impressed-with-CSS-pin.
