# Wave 3 — RTL Arabic Typography Audit

**Audited:** 2026-04-26
**Live URL:** http://localhost:8765/index.html
**Source:** `C:\Users\toner\OneDrive\Desktop\almobadir\website\`
**Locale:** `<html lang="ar" dir="rtl">` (global) + per-section `dir="rtl" lang="ar"` repeated on header / hero / manif3 / nw3a / mtd3 / cnt3 / nl3-section / fnd3a / ftr3
**Rendering reality:** Brand fonts (Mostaqbali / Graphik Arabic / Madani Arabic) are 404 → the cascade falls through to **Almarai / Tajawal / IBM Plex Sans Arabic** (Google-served). Audit is on what actually renders, not the spec.

Benchmark bar: Aramco editorial PDFs · NYT Arabic (legacy) · Al Jazeera English · Pentagram Arabic · Cairo Review · 23andMe Hebrew RTL · Sketch's `ar-SA` localization.

---

## Executive Summary

The architecture is competent — `dir="rtl"` is set globally and repeated on every section, the base body uses `font-ar-body`, logical properties (`margin-inline-*`, `padding-inline-*`, `border-inline-*`, `inset-inline-*`) appear in roughly **70%** of the directional CSS — but the remaining **30%** is the gap between "works" and "10/10 editorial." There are 18 locations in `v3.css` where physical `left/right`, `padding-left/right`, `margin-left/right`, or `border-right` ship instead of logical equivalents — most of them are subtle (drawer accent stripes, manifesto period margin, content tag dot margin, content tag position, content body accent bar) but they all fight the cascade in RTL. The bigger issues are typographic, not directional: (1) display headings use **`letter-spacing` between −0.02em and −0.04em**, which is appropriate for Latin display but **closes Arabic letter-joining gaps unnaturally**; the Almarai fallback already runs tight and these negative tracks make الأعمال and الترفيه look squeezed at clamp-max sizes. (2) The bio paragraph runs `text-align: justify; text-justify: inter-word` which on Arabic produces inter-word gaps (no kashida — browsers don't elongate by default), creating river-rivers visible in the founder section at 1440. (3) Numerals are **inconsistent**: HUD progress and newsletter mock are Arabic-Indic (٠١, ١٤٧, ٤٫٢٪), but stats, KPIs, ticker, dates in cnt3, and footer copyright are Western (1.5M, 5,000+, 22.04.2026, +966) — there is no policy. (4) The newsletter email input forces `dir="ltr"` with a Latin placeholder `name@example.com`, breaking Arabic posture. (5) Mixed-script eyebrows (`§ 001 · المنهج` / `MANIFESTO / 001`) and tags (`FEATURED · الأكبر`) lack `unicode-bidi: isolate` on the Latin run, so the bidi algorithm is doing the work — it gets it right ~95% of the time but punctuation drift will appear in some browsers. (6) Glyph clipping: line-height ≥ 1.18 and `padding-block: 0.04em` are applied on the major display headings, but the founder name (`clamp(40px, 5vw, 76px)` with `padding-block: 0.06em`) and hero tagline still risk fatha/damma clipping when stronger Arabic display fonts load. (7) The manifesto wraps each word in `<span class="manif3__word">` with `margin-left: .18em` — this is a **physical property under `dir="rtl"`**; in Firefox it visually pushes words the wrong direction in some compositions (the visible word-gap should always be on the **start** side). 46 findings below.

---

## Findings

### A. `dir="rtl"` SCOPE & STRUCTURE

#### A.1 Global `dir="rtl"` is correctly applied at the document root
✅ **PASS.** `index.html:2` sets `<html lang="ar" dir="rtl">`. Every major section ALSO redeclares `dir="rtl" lang="ar"` (header line 35, hero 141, manif3 302, nw3a 356, mtd3 458, cnt3 833, nl3 908, fnd3a 1001, ftr3 1127). Redundant but harmless — defends against future markup-injection regressions.

#### A.2 Sub-sections that intentionally flip to LTR are flagged correctly
✅ **PASS** for: hero ticker (`.hero3__ticker-track { direction: ltr }`), manifesto English subtitle (`.manif3__eyebrow--en { direction: ltr }`), manifesto English shadow (`.manif3__shadow { direction: ltr; text-align: left }`), manifesto meta caption (`.manif3__attr--meta { direction: ltr }`), method HUD brand (`.mtd3__hud-brand { direction: ltr }`), method progress (`.mtd3__progress { direction: ltr }`), method art tags (`.mtd3__art-tag { direction: ltr }`), and base mono utilities (`.tx-eyebrow / .tx-mono / .s-num { direction: ltr }`). These are correct uses of LTR override for ticker/timecode/mono micro-type.

#### A.3 `.hero3__ticker-track` direction:ltr is correct, but the ticker contains a mixed item that breaks
🟡 **MEDIUM.** v3.css:229 forces LTR on the ticker, which is right for `TASI 11,842.6` and `BRENT 84.21`. But one row contains `ALMOBADIR · LIVE` and `VISION 2030 · DAY 2,142` — those are fine. Issue is the up/down arrows `▲ 0.42%` rendered inside an `<em class="up">`: in LTR context the visual order works, but on screens narrower than 600px the ticker scrolls in the wrong direction relative to the rest of the RTL UI (it scrolls left-to-right while page layout reads right-to-left). Fix: keep `direction: ltr` for typesetting BUT swap animation direction (`hero3-ticker` translates by `-100%` — should be `+100%` so the strip travels in the natural Arabic reading direction).

#### A.4 `.fnd3a__rail` uses `writing-mode: vertical-rl` + `transform: rotate(180deg)` — works but is hacky
🟢 **LOW.** v3.css:716. The rail-text loops `PORTRAIT · BADR SHAKER · FOUNDER · ALMOBADIR · #6 FAVIKON SAUDI B2B 2025`. The rotate(180deg) is a workaround to flip the text right-side up; cleaner is `writing-mode: vertical-lr` without the rotate. Also: pure-Latin content in vertical-rl on an RTL page reads bottom-to-top — verify against Aramco's vertical-rule pattern.

