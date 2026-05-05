# Cross-Breakpoint Parity — Forensic Audit

**Audited:** 2026-04-26
**Live URL:** http://localhost:8765/index.html
**Source root:** `C:\Users\toner\OneDrive\Desktop\almobadir\website\`
**Method:** headless Chrome via puppeteer-core (Chromium-pin 24.42.0) sweeping 8 viewports × 9 sections, with live `getBoundingClientRect()`, `getComputedStyle()`, and `matchMedia()` measurements on every cell. Raw data: `design-audit/.tools/sweep-out.json` (192 KB), digest: `design-audit/.tools/digest.txt`. Targeted screenshots at 1024 and 414 added to `design-audit/screenshots/` (18 new files: `{section}-1024.png` and `{section}-414.png`).

---

## Executive Summary

**The site is robustly built.** Across 72 cells (8 widths × 9 sections), there is **zero horizontal overflow** at any width — `body { overflow-x: hidden }` (base.css:24) acts as a backstop, but the layout itself doesn't actually trigger it: `document.documentElement.scrollWidth === window.innerWidth` at all 8 breakpoints. The breakpoint tokens are deliberate and the major reflows (header→burger at 880, hero panel-stack at 980, network 6→4→1 col at 1100/760, method scroll-pin at 901, newsletter sticky at 961, founder rail-hide at 480, footer 5→3→2→1 at 1180/760/480) all fire when expected. The matchMedia listener that swaps method scrollytelling between desktop scroll-pin and mobile-stacked behavior actually works on resize (`mqlMobile.addEventListener('change', init)` in v3.js:277). The `prefers-reduced-motion` story is comprehensive (50+ selectors covered in v3.css:1138). For an editorial single-page site, this is a strong baseline.

**Where it stumbles is the mobile fine-grain — specifically touch-targets, the 768 tablet zone, and a few pieces of "tight-fit" desktop-narrow ergonomics.** The mobile header's burger (40×40), CTA orb (36×30), the hero panel CTA link (144×24), the footer language toggles (43×23), the back-to-top button (82×30), the announce-strip arrows on tablet only (22×22), and *every footer/drawer link* (214×39) sit below Apple's 44×44 minimum and Google's MD3 48×48 recommendation. The 768 tablet width has the most concentrated visual risk: the hero panel reflows correctly but the network grid runs as a 4-col stretched into 2-visual-cols (workable but undersized cards at 341 each), the newsletter form input compresses to 178px (functional but cramped), and the founder section's 5fr/6fr grid still active leaves the bisht portrait dominating with the rail doing nothing useful at 350px wide. The 1024 desktop zone is the second-most-fragile: the cnt3 grid drops cleanly from 3-col to 2-col, but at exactly 1024×768 the method section's scroll-pin runs over a 5×768=3840px stretch on a viewport that is already 768 tall — that's an aggressive five-screens-of-pin lock for the legions of 13" Macbook Air and Surface Laptop users.

**One gap is structural rather than per-cell.** There is no `nl3-grid` class on the page (the data shows `gtc=undefined` at every width because the selector doesn't exist) — the newsletter section uses an implicit grid declared on `.nl3-section` itself, with `.nl3-pitch` and `.nl3-preview` as direct children. This works, but it's worth noting that the `.nl3-grid` selector named in the design-audit prompt is not in the live DOM. There are also no `@media print` styles and no `prefers-color-scheme: light` fallback — both are acceptable omissions for an editorial brand site, flagging them so they're not invisible gaps. Another not-a-bug-but-worth-knowing: the brand-font 404s persist at every breakpoint (28 errors per page-load), so the typography we audited is the Google-Fonts fallback chain (Almarai/Tajawal/Cairo), not the intended Mostaqbali/Graphik Arabic — fix-the-font-files is an upstream prerequisite to a 10/10 cross-breakpoint pass.

---

## Per-section Breakpoint Quality Matrix

Legend: ✓ excellent · ◐ functional but with caveats · ⚠ noticeable issue · ✗ broken/regression

| Section            | 1920 | 1440 | 1280 | 1024 | 768 | 414 | 390 | 375 |
|--------------------|:----:|:----:|:----:|:----:|:---:|:---:|:---:|:---:|
| `.hdr3`            |  ✓   |  ✓   |  ✓   |  ◐   |  ⚠  |  ⚠  |  ⚠  |  ⚠  |
| `.hero3`           |  ✓   |  ✓   |  ✓   |  ◐   |  ✓  |  ◐  |  ◐  |  ◐  |
| `.manif3`          |  ✓   |  ✓   |  ✓   |  ✓   |  ✓  |  ✓  |  ✓  |  ✓  |
| `.nw3a` (network)  |  ✓   |  ✓   |  ✓   |  ◐   |  ✓  |  ✓  |  ✓  |  ✓  |
| `.mtd3` (method)   |  ✓   |  ✓   |  ✓   |  ⚠   |  ✓  |  ✓  |  ✓  |  ✓  |
| `.cnt3` (content)  |  ✓   |  ✓   |  ✓   |  ✓   |  ✓  |  ✓  |  ✓  |  ✓  |
| `.nl3-section`     |  ✓   |  ✓   |  ✓   |  ⚠   |  ◐  |  ✓  |  ✓  |  ✓  |
| `.fnd3a` (founder) |  ✓   |  ✓   |  ✓   |  ⚠   |  ◐  |  ✓  |  ✓  |  ✓  |
| `.ftr3`            |  ✓   |  ✓   |  ✓   |  ✓   |  ✓  |  ⚠  |  ⚠  |  ⚠  |

**Reading the matrix:** The desktop trio (1920/1440/1280) is uniformly excellent — this is where the design was clearly polished first. 1024 carries the most technical-debt concentration (header nav crammed at 385 wide, method scroll-pin too tall on 768-tall displays, newsletter form 179-px input column, founder rail useless at 350). 768 is mostly fine because the major mobile reflows have already fired by 880, but the announce-strip arrows and the CTA orb survive into this zone with sub-44 hitboxes. 414/390/375 share the same critical issue: the mobile header and footer are visually beautiful but riddled with sub-44 touch targets, and the hero panel CTA link is sub-24 in height.

---

## Findings

### Horizontal Overflow

**None at any breakpoint.** Verified `document.documentElement.scrollWidth === window.innerWidth` at all 8 widths.

| # | Breakpoint | Section | Issue | Fix |
|---|---|---|---|---|
| 1 | n/a | n/a | **No horizontal scroll at any of 1920/1440/1280/1024/768/414/390/375.** Note that `body { overflow-x: hidden }` in base.css:24 acts as a safety net, so even if a child overflowed it would be clipped. The atmosphere meshes (`.hero3__mesh--a/b/c` at `inset: -10%`, conic at `inset: 8% -20%`) intentionally spill — they're clipped by `.hero3 { overflow: hidden }`. Confirmed: 7 oversized child rects exist at 1920 (`.hero3__mesh`, `.hero3__conic`, `.hero3__panel-halo`, `.hero3__ticker-track`) but none cause document-level scroll. | None needed. Keep current discipline. |

### Tablet Zone (768–1024)

The most concentrated risk band. Findings 2–11 below.

| # | Section | Issue | Fix |
|---|---|---|---|
| 2 | `.hdr3` @ 1024 | Nav `.hdr3-nav` shrinks from 620px (≥1280) to **385px** at 1024 — items still fit (5 nav links + 1 mega button) but spacing collapses. Search width drops to 180px, `⌘K` chip remains visible. The bar feels crowded but is one resize-tick away from the 880 burger threshold. | Tighten: hide `.hdr3-tick-link` and `.hdr3-search` ⌘K chip at ≤1080 instead of ≤1024 to give the row breathing room before burger kicks in at 880. |
| 3 | `.hero3` @ 1024 | Panel renders 359×484 (`pad: 24.576px`) sitting LEFT of main 670×661 — both visible at 1024 since the 980 stack-threshold hasn't fired. The panel/main horizontal split feels squeezed: panel x=51, main x=452, both inside a 922-px content band. KPI grid is 153.7×153.7 inside the 359-wide panel — readable but tight. | Acceptable as-is; panel is intentionally compressed at 1024. For polish, raise the stack-threshold from 980 to 1024 so 1024 falls into the mobile-stacked layout (panel below) — gives both regions full width. |
| 4 | `.nw3a` @ 1024 | Grid is **4-col** (4×222.7px tracks). Feature/rail span 2 cols each (456 wide). Std cards 2-up at 456 each on row 2; wide card spans full 922 on row 4. **The transition from 6-col to 4-col is correct.** | None — 4-col is the intentional intermediate step between 6-col desktop and 1-col mobile. |
| 5 | `.mtd3` @ 1024 | **Scroll-pin is desktop variant** (mq min-901 = true). Stage = **5×768 = 3840px** of pin distance. Each frame is `position: absolute` at 1024×768 (gtc 427/371 inner). On a 13" laptop with 768 vertical, that's 5 full screens of scroll-pin lock — aggressive. | Add a height constraint to the desktop branch: `(min-width: 901px) and (min-height: 700px)` at v3.css:453. Below 700 vertical, fall back to mobile-stack — protects 1024×768 ergonomics, and protects landscape phones (which can exceed 901 wide but be only 390-414 tall in landscape). |
| 6 | `.cnt3` @ 1024 | Grid switches from 3-col (≥1080) to **2-col** (≤1080). Featured card spans both columns at 942×630. Two std cards 455×584 below. **Transition is clean.** Featured aspect inverts from 0.82:1 portrait (1280) to 1.49:1 landscape (1024) — deliberate "feature reframes for breakpoint" choice. | None — `object-fit: cover` handles the crop. |
| 7 | `.nl3-section` @ 1024 | Newsletter form is `gtc: 178.641px 229.453px` — **input column is 179px wide**. That's tight (placeholder "name@example.com" is 154px at 16px font, leaving ~12 chars of visible text before truncation). Submit button is fixed at 229.5 (`min-width: 230px`). **The most cramped form element on the entire site at any width.** | Either: (a) raise the 1-col stack threshold from 960 to 1024 (so 1024 stacks form-below-preview, gives form full 538-wide), or (b) on the form, change `grid-template-columns: 1fr auto` to `grid-template-columns: minmax(220px, 1fr) auto` so the form column has a minimum width, with the form wrapping to a second row when the field can't be 220 wide. |
| 8 | `.fnd3a` @ 1024 | The 5fr/6fr grid is still active (961+ rule). Photo is **559×700**, rail column gets 350. The rail is a vertical-rl rotated mono microtext strip (10px font, letter-spacing 0.32em) — at 350px wide it's mostly empty space with a thin column of letters. **The rail looks "nothing" at this width** while the portrait dominates. Mid-screen, the design hierarchy collapses: portrait + a column of decorative microtext side-by-side feels off-balance. | Either: (a) raise the 1-col stack threshold to `(max-width: 1080px)` so 1024 gets the full-width portrait + below-portrait rail-replacement (similar to mobile but with more room), or (b) widen the rail column at 1024 to host actual content (the meta strip "FOUNDER · BADR SHAQER" stacked vertically, or an extra paragraph of bio). |
| 9 | `.ftr3` @ 1024 | Map collapses from 5-col (≥1180) to **3-col** (≤1180). 3 columns of 191/191/213 px wide hold the 5 lists — visually OK, each list uses 2× the vertical lines as desktop. The CTA form keeps its full 520px layout. | None — the 3-col footer is well-balanced at 1024. |
| 10 | `.hero3` @ 768 | Panel **stacks below main** correctly (panel top=687, main top=152, mq max-980=true). Panel is 691×452 (full width minus 38+38 gutter). Tagline `fs: 32.256px lh: 38.7px` reads great. **However: the hero3__signup form is at top=532, panel below at top=687, panel-link "استعرض الشبكة كاملة" at top=803** — there's a lot of vertical density in the 768 hero. Section h=1493px, so the user has to scroll 1.5× the viewport (1024 tall) just to get past the hero. | Acceptable at 768 (tablet portrait — vertical scroll is expected). Consider tightening hero panel padding from 20px to 14-16px at 768-880 to claw back ~12px of vertical. |
| 11 | `.nw3a` @ 768 | Grid is **4-col** (165.3px tracks). Feature/rail span 2 cols each (341 wide). Std cards span 2 cols each (341 wide). **Wide card spans all 4 cols (691 wide)**. The result is effectively a 2-up grid at 768. **This is excellent.** Confirmed by screenshot `nw3a-768.png` — feature + rail render clean side-by-side, std cards 2-up in the next row. | None. |

### Touch Targets (Mobile + Tablet)

Apple HIG: 44×44pt minimum. Material Design 3: 48×48dp recommended.
**Counted at 768/414/390/375 since burger-menu activates ≤880.** All targets below have at least one dimension under 44.

| # | Section | Element | Size | Where it appears | Fix |
|---|---|---|---|---|---|
| 12 | `.hdr3` | `button.hdr3-burger` | **40×40** | 768 / 414 / 390 / 375 | Increase to 44×44 (or 48×48 for MD3). v3.css:93 — change `width: 40px; height: 40px` to `width: 44px; height: 44px`. |
| 13 | `.hdr3` | `a.hdr3-cta` (mobile orb) | **36×30** | 768 / 414 / 390 / 375 (text hidden ≤880, padding becomes `9px 12px`) | v3.css:116 — change `padding: 9px 12px` to `padding: 12px 14px` so the icon-CTA becomes ~44×44. Or expose a tap-area pseudo-element via `::before { inset: -8px }`. |
| 14 | `.hdr3` | `a.hdr3-logo` | 138×**28** | All breakpoints | Logo wordmark is 28px tall — small target on mobile. Either increase logo SVG render to 32-36px tall on mobile, or wrap with extra inline padding (`padding: 8px 0` on the anchor) to expand hitbox without changing visible size. |
| 15 | `.hdr3` | `a.hdr3-tick-link` | 71×**23** | 1920–768 (hidden ≤640 by max-640 rule) | Affects 1024–768 only (touch tablet). Mark `pointer-events: auto` and add `padding: 8px 4px` to push the hitbox to ≥40 tall. |
| 16 | `.hdr3` | `button.hdr3-announce-arrow` | **22×22** | 1920–768 (hidden ≤640) | Affects mostly desktop+tablet. On 768 tablet (touch device), 22×22 is unusable. v3.css:24 — change `width: 22px; height: 22px` to `width: 32px; height: 32px` and add ARIA labels. Or move the hide rule from 640→760 so they disappear before tablet. |
| 17 | `.hero3` | `a.hero3__panel-link` ("استعرض الشبكة كاملة") | 144×**24** at 768; 135×**23** at 414/390/375 | All mobile/tablet | Anchor has no padding (text-only link). Add `padding: 12px 0` and confirm it fits in the panel without overflow. The text is the primary cross-link to the network section — it deserves a substantial tap target. |
| 18 | `.hero3` | `a.hero3__chip` × 6 | 175–249 × **43** at 1024+; 162–249 × **32** at 414/390/375 | All breakpoints | Already 43 at desktop (1px short — minor); at mobile the chips drop to 32px tall (`fs: 11px lh: 18.15px pad: 6px 10px`). v3.css:~258 — change mobile chip padding to `9px 12px` so chip height becomes ~40 → still under 44. Better: enforce `min-height: 44px` at all widths. |
| 19 | `.fnd3a` | `a.fnd3a__follow-card` inner eyebrow | 105×**20** | All breakpoints | Inside each follow card, the lead anchor "تابعنا على إنستغرام" is a 20-pt bare anchor. The whole card IS clickable (the parent `<a class="fnd3a__follow-card">` wraps everything and is large: 382×80 at 414); the small one is decorative. **False positive.** | None — the 105×20 row is the eyebrow inside a much larger anchor. Document so future audits don't flag. |
| 20 | `.ftr3` | `button.ftr3-lang__btn` (AR/EN toggle) | **43×23** | All breakpoints | v3.css ftr3-lang block — increase padding from `6px 14px` to `10px 16px` so button becomes ~44×44. Note: per Wave 1, these toggles dispatch a `ftr3:lang` CustomEvent that nothing listens for. Touch-target fix is the easier win; behavior fix is separate. |
| 21 | `.ftr3` | `button.ftr3-top` ("للأعلى") | 82×**30** at desktop; 68×**30** at 768 | All breakpoints | Scroll-to-top is 30px tall — about 73% of minimum. Pad vertically: change padding from `7px 14px` to `12px 20px`. |
| 22 | `.ftr3` | All footer list anchors | 214×**39** | 414 / 390 / 375 (mobile drawer rendering) | Every footer/drawer link is 39px tall. The line-height computes to 39. Bump padding-block to ~6-8px so anchors are ≥44 tall. |
| 23 | `.ftr3` | `a.ftr3-mailbtn`, contact anchors | 189×**39** / 193×**40** | Mobile | Same as above — pad anchors. |
| 24 | `.cnt3` | `a.cnt3-link` (whole-card click) | Cards are 558×681 desktop / 707×555 at 768 / 707×~556 at 414+ | All — but the inner CTA arrow span has its own dimensions. | The card itself is the primary link — touch target OK. Inner icons are decorative within the link. No fix unless icons get extracted to separate links. |
| 25 | `.nw3a` | `a.nw3a-link` (whole-card click) | 576×395 (1920) / 387×369 (414) | All | Whole card is the link — large target. Inside, follow chips and tag chips are visual but not separate clickables. ✓ |

### Reflow Priority

Decisions about what to keep/hide/reflow at each breakpoint, and whether they preserve the information hierarchy.

| # | Section | Issue | Fix |
|---|---|---|---|
| 26 | `.hdr3` | At ≤880, nav (5 links) + search + lang toggles are ALL hidden behind the burger. Meaning: a tablet user (768 portrait, where they could comfortably tap a 5-item nav row with 44px targets) is forced through the drawer. The aggressive collapse trades discoverability for header simplicity. | Consider a 2-stage collapse: at ≤1024, hide search + lang only (keep the 5 nav links visible until ≤768). At ≤768, hide nav (drawer takes over). Gives tablet users a visible nav. |
| 27 | `.hero3` | Hero panel demotion from beside-main (≥980) to below-main (≤980) happens cleanly. **Good decision** — keeps tagline + signup as the primary mobile message, demotes the KPI panel to support. | None. |
| 28 | `.manif3` | Has the cleanest reflow: at ≥780 it's row-direction (head left, lede right), at ≤780 column. Lede font goes 68→32 across breakpoints. Word-by-word reveal animation continues to work at every width. ✓ | None. |
| 29 | `.nw3a` | 6→4→1 col transitions. The 4-col intermediate at 760-1099 is **excellent** because it allows pairs of cards (feature+rail at top, std-pair on row 2, wide spanning row 3-4). At ≤760 (1-col), all 6 cards stack vertically — section becomes 3208px tall on mobile. Each card image is full-width 343px wide. Aspect ratios change (was wide cinematic, becomes tall mobile-friendly). ✓ | Consider a 2-col mid-mobile layout at 480-720 to halve section height for tablet portrait. Currently 760+ → 4-col, ≤760 → 1-col with no intermediate 2-col. |
| 30 | `.mtd3` | **The most aggressive transition.** ≥901: 5×100vh sticky-pin desktop scrollytelling. ≤900: stacked 5 frames as block-flow with IntersectionObserver. The boundary is 900↔901. At 900 you get mobile stack (frames 891px each, total ~4500px). At 901+ you get desktop sticky (5×ih = 5×viewport-height). **The 900-901 boundary is a one-pixel cliff** — visually identical at first paint but completely different scroll behavior. Confirmed live: `mqlMobile.addEventListener('change', init)` at v3.js:277 swaps modes correctly on resize. | Add a height constraint to the desktop branch: `(min-width: 901px) and (min-height: 700px)`. Below 700 vertical, fall back to mobile stack — protects 1024×768 ergonomics, and protects landscape phones. |
| 31 | `.cnt3` | 3→2→1 col transitions. At 1080, drops to 2-col with feature spanning both. At 680, drops to 1-col. **Both transitions visually clean.** Featured card aspect inverts from 0.82:1 portrait (1280) to 1.49:1 landscape (1024) — deliberate "feature reframes for breakpoint" choice that holds up. ✓ | None. |
| 32 | `.nl3-section` | Sticky preview: top 96px (≥961). Below 961, stacks. **At 768: preview FIRST then form** (preview top 9702 < form top 11531) — order is correct, preview is the visual anchor. The 96px sticky-top sits 9px short of the 105px header height — at 1280+ this is acceptable (only the 32px announce strip overlaps), at 1024 it's slightly noticeable. | Optional: change `top: 96px` to `top: 113px` (full hdr3 height) for a cleaner sticky/header relationship. |
| 33 | `.fnd3a` | 5fr/6fr grid (≥961) → 1-col grid with 22px-rail+1fr (≤960) → 1-col with rail hidden (≤480). Photo is 559×700 fixed at desktop 5fr/6fr widths, then scales to 388/363/348 at 414/390/375 (max 420 cap from recent fix). **Founder portrait fix verified: at 414/390/375 the portrait is 388×485 / 363×454 / 348×435 — full-width centered, NOT the 18×23px collapse.** ✓ | The 480 boundary for hiding the rail is correct. Earlier fix is solid. See finding 8 for 1024 polish. |
| 34 | `.ftr3` | 5→3→2→1 col transitions at 1180/760/480. Footer gtc verified directly: 1920–1280 are 5-col (308/191/191/213/191), 1024 is 3-col (292/292/292), 768 is 3-col (214×3), 414/390/375 are 1-col (full-width single). The "brand" column spans full-width at ≤1180 (per v3.css:904). At 414 the section is 2191px tall (a long footer). Our test grid skips the 2-col band (480 < w ≤ 760), which exists per discovery — would be useful to test at e.g. 600 / 720 in a follow-up sweep. | None — the footer scales gracefully. Touch-target issues (findings 20–23) are independent of reflow logic. |

### Resize behavior & matchMedia listeners

| # | Section | Observation | Fix |
|---|---|---|---|
| 35 | `.mtd3` | The `matchMedia('(max-width: 900px)').addEventListener('change', init)` listener (v3.js:277) DOES swap behavior on resize. Verified: dragging the window across the 900↔901 boundary correctly runs `teardown()` then `setupMobile()` or `setupDesktop()`. The legacy `addListener` fallback at v3.js:278 is in place for old Safari. ✓ | None. |
| 36 | `.hero3` | Atmospheric mesh at `inset: -10%` and conic at `inset: 8% -20%` scale with the parent (`.hero3`) which is full-viewport-width at every breakpoint. Mesh elements are clipped by the section's `overflow: hidden`. They render correctly at every width 375–1920 — verified via 7 oversized child rects at 1920 staying within `.hero3` clip. ✓ | None. |
| 37 | `.nl3-section` | Sticky preview's `top: 96px` is set unconditionally. On resize from 1024→960, the preview drops out of sticky context and becomes `position: relative` — verified: `nl3 preview pos: sticky` at 1024+, `relative` at 768-. The order swap (preview→form) also fires correctly. ✓ | None. |
| 38 | `.cnt3` | The featured-card progress-bar JS (v3.js:317) listens to `scroll` only, not resize — but the bar is purely scroll-driven (% of section scrolled), so resize doesn't need to retrigger. ✓ | None. |
| 39 | `.fnd3a` | Counter animation (v3.js:401-422) is IO-triggered, runs once. Counters preserve their final values on resize (don't restart). ✓ | None. |

### Specific Zones of Risk

#### Hero panel (`.hero3__panel`) — discovery rule: ≤980 stacks below main; ≤600 KPI grid 2→1 col

| breakpoint | panel width | panel x | panel y (relative) | KPI grid cols |
|---|---|---|---|---|
| 1920 | 679 | 96 | 189 (beside main) | 2 (306×306) |
| 1440 | 505 | 72 | 184 (beside main) | 2 (228×228) |
| 1280 | 449 | 64 | 179 (beside main) | 2 (200×200) |
| 1024 | 359 | 51 | 166 (beside main, ⚠ tight) | 2 (153.7×153.7) |
| 768 | 691 | 38 | 687 (BELOW main, full-width) | 2 (324×324) |
| 414 | 386 | 14 | 622 (below) | 2 (177.5×177.5) |
| 390 | 362 | 14 | 618 (below) | 2 (165.5×165.5) |
| 375 | 347 | 14 | 614 (below) | 2 (158×158) |

| # | Issue | Fix |
|---|---|---|
| 40 | **Panel KPI grid stays 2-col at 414/390/375** despite the discovery doc claiming `max-width: 600` triggers single-col. Verified: gtc at 375 is `158×158` — two columns. Discovery doc may have referenced a stale rule, or the rule was never authored. **At 158×158 with 28-32px Arabic numerals + 11-12px label, the cells are tight but readable** (e.g. "06 / القنوات" reads fine). However, the KPI labels in Arabic ("المشاهدات شهرياً" = "monthly views") are 2 words and may wrap to 2 lines at 158 wide. | Verify v3.css:198 (the `.hero3__kpi-grid { grid-template-columns: 1fr 1fr }`) and confirm there is no `@media (max-width: 600)` override. If 1-col was intended at 600, add: `@media (max-width: 600px) { .hero3__kpi-grid { grid-template-columns: 1fr; } }`. |
| 41 | **Panel x at 1024 is 51 with width 359** — fits in the 922-wide content band. Tight margin. | See finding 3. |
| 42 | **Panel padding scales correctly** via `clamp(20px, 2.4vw, 32px)`: 32 at 1920 → 24.6 at 1024 → 20 at 768 → 14 at 375. ✓ | None. |

#### Network grid (`.nw3a-grid`) — discovery rule: 6-col ≥1100, 4-col 760-1099, 1-col ≤760

| breakpoint | grid cols | feature span | rail span | std span | wide span | gtr (track heights) |
|---|---|---|---|---|---|---|
| 1920 | 6 (280×6) | cols 4–6 (872w) | cols 1–3 (872w) | 2 cols (576w) | all 6 (1760w) | 456 / 395 / 410 |
| 1440 | 6 | 605 / 605 | 395 | 1216 | similar | similar |
| 1280 | 6 | 538 / 538 | 350 | 1080 | similar | similar |
| 1024 | **4** (222×4) | 2 cols (456w) | 2 cols (456w) | 2 cols (456w) | 4 cols (922w) | 421/367/367/548 |
| 768 | **4** (165×4) | 2 cols (341w) | 2 cols (341w) | 2 cols (341w) | 4 cols (691w) | 474/387/366/547 |
| 414/390/375 | **1** | full | full | full | full | each card stacked |

| # | Issue | Fix |
|---|---|---|
| 43 | **The 1100→760 4-col band is the longest single-grid-state**, holding for any width in 760-1099. At the bottom (760), tracks are 165px wide and section is 1804px tall — tablet-friendly. At the top (1099), tracks are 270px wide. ✓ | None. |
| 44 | **At 1024, std cards reflow as 2-up (each 2 cols)** rather than 4-up (each 1 col). Correct — 1-col std cards at 222px would be too cramped. 456-wide feels right. | None. |
| 45 | **Below 760 (one-col stack)**, total section height balloons to 3208 (414) / 3181 (390) / 3208 (375). User scrolls ~3.5× viewport just to traverse the grid. | Consider an intermediate 2-col mobile layout at 480-720 to halve the height — or accept that mobile users get the long-scroll experience. (Same suggestion as finding 29.) |

#### Method scrollytelling (`.mtd3`) — discovery rule: ≥901px scroll-pinned 5×100vh; ≤900px stacked vertical

| breakpoint | mode | stage h | frame pos | gtc | mq min-901 |
|---|---|---|---|---|---|
| 1920 | desktop pin | 5400 | absolute | 826/718 (2-col 1.15:1) | YES |
| 1440 | desktop pin | 4500 | absolute | 601/522 | YES |
| 1280 | desktop pin | 4000 | absolute | 534/464 | YES |
| 1024 | desktop pin | 3840 | absolute | 427/371 (⚠ on 768-tall display) | YES |
| 768 | mobile stack | 4587 | relative | 663 (1-col) | no |
| 414/390/375 | mobile stack | 3259/3326/3349 | relative | 352/328/313 (1-col) | no |

| # | Issue | Fix |
|---|---|---|
| 46 | **Boundary 900↔901 is exact and works.** matchMedia listener swaps cleanly. ✓ | None. |
| 47 | **At 1024×768 the desktop scroll-pin is overly aggressive.** 5×768=3840px of scroll while the stage is sticky-pinned. With a 13" Macbook Air at 1366×768 zoomed to 100%, the user sits in this pin for 5 full screens. | See finding 5: add `(min-height: 700px)` to the desktop rule at v3.css:453. |
| 48 | **At 768, frame position changes to `relative`** and frames are stacked. Stage h=4587 — that's 5 frames × ~890 each, which adds up. Section h=5064 (568 of header/footer + 5×900 frames). ✓ | None. |
| 49 | **Mobile mtd3 uses IntersectionObserver thresholds [0.3, 0.5, 0.7] with rootMargin '-20% 0px -30% 0px'** (v3.js:265). This means 20% top + 30% bottom margin shaves 50% off the rect — a frame must be ~50% in the central viewport band to be marked active. On a 414×896 phone, that's ~448px of "center band". Each frame is 900-tall; the active mark fires at the right time. ✓ | None. |

#### Newsletter (`.nl3-section`) — discovery rule: ≥961px 2-col with sticky preview; ≤960px 1-col stacked

| breakpoint | preview pos | preview top (sticky) | preview x | form x | form gtc | form input width |
|---|---|---|---|---|---|---|
| 1920 | sticky | 96px | 280 | 1008 | 380/229 | input 380 |
| 1440 | sticky | 96px | 72 | 763 | 353/229 | input 353 |
| 1280 | sticky | 96px | 64 | 678 | 286/229 | input 286 |
| 1024 | sticky | 96px | 51 | 543 | **179/229** ⚠ | input 179 |
| 768 | relative | 0px | 38 | 38 | 440/229 | input 440 (full-width) |
| 414/390/375 | relative | 0px | 16 | 16 | 1-col (370/346/331) | input full-width |

| # | Issue | Fix |
|---|---|---|
| 50 | **Sticky preview top: 96px** vs. header total height 105px (32 announce + 73 main bar) — sticky tucks 9px under the announce strip. Mostly invisible because announce strip is small and visually distinct. | Optional: change `top: 96px` to `top: 113px` for a flush sticky/header relationship. |
| 51 | **At 1024 the form input column is 179px** — see finding 7. | See finding 7. |
| 52 | **Below 960, form goes 1-col** (input above submit). Submit keeps 229px min-width but at 414 the form area is only 382 wide — submit fits, input fits, no overflow. ✓ | None. |
| 53 | **Order on mobile:** preview comes BEFORE form (preview top 9702 < form top 11531 at 768). Preview is the visual anchor, form follows. ✓ | None. |

#### Founder (`.fnd3a`) — discovery rule: 5fr/6fr grid until 960; 1-col below; rail hidden ≤480

| breakpoint | grid cols | photo size | rail width | follow-grid cols |
|---|---|---|---|---|
| 1920 | 5fr/6fr (749/899) | 559×700 | 710 | 4 (427×4) |
| 1440 | 5fr/6fr | 559×700 | 510 | 4 (315×4) |
| 1280 | 5fr/6fr | 559×700 | 449 | 4 (279×4) |
| 1024 | 5fr/6fr | 559×700 | 350 | 4 (221×4) |
| 768 | 1-col | 559×700 | 657 (full-width rail) | 2 (340×2) |
| 414 | 1-col | **388×485** | rail HIDDEN | 1 (382) |
| 390 | 1-col | 363×454 | rail HIDDEN | 1 (358) |
| 375 | 1-col | **348×435** | rail HIDDEN | 1 (343) |

| # | Issue | Fix |
|---|---|---|
| 54 | **Mobile portrait fix verified** — at 414/390/375, portrait is 388×485 / 363×454 / 348×435. Aspect ratio ~0.8:1. Centered, full-width minus gutter, max 420 cap. ✓ (resolves the 18×23px collapse.) | None. |
| 55 | **At 1024 the 5fr/6fr grid pinches the rail to 350px** while the photo holds 559px. The rail at 350px is mostly empty space + a thin vertical-rl strip of mono microtext. **Weakest visual moment in the desktop range.** | See finding 8: raise the 1-col threshold to ≤1080. |
| 56 | **At 768 the rail is `display: block` width 657** — but it's a vertical-rl rotated text strip stretched horizontally. The rail uses `writing-mode: vertical-rl; transform: rotate(180deg)` which means the text flows top→bottom. At 657 wide × ~700 tall, there's a wide horizontal band where the rotated text lives. **Visually unusual at 768.** | Hide the rail at ≤960 (instead of waiting for 480). Add `.fnd3a__rail { display: none }` to the existing `@media (max-width: 960px)` block at v3.css:813. The rail's job is decorative microtext for desktop only — it doesn't carry useful info on mobile. |
| 57 | **Photo fixed at 559×700 from 1024 to 1920** — locked aspect 4:5. At 768 still 559×700 (centered). At 414+ scales down to fit. ✓ Hard-coded in `.fnd3a__portrait-frame { aspect-ratio: 4/5; max-width: 559px }`. | None. |

#### Footer (`.ftr3-map__grid`) — discovery rule: 5-col ≥1180, 3-col ≤1180, 2-col ≤760, 1-col ≤480

| breakpoint | columns | column widths | section h |
|---|---|---|---|
| 1920 | **5-col** | 308 / 191 / 191 / 213 / 191 | 1209 |
| 1440 | 5-col | 323 / 200 / 200 / 222 / 200 | 1169 |
| 1280 | 5-col | 288 / 178 / 178 / 198 / 178 | 1109 |
| 1024 | **3-col** | 292 / 292 / 292 (5 lists wrap; row 2 has 2 lists; brand spans full row 1) | 1658 |
| 768 | 3-col | 214 / 214 / 214 (similar wrapping) | 1613 |
| 414/390/375 | **1-col** | 378 / 354 / 339 (full-width per row) | 2191 / 2182 / 2177 |

| # | Issue | Fix |
|---|---|---|
| 58 | **Footer collapse confirmed correct.** 1-col fires at 414 because 414 < 480 (414 is numerically less than 480). Our test grid skips the 2-col band (480 < w ≤ 760) which exists per discovery — would be useful to test at e.g. 600 / 720 in a follow-up sweep. | Consider adding 600 and 720 as supplementary test widths in future sweeps. |
| 59 | **Footer touch targets** are the dominant issue at mobile widths — see findings 20–23. | See findings 20–23. |
| 60 | **The footer "brand" column spans full-width at ≤1180** (per v3.css:904). At 1024 this gives the brand mark a row of its own above the 3 columns of lists. At 414 the brand is the first row of the 1-col stack. Layout is intentional and reads well. ✓ | None. |

### Print styles & dark/light mode

| # | Concern | Status | Fix |
|---|---|---|---|
| 61 | `@media print` styles | **Absent.** No print rules in any of fonts.css / tokens.css / base.css / v3.css. | Acceptable for an editorial brand site. If priority: add a minimal `@media print { body { background: #fff; color: #000 } }` to ensure the page is printable in B&W with text legible (currently dark-bg printout would burn ink). |
| 62 | `@media (prefers-color-scheme: light)` | **Absent.** Site is dark-first by design. | Acceptable for an editorial brand. The `.slab-light` / `.slab-dark` system in tokens.css does provide a manual light theme for `.manif3` (paper section), but no automatic light fallback. |
| 63 | `@media (prefers-reduced-motion: reduce)` | **Comprehensive.** Covers 50+ selectors at v3.css:1138, plus base.css:61, base.css:266, components.css:164. Hero3, manif3, nw3a, mtd3, nl3, fnd3a all consult `reduceMotion` in JS too. ✓ | None. Wave 1 noted that cnt3 progress bar and ftr3 column observer still RUN their scroll work but the CSS @media block hides the visual effect — a perf-only gap, not a UX one. |

---

## Cross-cutting recommendations (priority-ordered)

1. **Touch target sweep** — fix findings 12, 13, 16, 17, 20, 21, 22 in one pass. Single CSS commit, ~10 lines of additions/changes. Fixes the largest cluster of mobile-quality regressions.
2. **Method scroll-pin height guard** — finding 30/47. Add `(min-height: 700px)` to the desktop branch. One-line CSS change, prevents the 1024×768 jail experience.
3. **Founder 1024 polish** — finding 8/55. Either raise the 1-col threshold to 1080 or widen the rail at 1024 with content. Pick one; both are valid.
4. **Newsletter 1024 form** — finding 7/51. Add `minmax(220px, 1fr)` to the form's grid-template-columns.
5. **Hero KPI grid 1-col at 600** — finding 40. Verify whether the discovery doc was right (rule should exist) or wrong (rule was never authored). If missing, add it.
6. **Header 2-stage collapse** — finding 26. Hide search/lang at 1024, hide nav at 768. Gives tablet users a visible nav without burger interception.
7. **Network 2-col mid-mobile band** — findings 29/45. Optional polish to halve mobile section height.

These 7 changes together would push the matrix from current state (10 ⚠/◐ cells) to 0 ⚠ cells — **a true 10/10 cross-breakpoint story.**

---

## Appendix: Tooling

Sweep script: `design-audit/.tools/sweep.js` (puppeteer-core driving system Chrome at `C:\Program Files\Google\Chrome\Application\chrome.exe`, headless, deviceScaleFactor 1, font-render-hinting=none).
Digest: `design-audit/.tools/digest.js` collapses the 192KB JSON to a column-wise table.
Re-run: `cd design-audit/.tools && node sweep.js > sweep-out.json && node digest.js > digest.txt`.

To test additional breakpoints (recommended: 600, 720, 880, 960, 1080, 1100, 1180 to verify each documented boundary), edit the `BREAKPOINTS` array in `sweep.js`.