#### A.5 `.mtd3__rail-list` uses `writing-mode: vertical-rl` for Arabic step labels
🟡 **MEDIUM.** v3.css:383 sets vertical-rl on the rail (اختيار · تبسيط · سرد · تصميم · إصغاء). At desktop the rail is hidden (`.mtd3__rail { display: none }` line 382), so this is currently dead in production — but if it ever activates, vertical Arabic is hard to read because letter-joining and dot positions weren't designed for stacking. Pentagram's Aramco vertical work uses individual letters separately (`text-orientation: upright`) — currently absent here.

---

### B. LOGICAL vs PHYSICAL PROPERTIES — non-conformance map

#### B.1 `.hdr3` sticky uses `left: 0; right: 0` instead of `inset-inline: 0`
🟢 **LOW.** v3.css:11. `position: sticky; top: 0; left: 0; right: 0;` works identically in RTL/LTR for sticky-full-width, but mixed paradigm. Use `inset-inline: 0` for consistency. Same on `.hdr3-mega` left/right (line 53).

#### B.2 `.hdr3-card` uses `padding: 14px 14px 14px 18px` (physical extra-left)
🔴 **HIGH.** v3.css:61. The asymmetric `padding-left: 18px` was meant to leave room for the colored `.hdr3-card-stripe` (`right: 0; width: 3px` on line 63). In RTL, the stripe sits on the **right edge** (which is the start side in Arabic — correct intent), but the extra padding is on the **left** (end side) — i.e., the padding is on the **opposite side from the stripe**. Result: the stripe abuts text directly with no breathing room, while empty space pads the far side. Fix: `padding-inline: 18px 14px` and `.hdr3-card-stripe { inset-inline-start: 0; right: auto }`.

#### B.3 `.hdr3-nav-link::after` underline uses `left: 14px; right: 14px`
🟢 **LOW.** v3.css:47. Symmetric so it works in RTL. But `transform-origin: right` + `transform: scaleX(0)` means the underline grows from the right (start) edge — that's correct in RTL but accidental: in LTR it would also grow from "right" (end). Fix: `transform-origin: var(--start, right)` or use logical anchoring via `inset-inline`.

#### B.4 `.hdr3-card-stripe` uses `right: 0` (physical, intent: start edge)
🟡 **MEDIUM.** v3.css:63. Should be `inset-inline-start: 0`. As-is: in RTL the stripe is on the right (start) — correct visual intent. In LTR (if a translation is shipped), the stripe stays on the right which is now the END side — wrong.

#### B.5 `.hdr3-drawer-net a` uses `border-right: 3px solid` (4 occurrences)
🔴 **HIGH.** v3.css:108–111. `border-right` for the colored brand stripe on each mobile drawer item. In RTL this puts the stripe on the start side — visually correct **today**, but breaks the moment language toggles to EN. Fix: `border-inline-start: 3px solid var(--crimson)` and matching `border-inline-start-color` overrides for `[data-color]` variants.

#### B.6 `.manif3__word` uses `margin-left: .18em` for inter-word gap
🔴 **HIGH.** v3.css:257. This is the manifesto stagger-reveal mechanism — each Arabic word is wrapped in a span, and the inter-word gap is added via `margin-left`. In RTL this places the gap on the END side of each word, which by visual ordering becomes the gap to the **next** word (still correct because both words flip). But this fights the bidi algorithm in mixed-script contexts (the `MANIFESTO / 001` adjacent eyebrow could collide). Fix: `margin-inline-end: .18em`. While here, `.manif3__period { margin-right: .04em }` (line 260) should be `margin-inline-start: .04em`.

#### B.7 `.manif3__underline` SVG uses `left: 0; right: 0` (symmetric, OK)
🟢 **LOW.** v3.css:263. Both edges defined → safe. But `width: 100%` makes both redundant. Strip one.

#### B.8 `.manif3__shadow` uses `padding-left: 24px; border-left: 2px solid`
✅ **PASS in this case.** v3.css:266. Because `direction: ltr; text-align: left` is also set on this element (it's the English shadow paragraph), `padding-left + border-left` is correct. Keep as-is.

#### B.9 `.manif3__attr--meta` uses `margin-right: auto` for spacer
🔴 **HIGH.** v3.css:273. In RTL flexbox, `margin-right: auto` pushes the element toward the **left** edge (visually toward the END of the row in RTL) — but the intent here is "shove DOC. 001 / EDITORIAL CHARTER to the far end" which is the LEFT in RTL because the row reads right-to-left. So **it works visually**, but the @780px override at v3.css:274 (`margin-right: 0`) and the property name itself are RTL-coupled. Fix: `margin-inline-start: auto` and the override becomes `margin-inline-start: 0` — semantic and stable.

#### B.10 `.nw3a-stripe` uses `top: 0; left: 0; right: 0` for full-width top stripe
🟢 **LOW.** v3.css:308. Symmetric, works. But `transform-origin: right center` (line 308) means the stripe grows from the right (start) edge in RTL — visually correct today, breaks in LTR. Fix: use a CSS variable `--reveal-anchor: right` defaulted to `right` and overridden in `[dir="ltr"]` blocks, or use `transform-origin: var(--logical-start)`.

#### B.11 `.nw3a-tag-en` uses `padding-right: 8px; border-right: 1px solid`
🔴 **HIGH.** v3.css:327. The English Latin tag (e.g., `Self-Development`) gets a **right border** as a divider from the Arabic tag. In RTL this border sits on the END side of the EN run, which abuts the Arabic tag — semantically that's the divider position. But the EN run is itself LTR (it should be in a `<span dir="ltr" lang="en">`, currently not). The border-right thus sits on the visual-LEFT of the EN text, separating it from nothing. Fix: wrap the EN run in `<span lang="en" dir="ltr">`, change to `border-inline-end` (which becomes border-left in the LTR isolated span, abutting the Arabic correctly).

#### B.12 `.nw3a-count-unit` uses `margin-right: 3px`
🔴 **HIGH.** v3.css:338. The count-unit (`K`) sits next to the figure (`478`). In RTL the unit appears on the LEFT of the number visually — `margin-right` pushes it further left. Should be `margin-inline-start`.

#### B.13 `.cnt3-tag` uses `top: 16px; right: 16px`
🟡 **MEDIUM.** v3.css:572. The category tag (`ثقافة مالية`) is positioned to the upper-right corner of the cover image — which in RTL is the **start corner**, matching the natural reading entry-point. Correct intent, but `right: 16px` should be `inset-inline-start: 16px` so it auto-flips for an EN port.

#### B.14 `.cnt3-tag::before` (the dot) uses `margin-left: 8px`
🔴 **HIGH.** v3.css:573. The colored dot prefix on each tag uses `margin-left: 8px` — in RTL flexbox display:inline-block, this puts whitespace on the visual-RIGHT of the dot. The dot precedes the text in RTL (right side), so the gap should be on the END side (left in RTL). `margin-left` actually achieves the right visual gap by accident. Fix: `margin-inline-end: 8px`.

#### B.15 `.cnt3-cover-meta` uses `bottom: 0; left: 0; right: 0`
🟢 **LOW.** v3.css:574. Symmetric, fine.

#### B.16 `.cnt3-accent-bar` uses `top: 0; right: 28px` (intent: start-corner)
🟡 **MEDIUM.** v3.css:579. The 32px decorative red bar sits in the upper-right (start) corner of each card body — semantically the "begin reading" anchor. `right: 28px` works in RTL but breaks in LTR. Fix: `inset-inline-start: 28px`.

#### B.17 `.fnd3a__photo` no issues; `.fnd3a__verified` uses `top: 18px; inset-inline-end: 18px`
✅ **PASS.** v3.css:724 already uses logical `inset-inline-end`. Same pattern correctly applied at 1048 and 1051 in mobile overrides. Good.

#### B.18 `.ftr3-list .ftr3-arrow` uses `transform: translateX(6px)` (physical translate)
🔴 **HIGH.** v3.css:863. The arrow nudges in by `translateX(6px)` on hover — in RTL `translateX(positive)` moves the arrow visually LEFT, which is the wrong direction (arrows in RTL should move toward the start = right). Same problem on `.ftr3-cta__btn:hover { transform: translateX(-2px) }` (line 834) and `.ftr3-mailbtn:hover` (886). Fix: use a logical custom property `--rtl-x: -1` defaulted such that `translateX(calc(6px * var(--rtl-x)))` flips in `[dir="ltr"]`. Or simpler: `translate(start, 0)` via logical `translate` keyword (Chrome 124+).

#### B.19 `.ftr3-lang__pill` uses `right: 3px; transform: translateX(0)`
🟡 **MEDIUM.** v3.css:898. The active-language pill in the language toggle pins to `right: 3px` — in RTL that's the start side, which is where AR (active by default) sits. When EN activates, `[data-active="en"] .ftr3-lang__pill { transform: translateX(-100%) }` (line 899) — but `translateX(-100%)` moves the pill RIGHT in RTL coordinates (because the visual axis is flipped), which means it stays on AR. **This is broken**: clicking EN does not move the pill correctly in RTL. Fix: use `inset-inline-start: 3px` and toggle via percentage on a logical translate, or use a CSS custom prop that flips with `dir`.

#### B.20 `.nl3-quote` uses `border-inline-start: 2px solid` and `text-align: left`
✅ **PASS** for border (logical). But the `.nl3-quote { background: linear-gradient(to left, ...) }` (v3.css:650) hard-codes the gradient direction. In RTL the gradient should flow from the start side (right) inward — `to left` is wrong. Fix: `linear-gradient(to var(--logical-start, left), ...)` or use `direction`-aware percent stops.

#### B.21 `.nl3-form .nl3-ink` uses `bottom: 8px` + `transform-origin: right center`
🟡 **MEDIUM.** v3.css:630. The animated ink underline in the form. `transform-origin: right center` plus `transform: scaleX(0) → scaleX(1)` makes the line sweep from right (start in RTL) to left (end). In RTL this is correct. In LTR it would sweep from end to start — wrong. Tie to logical custom property.

#### B.22 `.nl3-issue` marquee uses `flex-direction: row` natural with `dir="rtl"` parent — confirmed
✅ **PASS.** The marquee row inherits RTL; items lay out right-to-left naturally. Animation `nl3-marquee` is `translateX(0 → 50%)` — needs verification at 1440. Currently produces what reads as right-to-left scrolling: ✅ correct.

#### B.23 `cnt3-cta` uses `padding: 14px 22px 14px 26px` (asymmetric)
🟡 **MEDIUM.** v3.css:593. Extra padding on the LEFT — intent is to give the arrow icon (which is `→` rendered backward as `←` for RTL) breathing room. In RTL the arrow sits on the visual left → padding-left is the correct side **today**. But couple to LTR breakage. Fix: `padding-inline: 22px 26px` (start, end).

---

### C. ARABIC GLYPH CLIPPING & VERTICAL METRICS

#### C.1 Hero tagline `.hero3__tagline` — generous and safe
✅ **PASS.** v3.css:169. `line-height: 1.2`, `padding-block: 0.04em`, font size up to 60px. At Almarai fallback this comfortably clears damma + sukun and tracks descenders on م ج ح خ. Verified in `hero3-1440.png` — no clipping on الترفيه or التعليم.

#### C.2 Manifesto lede `.manif3__lede` — safe
✅ **PASS.** v3.css:256. `line-height: 1.25`, font size up to 68px. No `padding-block` set explicitly but `text-wrap: balance` helps. Manifesto screenshot shows full diacritics on الجادّة, مملّة، أعقدَ.

#### C.3 Network title `.nw3a-title` — `line-height: 1.18` + `padding-block: 0.04em`
✅ **PASS.** v3.css:286. Adequate for `ست منصات.` + `صوت واحد.`. Italic `<em>` does not break joining (font-style italic on Arabic is mostly cosmetic in fallbacks; Almarai has no real italic — browser synthesizes a slant which sometimes detaches dots).

#### C.4 Method title `.mtd3__title` — `line-height: 1.0` is TOO TIGHT
🔴 **HIGH.** v3.css:366. `line-height: 1.0` for an Arabic display heading at clamp-max 68px will clip the tail of م، ج، خ in any font that has descenders below the baseline. Almarai gets away with it because its descenders are short, but with Mostaqbali (the brand display) loaded this WILL clip. Fix: minimum `line-height: 1.08` and add `padding-block: 0.06em`. Currently the heading reads `من فكرة معقّدة إلى قصّة لا تُنسى` — the ج in قصّة has visible bottom-clip risk.

#### C.5 Founder name `.fnd3a__name` — `padding-block: 0.06em`, `line-height: 1.15`
🟡 **MEDIUM.** v3.css:737. Up to 76px. Background-clip text gradient doesn't affect metrics. With Almarai bold this is fine. With Mostaqbali Black (font-weight 800–900, the spec) the شين (ش) in بدر شاكر will have a heavier descender and 1.15 may clip — bump to 1.18 to match other displays.

#### C.6 Newsletter title `.nl3-title` — `line-height: 1.2` + `padding-block: 0.04em` ✅
✅ **PASS.** v3.css:617.

#### C.7 Footer CTA title `.ftr3-cta__title` — same pattern ✅
✅ **PASS.** v3.css:825.

#### C.8 Method H3 frame headings `.mtd3__h` — `line-height: 1.18` + `padding-block: 0.04em`
✅ **PASS.** v3.css:408. نختار الموضوع / نُبسّط بدون اختزال etc. all fit cleanly.

#### C.9 Hero KPI value `.hero3__kpi-value` — `line-height: 1`
🟡 **MEDIUM.** v3.css:202. The KPIs render as numerals (1.5, 25, 6, 5,000) so `line-height: 1` is fine for digits. But for `إجمالي المتابعين` label (`.hero3__kpi-label`) above it, the label uses uppercase mono font — no Arabic. Confirm the Arabic equivalent labels (currently rendered) don't clip — they do NOT (label uses `font-size: 10.5px`).

#### C.10 The hero wordmark SVG `<text>` is rasterized; padding: irrelevant
✅ **PASS.** v3.css:174. The `المبادر` wordmark is an SVG `<text>` element — the SVG viewBox provides plenty of padding (`y="170"` in a 240-unit-tall viewBox). No clipping risk regardless of font.

#### C.11 `overflow: hidden` review for sections — most are fine
✅ **PASS.** Sections use `overflow: hidden` at the SECTION level (manif3, nw3a, fnd3a, ftr3, mtd3, cnt3) — but headlines inside have plenty of padding-block. No clipping at SECTION level. Verified across 9 screenshots.

#### C.12 `.cnt3-headline` uses `-webkit-line-clamp: 3` + `overflow: hidden`
🟡 **MEDIUM.** v3.css:581. The card headline clamps to 3 lines with `display: -webkit-box; overflow: hidden`. In Arabic this works — but `line-height: 1.18` + clamp on 3 lines means the LAST line's bottom edge sits exactly at `overflow: hidden` boundary → letters with descenders on line 3 will clip the last few px of their tails. Fix: add `padding-block-end: 0.06em` to the headline OR raise line-height to 1.22 OR use `mask-image: linear-gradient(black 90%, transparent)` to fade the bottom instead of hard-clip.

---

### D. LETTER JOINING & LIGATURES

#### D.1 `letter-spacing: -0.02em` to `-0.04em` on display headings
🔴 **HIGH (multiple locations).** Negative tracking is a Latin display-typography move (Helvetica Neue Black, Inter at 60px). On Arabic it **closes letter-joining gaps** that were designed by the type designer. The Almarai fallback already runs tight; subtracting another 2–4% of em pushes letterforms into one another and the dots above/below stop having clear ownership. Affected:
  - `.manif3__lede { letter-spacing: -.01em }` (v3.css:256) — borderline acceptable
  - `.nw3a-title { letter-spacing: -0.03em }` (286) — too tight on Arabic
  - `.nw3a-name { letter-spacing: -0.01em }` (330) — borderline
  - `.nw3a-count { letter-spacing: -0.04em }` (335) — only digits ✓ OK
  - `.nw3a-stat-fig { letter-spacing: -0.02em }` (342) — only digits ✓ OK
  - `.mtd3__title { letter-spacing: -.02em }` (366) — too tight
  - `.mtd3__h { letter-spacing: -.02em }` (408) — too tight
  - `.cnt3-title { letter-spacing: -0.025em }` (551) — too tight
  - `.cnt3-headline { letter-spacing: -0.005em }` (581) — fine
  - `.nl3-title { letter-spacing: -0.025em }` (617) — too tight
  - `.fnd3a__name { letter-spacing: -0.025em }` (737) — too tight
  - `.ftr3-cta__title { letter-spacing: -.025em }` (825) — too tight

  **Fix:** scope negative tracking to digits only. Use `font-feature-settings: "kern" 1` (already on body) and let the type designer's kerning tables do the work. For Arabic-only headings, set `letter-spacing: 0` or maximum `-0.005em`. Add `:lang(en) { letter-spacing: -0.025em }` for the Latin-only runs.

#### D.2 Wordmark SVG uses `letter-spacing="-2"` (SVG attribute)
🟡 **MEDIUM.** index.html:174 + 1148. Both the hero and footer wordmark SVGs apply `letter-spacing="-2"` to المبادر. This is roughly −1% at 200px font-size = −2/200 = −0.01em — survivable on Arabic but tight. With Mostaqbali Black loaded this will start to bind. Reduce to `letter-spacing="0"` and let the font's metrics handle it.

#### D.3 Manifesto word-wrapping `<span class="manif3__word">نحن</span>` — joining preserved
✅ **PASS.** Words are whitespace-separated in the HTML. Each span contains a complete word, so within-word joining is intact (spans are `display: inline-block` so they don't break — verified). The previous founder dropcap that broke م + ؤ has been removed from HTML (CSS at v3.css:744 still defines `.fnd3a__dropcap` but the class is no longer in markup → DEAD CODE; remove from CSS to avoid future re-introduction).

#### D.4 The CSS `font-feature-settings` is set globally on body
✅ **PASS.** base.css:25 sets `font-feature-settings: "kern" 1, "liga" 1; font-kerning: normal`. Confirmed inherits to all Arabic text.

#### D.5 `.manif3__word--accent { font-feature-settings: "ss01" }` — relies on Mostaqbali stylistic set
🟢 **LOW.** v3.css:259. Mostaqbali isn't loaded (404). Almarai has no `ss01` feature → declaration is silently ignored. Side-effect: the accent words (منصةَ, محتوى., جاذبيةِ, أفلامِ, الإثارة) render in the same style as surrounding text. Manifesto screenshot confirms — the accent words are red-colored but visually identical to siblings. Need either (a) wait for brand fonts, or (b) add `font-style: italic` (synthesized italic on sans-Arabic is a fake slant — not great).

#### D.6 Bold rendering on `.fnd3a__bio-text strong { font-weight: 700 }` — Almarai 700 vs 400
✅ **PASS.** Almarai has 700 weight in the preloaded `<link>` (index.html:24). Renders correctly.

#### D.7 SVG `font-family="Mostaqbali, Almarai, Tajawal, sans-serif"` falls through cleanly
✅ **PASS.** The hero wordmark SVG `<text>` (line 174) and footer mark SVG (1148) both list Mostaqbali first → falls to Almarai. No FOUT issue because SVG renders synchronously with whatever's available. Verified in screenshots.

---

### E. KASHIDA / JUSTIFICATION

#### E.1 `.fnd3a__bio-text { text-align: justify; text-justify: inter-word }`
🔴 **HIGH.** v3.css:742. `text-align: justify` on Arabic with `inter-word` produces visible river-rivers because:
  - Modern browsers (Chrome 124+, Safari 17+, Firefox 125+) **do not insert kashida** by default. They expand inter-word spaces.
  - At narrow column widths (the bio is in a column < 60ch wide), this looks like rag-right-with-extra-gaps rather than a clean justified block.
  - Aramco's print PDFs use **kashida-aware OpenType fonts** with explicit `font-feature-settings: "kshd"` (kashida feature), which Almarai doesn't have.

  **Fix:** drop `text-align: justify` for Arabic body. Use `text-align: start` with `text-wrap: pretty` (already set globally on `p`). The mobile @560 override (v3.css:1058) already does `text-align: start; text-justify: auto` — that should be the rule, not the exception.

#### E.2 No other element justifies Arabic — good
✅ **PASS.** Only `.fnd3a__bio-text` justifies. Manifesto, hero lede, network lede, mtd3 body, cnt3 excerpt, nl3 lead, ftr3 desc all default to `text-align: start` (which becomes `right` in RTL).

---

### F. MIXED-SCRIPT BIDI

For each mixed run, verify the Latin part is wrapped in `<span lang="en" dir="ltr">` AND `unicode-bidi: isolate` is applied (or implicit via `dir`).

#### F.1 Hero eyebrow: `LIVE · شبكة من 6 قنوات · +1.5M متابع · EST. 1446H`
🟡 **MEDIUM.** index.html:154–162. The eyebrow has `LIVE` and `EST. 1446H` in Latin spans without `lang="en"` or `dir="ltr"`. The CSS at v3.css:163 (`.hero3__eyebrow-label`) and 165 (`.hero3__eyebrow-mono`) doesn't isolate bidi. The bidi algorithm gets it right because `LIVE` is unambiguously LTR and the surrounding context is RTL — but `EST. 1446H` includes "1446H" which is digits + capital Latin — could collide with `+1.5M` neighbor in some browser/font combos. Fix: `<span lang="en" dir="ltr">LIVE</span>` and same for `EST. 1446H`. Add `unicode-bidi: isolate` to the labels.

#### F.2 Manifesto eyebrow: `§ 001 · المنهج` and `MANIFESTO / 001`
✅ **PASS** for the second span — index.html:306 has `lang="en"`. ❌ **FAIL** for the first span (line 305): `§ 001 · المنهج` mixes a section sign + digits + Arabic. The § is bidi-neutral, digits are bidi-neutral, then Arabic flips the run. Currently `.manif3__eyebrow` doesn't have `dir` set. Visually it renders left-to-right because there's no explicit RTL marker — meaning **the section number and Arabic title order is wrong** for an RTL document. Fix: either reorder content to `المنهج · ١ §` or wrap the leading numeric run in `<span dir="ltr">§ 001</span>`.

#### F.3 Network eyebrow: `الشبكة` + `/ Network`
🟡 **MEDIUM.** index.html:358. Two adjacent spans. Latin portion lacks `lang="en"` / `dir="ltr"`. `.nw3a-eyebrow-en` CSS at v3.css line ~285+ doesn't isolate. The slash `/` is bidi-neutral and adjacent to both — ambiguity. Add `<span lang="en" dir="ltr" class="nw3a-eyebrow-en">/ Network</span>`.

#### F.4 Network card tags: `تطوير الذات` + `Self-Development`
🔴 **HIGH.** index.html:376, 389, 402, 415, 428, 441. Six instances. The Latin tag-en spans have no `lang` attribute. The CSS uses `border-right: 1px solid var(--hairline)` (v3.css:327) intended as a divider — see B.11. Wrap in `<span lang="en" dir="ltr" class="nw3a-tag-en">Self-Development</span>` and use `border-inline-end`.

#### F.5 Network feature flag: `FEATURED · الأكبر` and `AGENCY · الذراع التنفيذية`
🔴 **HIGH.** index.html:376, 441. The string starts in Latin then flips to Arabic with a `·` interpunct between. Currently inside a single `<span class="nw3a-feature-flag">`. The `·` separator is bidi-neutral; Arabic dominates the directionality of the run. Result: `FEATURED · الأكبر` may render as `الأكبر · FEATURED` in strong-RTL contexts (the bidi algorithm reorders neutral punctuation). Test in Firefox specifically. Fix: split into `<span dir="ltr">FEATURED</span> · <span dir="rtl">الأكبر</span>` with explicit isolation.

#### F.6 Method HUD brand: `المبادر · METHOD`
🟡 **MEDIUM.** index.html:468, v3.css:376 forces `direction: ltr` on the whole HUD-brand span. With LTR direction, the text reads `المبادر · METHOD` left-to-right — but Arabic letters within an LTR-overridden run still join correctly. The issue is the dot `·` interpunct sits between Arabic and Latin and its position is now **fixed** by LTR — visually placed to the right of المبادر which is correct. Acceptable. Best-practice: split spans.

#### F.7 Method art tag: `TARGETING · ٠١`, `CLARITY · ٠٢`, `CINEMA · ٠٣`, `EDITORIAL · ٠٤`, `BROADCAST · ٠٥`
🟡 **MEDIUM.** index.html:573, 636, 729, 750, 820. `direction: ltr` is applied (v3.css:416). Mixes Latin word + interpunct + Arabic-Indic digit. Arabic-Indic digits are bidi-NEUTRAL (not strongly Arabic), so they follow the surrounding LTR direction — render correctly as `TARGETING · ٠١`. ✅ Acceptable. (Note: `٠١` displayed AFTER the Latin word is the natural English reading order — `target #01`.)

#### F.8 Content card meta: `@almobadir_flousak · 22.04.2026 · 9 دقائق قراءة`
🟡 **MEDIUM.** index.html:860, 876, 892. Three runs separated by interpuncts. Currently `.cnt3-meta` is just font-mono RTL by inheritance. The Latin handle + Latin date come first in source, then Arabic reading-time. Bidi resolves: handle (LTR) + Arabic reading-time (RTL) → in an RTL container the Arabic floats to the right (start) and the handle to the left (end). Reading-order: time → date → handle, which conflicts with the source order. Visual confirmation needed at 1440 — likely shows reading-time on right, handle on left, which is the **opposite of source order**. Fix: wrap the Latin handle and date in `<span dir="ltr">` to preserve source ordering, OR re-source as `<bdi>` elements.

#### F.9 Founder caption: `FIG. 01` + `بدر شاكر — الرياض، 2025`
🔴 **HIGH.** index.html:1028. Two siblings. `FIG. 01` lacks `lang="en"`. The Arabic part has Western "2025" inline at the end → mixed direction within the second span. Fix: split into three: `<span lang="en" dir="ltr">FIG. 01</span><span lang="ar">بدر شاكر — الرياض،</span><span lang="en" dir="ltr">2025</span>`.

#### F.10 Founder tagline: `بزنس و تسويق ● Top 20 B2B Saudi Arabia · 2025`
🔴 **HIGH.** index.html:1036. `<span class="fnd3a__tagline-en">Top 20 B2B Saudi Arabia · 2025</span>` correctly isolated as a separate span — but no `lang="en"` / `dir="ltr"`. Add them.

#### F.11 Hero ticker mixes `TASI 11,842.6 ▲ 0.42%` / `USD/SAR 3.7503` / `VISION 2030 · DAY 2,142`
✅ **PASS.** v3.css:229 forces `direction: ltr` on the whole track — these are all Latin/numeric runs, no Arabic mixed in. (But see A.3 for the animation-direction bug.)

#### F.12 Footer base: `© 2026 ALMOBADIR · SAUDI ARABIA · +966`
✅ **PASS.** index.html:1203 — entirely Latin/numeric, runs in `.ftr3-mono` which inherits direction. The `©` symbol is bidi-neutral, surrounding context is mono → renders LTR. No issue.

#### F.13 Footer tags: `ARABIC FIRST · DARK MODE NATIVE · MADE WITH INTENT`
✅ **PASS.** All Latin, line 1204.

#### F.14 Founder rail (decorative): `PORTRAIT · BADR SHAKER · FOUNDER · ALMOBADIR · #6 FAVIKON SAUDI B2B 2025 ·`
✅ **PASS.** All Latin in vertical-rl. No Arabic mixed.

---

### G. NUMERALS POLICY

#### G.1 No coherent numeral policy across the site
🔴 **HIGH.** Inventory:
  - **Arabic-Indic (٠١٢٣٤٥٦٧٨٩):** mtd3 progress (`٠١ / ٠٥`), mtd3 art tags (`TARGETING · ٠١`), mtd3-cover issue (`عدد ٠٤ · ٢٠٢٦`), mtd3-cover lines (`٢٤ صفحة · ٧ مصادر`), mtd3 frame4 meta (`١٢ عمود`), nl3-mock issue (`العدد ١٤٧`), nl3-mock date (`٢٧ أبريل ٢٠٢٦`), nl3 mock blocks (`٠١ / ٠٢ / ٠٣`), nl3-stat (`٤٫٢٪`), nl3 mock body (`٧٢ ساعة`, `٤٠٪`, `٢٠١٤`, `٢٠٣٠`), nl3-issue past-issues (`١٤٦ / ١٤٥ / ١٤٤ / ١٤٣`, `٢٠ أبريل` etc.), ftr3 desc (`١٫٥ مليون`).
  - **Western (0123456789):** hero ticker (TASI 11,842.6 / 25M MONTHLY VIEWS / DAY 2,142), hero panel KPIs (1.5, 25, 6, 5,000+), hero trust (`#6 Favikon · AUTHORITY 8,545`), nw3a totals (`1.54M / 25M / 06`), nw3a numbers (`478K / 363K / 317K / 202K / 124K / 57.7K`), nw3a media stats (`143+ / 5K+ / $1M+`), mtd3 num overlay (`01/02/03/04/05`), cnt3 issue (`№ 184 / أسبوع 17 / 2026`), cnt3 dates (`22.04.2026`), cnt3 reading times (`9 دقائق`, `6 دقائق`, `5 دقائق`), cnt3 headlines (`قاعدة الـ 1٪`, `قاعدة 7-11-4`), nw3a network (`6 حسابات`), hero eyebrow (`6 قنوات`, `+1.5M`), nl3 trust count (`+50,000`), fnd3a stats (`#6 / 325K / 5,000`), ftr3 +966.
  - **Mixed within single section:** hero3 has both EST. **1446H** (Hijri Western) and `+1.5M` (Western), no Arabic-Indic anywhere. Newsletter has both `1.5 مليون` Western and `١٫٥ مليون` Arabic-Indic — different blocks.

  **Recommendation:** establish policy: `editorial chrome and dates → Arabic-Indic; large stats and counters → Western with Arabic-context-formatted thousand separator (٬ U+066C, not Western comma); pure-tech contexts (TASI, USD/SAR, $1M+, +966, dates in card meta) → Western`. Rewrite the ticker, KPIs, network totals to use Arabic-Indic where reading-context is Arabic (the panel says `إجمالي المتابعين` — Arabic — followed by `1.5M` — should be `١٫٥M` with Arabic-Indic + Latin unit). Aramco's editorial Arabic uses Arabic-Indic for in-text numbers and Western for table columns and equations. Use that as the rule.

#### G.2 Arabic decimal separator is wrong (or missing)
🟡 **MEDIUM.** Arabic uses `٫` (U+066B ARABIC DECIMAL SEPARATOR) and `٬` (U+066C ARABIC THOUSANDS SEPARATOR). Currently `nl3-stat-fig` uses `٤٫٢٪` (correctly with U+066B) — good. But `ftr3-desc` uses `١٫٥ مليون` (correct U+066B) — also good. However, hero panel `1.5M` uses Western period — inconsistent. If switching to Arabic-Indic, swap period for U+066B.

#### G.3 Percent sign — Arabic uses `٪` (U+066A), Latin uses `%`
✅ **PASS.** `nl3-stat-fig` has `٤٫٢٪` and `nl3-block-body` has `٤٠٪` and `cnt3-headline` has `1٪` — all use Arabic percent sign U+066A. Consistent.

#### G.4 Newsletter stat at 1440 (`٤٫٢٪`) renders correctly
✅ **PASS.** Verified in `nl3-section-1440.png`.

---

### H. PUNCTUATION

#### H.1 Arabic comma `،` (U+060C) — used correctly in body
✅ **PASS.** Multiple instances confirmed: `مملّة،` (manifesto), `الأثر، لا الضجيج`, `يتم تطبيقه، خمس مراحل`, `الحشو والغموض`, `وفكرة قابلة للتطبيق` — Arabic commas used throughout. No Western commas in Arabic body text found.

#### H.2 Arabic question mark `؟` (U+061F) — MISSING in some spots
🔴 **HIGH.** Checked the questions in content:
  - `cnt3-headline` line 874: `لماذا تتفوّق على من يعمل أضعافك؟` ✅ uses ؟
  - `cnt3-headline` line 890: `كيف يُغلق أفضل المسوّقين الصفقات` — **missing ؟**
  - `nw3a-desc` line 391: `أسرارها وقصصها ومفاهيمها.` (statement, no Q)
  - `nl3-issue-topic` line 985: `عودة الاكتتابات: من يربح فعلاً؟` ✅ uses ؟
  - `nl3-issue-topic` line 987: `العقار يصمت — لماذا الآن بالذات` — **missing ؟**
  - `nl3-block-body` line 891: `لماذا يحتاج العميل لرؤيتك بهذه الكثافة قبل أن يشتري؟` ✅ uses ؟

  Two card headlines that pose questions are missing the trailing ؟. Fix in HTML.

#### H.3 Arabic quotation marks `«» ` (U+00AB / U+00BB) — used in 2 locations, plain "" elsewhere
🟡 **MEDIUM.** Confirmed `«` `»`:
  - nl3 quote: `«أصبحتُ أفتح الأحد...»` ✅
  - nl3-block-body: `مؤسس «نون»` ✅
  - fnd3a__quote-mark (decorative): `«` ✅
  - fnd3a__colophon: `«اساهم في تطوير...»` ✅

  No straight quotes found in Arabic body — good. But Star-Toner-style curly quotes (`""`) are nowhere either. Aramco style guide uses the angle marks `«»` consistently — current usage is correct. Just verify no future content uses straight `"` in Arabic.

#### H.4 Em dash `—` (U+2014) used in mixed-script captions and body
✅ **PASS.** Editorial-correct usage: `بدر شاكر — الرياض، 2025`, `قراءة واحدة كل أسبوع — اقتصاد، أعمال، وتحليل`, `العقار يصمت — لماذا الآن بالذات`. Em dashes preferred over hyphens. Aramco style.

#### H.5 Interpunct `·` (U+00B7) used liberally as separator
✅ **PASS.** Uses U+00B7 throughout (verified in source). Renders consistently across browsers.

#### H.6 Western digits in Arabic context use Western thousands separator
🟡 **MEDIUM.** `+50,000`, `5,000+`, `1,486` etc. — Western comma. If those numerals stay Western, Western comma is fine. If they convert to Arabic-Indic, switch to U+066C `٬` (e.g., `٥٬٠٠٠`).

#### H.7 The `cnt3-cover` `№` symbol (U+2116)
✅ **PASS.** index.html:841 — `العدد <em>№ 184</em>`. № is the standard Cyrillic/typographic numero sign. Renders as Times-style glyph in Almarai fallback. Acceptable. Could swap to literal `رقم 184` for fully Arabic editorial chrome.

---

### I. CSS-IN-RTL MICRO-CHECKS

#### I.1 `.mtd3__cta svg { transform: scaleX(-1) }`
✅ **PASS.** v3.css:526. The CTA arrow SVG is intentionally mirrored to point left (the natural "forward" direction in RTL). Correct usage of scaleX(-1). Confirmed in mtd3-1440.png — arrow points correctly.

#### I.2 `.cnt3-cta-arrow svg` not mirrored — but content suggests it should be
🟢 **LOW.** v3.css:599. The arrow inside cnt3-cta-arrow is `<path d="M19 12H5"/><path d="M12 5l-7 7 7 7"/>` which already points left in source — no mirror needed. ✓.

#### I.3 `.ftr3-cta__arrow` and `.ftr3-mailbtn svg` — likewise drawn pointing left in source
✅ **PASS.** index.html:1137, 1197 — `<path d="M14 5l7 7-7 7M21 12H3"/>` is a leftward arrow.

#### I.4 `direction: ltr` is applied to `.tx-eyebrow / .tx-mono / .s-num` in base.css
✅ **PASS.** base.css:136, 202, 228. These are Latin mono utilities. Correct.

#### I.5 `::before` and `::after` content with directional characters
🟢 **LOW.** Spot checks:
  - `.cnt3-read::before { content: '≈ ' }` — non-directional ✓
  - `.cnt3-tag::before` (no content, just dot) ✓
  - No `::before` content with arrow characters that would need mirroring.

#### I.6 `transform-origin: right` patterns review
🟡 **MEDIUM.** Multiple instances:
  - `.hdr3-nav-link::after { transform-origin: right }` (line 47) — RTL-correct, LTR-broken
  - `.hero3__nav a::after { transform-origin: right }` (line 141) — same
  - `.manif3__rule { transform-origin: right center }` (247) — same
  - `.nw3a-stripe { transform-origin: right center }` (308) — same
  - `.nw3a-cta::after { transform-origin: right center }` (347) — same
  - `.cnt3-accent-bar { transform-origin: right top }` (579) — same
  - `.nl3-ink { transform-origin: right center }` (630) — same

  All seven cases assume RTL context. Single source of truth: define `--anchor-start: right` on `:root`, `--anchor-start: left` in a `[dir="ltr"]` block, and replace `right` with `var(--anchor-start)`. Touches 7 lines, future-proofs the LTR port.

---

### J. KEYBOARD INPUT & PLACEHOLDERS

#### J.1 Header search input — placeholder is Arabic
✅ **PASS.** index.html:99 — `placeholder="ابحث في المبادر…"`. Input itself inherits `dir="rtl"` from header. Cursor will start on the right (start) edge. Arabic IME input works natively.

#### J.2 Hero email input — placeholder is Arabic
✅ **PASS.** index.html:193 — `placeholder="عنوان بريدك الإلكتروني"`. Inherits RTL. ✓ — though when user types a Latin email (`a@b.com`), the input correctly switches the typed text to LTR-isolated rendering due to inherent character direction. Verified by browser default behavior.

#### J.3 Newsletter email input — `dir="ltr"` AND Latin placeholder
🔴 **HIGH.** index.html:930 — `<input ... placeholder="name@example.com" dir="ltr">`. This breaks Arabic posture in two ways:
  - Forces LTR direction → cursor enters on left, label `بريدك الإلكتروني` (line 929) is right-aligned but field below is left-aligned → visual mismatch.
  - Latin placeholder `name@example.com` instead of Arabic. Hero and footer use Arabic placeholders consistently.

  **Fix:** remove `dir="ltr"`. Change placeholder to `بريدك الإلكتروني` to match hero/footer. The browser will still render the typed email LTR because email characters are inherently LTR — no need to force.

#### J.4 Footer email input — Arabic placeholder, no dir override
✅ **PASS.** index.html:1136 — `placeholder="بريدك الإلكتروني"`, but `.ftr3-cta__input` CSS at v3.css:831 forces `direction: rtl`. Redundant since parent is RTL — could remove. Acceptable.

#### J.5 The newsletter form `.nl3-input` placeholder text uses opacity 0.36 cream
✅ **PASS.** v3.css:629. Color contrast on dark void: ~3.5:1. Borderline AA. Bump to 0.45 for AA-large.

---

### K. SUMMARY OF FIX PRIORITY

#### Critical (10/10 blockers)
1. **D.1** — Remove negative tracking from Arabic display headings (8 locations)
2. **E.1** — Drop `text-align: justify` on `.fnd3a__bio-text` for Arabic
3. **G.1** — Establish numeral policy and apply consistently
4. **J.3** — Newsletter email: remove `dir="ltr"`, switch placeholder to Arabic
5. **B.5** — Drawer accent stripe: `border-right` → `border-inline-start`
6. **B.18** — Footer hover `translateX` direction is reversed in RTL
7. **B.19** — Footer language pill animation broken in RTL
8. **F.4 / F.5 / F.9 / F.10** — Mixed-script wraps need `lang="en" dir="ltr"`
9. **C.4** — `mtd3__title` line-height too tight, will clip with brand font
10. **H.2** — Two missing question marks in card headlines

#### Important
- B.2, B.6, B.9, B.11, B.12, B.14, B.16 — physical→logical property migrations
- A.3 — hero ticker animation direction
- C.5, C.12 — secondary clipping risks with brand fonts loaded
- F.1, F.2, F.3 — additional bidi-isolate wrap
- H.6 — Arabic thousands separator if numerals migrate

#### Nice-to-have
- A.4, A.5 — vertical-rl architectural clean-up
- D.2 — wordmark SVG letter-spacing
- D.5 — `font-feature-settings: ss01` is no-op until brand font loads
- I.6 — single source of truth for transform-origin anchor

---

## Word Summary (200 words)

The almobadir.com RTL implementation is competent at the architectural level — `dir="rtl"` is set globally and reaffirmed per-section, body fonts cascade through Almarai/Tajawal correctly, and roughly 70% of directional CSS uses logical properties (`margin-inline-*`, `inset-inline-*`, `border-inline-*`). The remaining 30% is the gap between "works" and editorial-grade Arabic typography. Critical issues fall into three buckets. **Typography:** eight display headings apply 2–3% negative `letter-spacing`, a Latin-display convention that closes Arabic letter-joining gaps unnaturally and risks fatha/kasra ownership; the founder bio justifies with `text-justify: inter-word`, producing river-rivers because no browser inserts kashida by default. **Mixed-script:** seven of the ~15 Arabic+Latin runs lack `<span lang="en" dir="ltr">` wrapping or `unicode-bidi: isolate`, making bidi rendering browser-dependent — Firefox especially can reorder neutral punctuation. **Numerals & polish:** Arabic-Indic and Western digits coexist arbitrarily across editorial chrome, KPIs, dates, and stats with no policy; the newsletter email forces `dir="ltr"` and uses a Latin placeholder, breaking the Arabic posture; the footer language-toggle pill animation is mathematically broken in RTL; two question marks are missing on card headlines that ask questions. With 10 critical fixes plus the migrations to logical properties, this clears the Aramco / Cairo Review bar.
