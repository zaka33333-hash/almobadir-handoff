# Wave 2 — Footer Audit (`.ftr3`)

**Audited:** 2026-04-26
**Source:** `website/index.html` lines 1127–1216 · `website/assets/v3.css` lines 817–906, 1132–1134, 1138–1151 · `website/assets/v3.js` lines 443–497
**Screenshots:** `design-audit/screenshots/ftr3-{1440,768,390}.png`
**Bar:** billion-dollar website (Stripe / Linear / Apple / NYT / Bloomberg / GitHub).

---

## Executive Summary

The footer is the most operationally complete section of almobadir.com — it ships a CTA banner, a 5-column sitemap, a base bar, and a working live clock. Compared to the rest of the site (which is mostly editorial spectacle), this region is closest to a real product surface. That makes its lapses costly: the same footer that earns the right to be the site's information backbone also leaks two non-functional language buttons, an unsynced second newsletter form, dashed underlines on every contact item, a 1 Hz `requestAnimationFrame` clock that double-pumps with `setTimeout`, and a brand wordmark that overpowers the bar it sits in.

Held against the benchmark set, the structure is competitive (Stripe-class category density, Bloomberg-class palette discipline) but the execution is uneven: the CTA banner is too large to be a footer CTA and too small to be a hero (it duplicates the page's primary CTA from `nl3-section` — same headline pattern, same mailing intent, same arrow). The 5-column desktop grid is correct, but the column widths (`1.45fr .9fr .9fr 1fr .9fr`) are not earned — they encode a brand-column bias rather than information density. RTL handling is generally right (arrows point ←, transform on lang pill flips correctly, mark anchors right) but the contact list mixes baseline-aligned bidirectional text against a fixed `min-width: 54px` Latin-mono key, producing a ragged left edge in Arabic.

Forty-six findings below, ordered by severity. Phase 1 (Critical) corrects the lang toggle, the duplicate form, the live-clock loop, and the redundant CTA. Phase 2 (Refinement) tightens density, type, and bidi; Phase 3 (Polish) addresses the live-clock honesty, hairline contrast, and the "DOC. 001" archive aesthetic that doesn't actually appear in the footer (it appears upstream and the brief flagged it for cross-section consistency).

The footer is closer to shipping than the rest of the site. With the 12 critical fixes it would clear the GitHub bar; with all 46 it would sit comfortably with the NYT and Stripe references.

---

## Findings

### Phase 1 — Critical (block ship)

| # | Where | Finding | Severity | Recommendation |
|---|---|---|---|---|
| 1 | `index.html:1207–1209` · `v3.js:468–478` | Lang toggle is **fake**. AR/EN buttons fire a `ftr3:lang` CustomEvent that no listener consumes. Same defect as `.hdr3-lang-btn` (per discovery doc obs #9). The `aria-pressed` flips and the pill animates, but no content swaps. | Critical | Either (a) wire to an i18n table and actually swap copy, or (b) remove both lang toggles and stop promising bilingualism the site can't deliver. Half-implementation is worse than absence. |
| 2 | `index.html:1134` vs `index.html:927` | **Duplicate newsletter forms.** `.ftr3-cta__form` and `.nl3-form` are two separate components with two separate IDs (`ftr3-email`, `nl3-email`), two CSS rule families, and two JS submit handlers (`v3.js:480–492` vs the nl3 handler upstream). Same job, same destination, twice the code. | Critical | Promote one component (`<x-newsletter-form>` or a shared partial) and instantiate it in both places. The footer copy should differ, but the input + button + validation + success state must not. |
| 3 | `v3.js:452–454` | Live-clock loop is wrong: `raf = requestAnimationFrame(() => setTimeout(loop, 1000))` schedules a `setTimeout` *inside* a `requestAnimationFrame` — the rAF cancel on `visibilitychange` (line 455) cancels the rAF but **not** the setTimeout, so re-showing the tab can spawn a second concurrent loop on every visibility flip. After 5 hide/show cycles you'd have 5 ticks per second. | Critical | Use a single `setInterval(tick, 1000)` (it's a 1 Hz tick — rAF buys nothing here) and cancel/restart on `visibilitychange`. |
| 4 | `index.html:1129–1141` vs `index.html:912–937` | **Redundant CTA.** The footer's `.ftr3-cta` repeats the same offer as the dedicated newsletter section directly above it (`#newsletter`, section 7). Same value prop, same "أسبوعياً", same arrow. Stripe's footer never repeats the page's primary CTA — it offers a quieter secondary action (docs link, careers, brand assets). | Critical | Delete `.ftr3-cta` entirely and let the footer do footer work. Or, if kept, narrow it to one line ("بريدك. مرة في الأسبوع. اشتراك."), no eyebrow, no sub, no alt-text, in the base bar — not as a 96px-padded banner. |
| 5 | `v3.css:825` | `ftr3-cta__title` ramps to **60px**, larger than `--t-h2` body. Footer copy at 60px is unprecedented in the reference set: Stripe footer headings cap at ~14px; NYT at 13px; Linear at 15px; Bloomberg at 11px; GitHub at 14px. This is hero-class type doing footer work. | Critical | If the CTA stays, cap at `clamp(20px, 2vw, 28px)` semibold. The job is "go subscribe", not "stop scrolling". |
| 6 | `index.html:1145` | `<a href="#" class="ftr3-mark">` is a dead anchor — `href="#"` jumps to top, breaking standard "click logo to go home" expectation. In SPA contexts this also resets state. | Critical | `href="/"` (or `href="#top"` if the page is the home). Add `aria-current="page"` when on `/`. |
| 7 | `index.html:1148` | The wordmark SVG inlines `font-family="Mostaqbali, Tajawal, 'Noto Naskh Arabic', serif"` with `font-weight="900"` and `font-size="190"` — but Mostaqbali 404s (per Wave 1) so the visible wordmark is rendered in **Tajawal at 190px**. The mark is thus identity-fragile: ship the brand fonts or render the wordmark as a path, not as live text. | Critical | Convert the wordmark to outlined SVG paths. A logo cannot depend on a font that 404s — that's a fundamental identity bug. |
| 8 | `v3.css:849` · `index.html:1148` | The wordmark renders at `max-width: 380px` and `font-size="190"` inside a `viewBox 0 0 600 220`. At 1440 desktop with the 1.45fr column, this resolves to ~340 × 124 px — **roughly 8% of footer height**. By comparison: Stripe wordmark in footer is ~100×24px; GitHub's is ~32×32 (mark only); Linear's is the wordmark at ~22px. Almobadir's footer mark is ~5× the benchmark. | Critical | Cap the wordmark at `max-width: 200px`. The footer is not the place to re-establish the brand — visitors arrived already knowing it. |
| 9 | `v3.css:854` · `index.html:1154` | The "live clock" pill is decorative theater. It claims `aria-live="polite"` (so it announces every second to screen readers — a known anti-pattern: it will never shut up), uses a **hardcoded local server time** rendered through `Intl.DateTimeFormat({timeZone:'Asia/Riyadh'})` (correct for AST), but pulses a green dot suggesting "live data" when nothing about the page is actually live. | Critical | Either drop `aria-live` (the time updates visually; SR users don't need a tick announcement every second), or fix to `aria-live="off"` and set `aria-atomic="false"`. Better: only set `aria-live` if you intend hourly chime announcements. |
| 10 | `v3.css:898–899` | `ftr3-lang__pill` uses `transform: translateX(-100%)` for the EN state. In RTL, this translates the pill **left by its own width**, but the pill is positioned `right: 3px` with `width: calc(50% - 3px)` — so EN-active sends the pill to the **left half**, where the EN button is. That's correct. **However** there's no `[dir]`-aware fallback: if `dir` is dynamically set to `ltr` by the (broken) lang toggle, the pill will animate the wrong direction. | Critical | Use `inset-inline-end: 3px` + `inset-inline-start: auto` for the resting state, and switch to logical translation. Right now the toggle is fake so no one notices, but if fix #1 ships, this breaks. |
| 11 | `v3.js:466–467` | "Back to top" button uses native `scrollTo({top:0, behavior:'smooth'})` with **no fallback** for `prefers-reduced-motion`. Smooth scrolling for 14,809 px takes ~1.6s — disorienting at full-motion, nausea-inducing for vestibular users. | Critical | `const prm = matchMedia('(prefers-reduced-motion: reduce)').matches; window.scrollTo({top:0, behavior: prm ? 'auto' : 'smooth'});` |
| 12 | `index.html:1190–1196` · `v3.css:879` | The contact list mixes interactive `<a>` rows with non-interactive `<span>` rows that still receive the same dashed border-bottom styling. Visually identical, behaviorally different. Hover the location row → no cursor change, nothing happens. | Critical | Either make all 5 rows interactive (location → Google Maps, phone → tel:, region → none), or visually distinguish the static rows (no underline, no hover affordance). Currently inconsistent. |

### Phase 2 — Refinement (elevate the experience)

| # | Where | Finding | Severity | Recommendation |
|---|---|---|---|---|
| 13 | `v3.css:841` | Grid template `1.45fr .9fr .9fr 1fr .9fr` is hand-tuned but not principled. The 1.45 brand column eats horizontal real estate that the link columns need. | Moderate | Try `1fr 1fr 1fr 1fr 1fr` and let the wordmark cap at 200px. If the brand needs more, use `1.2fr repeat(4, 1fr)` — never 1.45×. |
| 14 | `v3.css:822` | `.ftr3-cta__inner` uses `display:grid` with `gap: clamp(16px, 2vw, 28px)` for 5 stacked elements. Vertical rhythm is loose — 28px between eyebrow and 60px headline is too tight; 28px between sub and form is too wide. | Moderate | Drop the grid, use explicit margins with paragraph hierarchy: `margin-top: 12px` after eyebrow, `margin-top: 20px` after title, `margin-top: 32px` before form. |
| 15 | `v3.css:825` | `ftr3-cta__title` `letter-spacing: -.025em` is too tight for Arabic at 60px. Arabic doesn't tighten the way Latin does — characters are already connected. | Moderate | Drop to `-.01em` or `0` for Arabic display. The `--ls-h2: -.028em` token also needs an Arabic exception. |
| 16 | `v3.css:827` | `ftr3-cta__line--accent` gradient runs `var(--ink) → var(--ink-2) → var(--crimson)` at `120deg`. In RTL the gradient is **not flipped** — crimson lands on the right side regardless of language. Likely intended for English left-to-right reading but the page is Arabic. | Moderate | Use `0deg` (vertical) for direction-neutrality, or flip to `60deg` so crimson sits on the leading (right) edge in RTL. |
| 17 | `v3.css:828` | `.ftr3-cta__sub` `max-width: 62ch` matches body text but the eyebrow above it has no max-width, and the form below caps at 520px. Three different widths, no shared rhythm. | Moderate | Set a single `--ftr3-cta-measure: 540px` and constrain all three to it. |
| 18 | `v3.css:829` | `ftr3-cta__form` uses `padding: 8px` with an inner button at `padding: 10px 22px` — touch target is 18px tall before the visual hit area kicks in. iOS HIG / WCAG demand 44×44. | Moderate | Outer `padding: 6px` + inner button `padding: 14px 26px` → total ~46px tall. |
| 19 | `v3.css:833` | `.ftr3-cta__btn` has `font: 600 14px/1 var(--font-ar-body)` — `line-height: 1` on a button label clips Arabic descenders (the ـة / ي tail). | Moderate | `line-height: 1.2` minimum on Arabic button labels. |
| 20 | `v3.css:834` | `.ftr3-cta__btn:hover` does both `background: var(--crimson); color: var(--ink); transform: translateX(-2px)` — three changes on hover. Reference set uses one or two: GitHub does brightness only, Stripe does color only, Linear does scale only. | Moderate | Pick one: just background swap, or just translate. Combined feels jittery. |
| 21 | `v3.css:837` | `.ftr3-cta__alt` is 13px on a dark surface at `var(--ink-mute)` (rgba(245,234,210,.52)). Contrast against `--void` is approximately 4.4:1 — passes WCAG AA Normal but fails AA Large at 13px non-bold. | Moderate | Bump to `--ink-soft` (.78 alpha → ~6.2:1) or 14px regular. |
| 22 | `v3.css:838` | `border-bottom: 1px solid var(--hairline-strong)` on the inline link — inherits the "underline links" pattern from Practical Typography; correct. But `padding-bottom: 1px` with a 1px border on a 13px line creates a sub-pixel gap that anti-aliases differently across browsers. | Moderate | `text-underline-offset: 3px` + `text-decoration: underline` + `text-decoration-thickness: 1px` is the modern equivalent and renders consistently. |
| 23 | `v3.css:842` | `.ftr3-col` enters with `opacity:0; transform: translateY(14px)` and reveals on intersection. **But on `prefers-reduced-motion`** (line 1145), opacity is forced to 1 — correct. However, the JS `IntersectionObserver` (v3.js:457–462) still runs and sets `is-in`, which adds the dot scale-in animation — `.ftr3-cdot` is then forced via line 1150 to `transform: scale(1)`. Net: PRM users see no motion, but the JS still does the work. Discovery doc obs #10 flagged this site-wide. | Moderate | Short-circuit the IO setup if `matchMedia('(prefers-reduced-motion: reduce)').matches`. |
| 24 | `v3.css:850` | `.ftr3-mark__svg { transition: transform .6s }` runs on hover (`translateY(-2px)`). On a logo, the link goes nowhere semantically (#) so the hover is pure ornament. | Moderate | If the mark gets `href="/"`, keep the hover. If it stays `#`, drop the hover and the cursor pointer. |
| 25 | `v3.css:852–853` | Brand column has `.ftr3-tag` (clamp 18-22px display, weight 700) above `.ftr3-desc` (14px body, line-height 1.7) — three voice levels in a 4-element column. Reference: Stripe footer brand col is logo + 1 line + nothing else. | Moderate | Drop `.ftr3-tag` or `.ftr3-desc`, not both. Currently competing for the same eye fix. |
| 26 | `v3.css:854` | `.ftr3-clock` border `1px solid var(--hairline)` (rgba(245,234,210,.08)) — that's 0.08 alpha. Contrast against `--void` is sub-1.5:1; the border is invisible on most monitors. | Moderate | Use `var(--hairline-strong)` (.18 alpha) — still subtle, actually visible. |
| 27 | `v3.css:859` | Column heads `.ftr3-h` are 11px mono caps with `letter-spacing: .18em` — readable but under the 12px floor most type systems use for caps. The `--t-eyebrow: 11px` token confirms the system intent. Acceptable but tight. | Moderate | If kept at 11px, ensure `font-weight: 500` (already set) and the resolved monospace renders heavier than Helvetica's 400. JetBrains Mono Medium is fine. |
| 28 | `v3.css:861–864` | Link rows hover `transform: translateX(-3px)` — a 3px translation on a row 8px tall is harsh. The arrow simultaneously fades in, so two things move at once. | Moderate | Choose one: either translate the row OR slide the arrow in. Not both. |
| 29 | `v3.css:863` | `.ftr3-arrow` is a literal `←` text glyph, not an SVG. In Arabic typeface fallback (Tajawal), the arrow can render with metrics that misalign vertically against the link text. | Moderate | Replace with the same SVG arrow used in `.ftr3-cta__btn` and `.ftr3-mailbtn` for consistency. |
| 30 | `v3.css:866` | `.ftr3-cdot` enters with `transform: scale(0)` and animates to scale(1) when `.ftr3-col.is-in` fires. The bullets thus pop in — every bullet on every col load — adding 6 + 5 + 5 = 16 micro-animations per visit. | Moderate | Animate only on first paint, not on re-intersect. Or drop entirely; the dots can just exist. |
| 31 | `v3.css:870` | `.ftr3-handle` (13px mono) and `.ftr3-meta` (10px mono caps tracked .12em) — the Latin handles vs Arabic metas creates two type cultures in one row. The eye lands on `@almobadir_mindset` (LTR) and is yanked back to `MINDSET` on the other end (LTR caps). | Moderate | Either set the meta in Arabic (`عقلية`) to match the row's Arabic frame, or accept the bidi but match weights and tracking. Currently 10px caps competes with 13px lowercase mono. |
| 32 | `v3.css:872` | `.ftr3-meta` color is `--ink-quiet` (.32 alpha) — that's ~2.6:1 against `--void`. Fails AA. | Moderate | Use `--ink-mute` (.52, ~4.4:1) at minimum. |
| 33 | `v3.css:879–880` | `.ftr3-list--contact li` and `.ftr3-list--contact a` BOTH set `border-bottom: 1px dashed var(--hairline)` — duplicate declaration on parent and child. Second one wins on `<a>`, first wins on `<span>` rows. Result: visual consistency happens by accident, not by design. | Moderate | Move to `.ftr3-list--contact > * { border-bottom: 1px dashed var(--hairline); }`. |
| 34 | `v3.css:879` | `align-items: baseline` with `.ftr3-k` at `min-width: 54px` and `.ftr3-v` at variable width creates a Latin baseline grid that fights Arabic baseline. The mono key sits visually higher than the Arabic value at the same `font-size`. | Moderate | `align-items: center` or `flex-end`. Baseline-aligning across scripts at different x-heights is a known typographic trap. |
| 35 | `v3.css:882` | `.ftr3-k min-width: 54px` is enough for "بريد", "الموقع", "الاتصال", "المنطقة" — but "الوكالة" (`الوكالة`) is wider in Cairo/Tajawal at 10px caps. Either the row wraps or the Latin column expands. | Moderate | Inspect actual widths in DevTools and set `min-width: max-content` or bump to 72px. |
| 36 | `v3.css:889` | `.ftr3-base` has `background: rgba(0,0,0,.18)` — a 0.18 black overlay on top of `--void` (#0A0F1E) yields ~#080C18, a near-imperceptible darkening. Either commit (use `--void-3` directly) or skip. | Moderate | `background: var(--void-3)` for a real visual separation; or remove and let the border-top do the work. |
| 37 | `v3.css:890` | `.ftr3-base__inner` is 3-column `1fr auto 1fr` — center column (tags) is `auto`, side columns share remaining space. At 1440px this works; at 980–1180px the tags cluster center-right because the lang toggle + back-to-top expand the right column. | Moderate | Either fix all 3 columns or use flex `space-between`. Currently the alignment is breakpoint-fragile. |
| 38 | `v3.css:891–892` · `index.html:1203–1204` | Base copy is `© 2026 ALMOBADIR · SAUDI ARABIA · +966` and tags are `ARABIC FIRST · DARK MODE NATIVE · MADE WITH INTENT` — 6 mono caps strings, 4 dot separators. Heavy chrome for a footer base bar. NYT base bar: 1 line of plain text + 5 unobtrusive policy links. | Moderate | Cut tags (lines 1204) entirely. They're a vanity declaration ("look how intentional we are") that adds zero user value. The copyright + jurisdiction is sufficient. |

### Phase 3 — Polish

| # | Where | Finding | Severity | Recommendation |
|---|---|---|---|---|
| 39 | `index.html:1148` | Inline SVG `letter-spacing="-2"` on the wordmark — SVG accepts unitless letter-spacing as user-units. At a 600-unit-wide viewBox this is small but still a magic number. | Minor | Move to a CSS rule on `.ftr3-mark text` so the value lives next to the rest of the type system. |
| 40 | `index.html:1149` | `<circle cx="492" cy="60" r="9" fill="#E6213B"/>` — accent dot on the wordmark uses the same crimson hex literal that appears 4× in v3.css and once in v3.js (per Wave 1 obs #4). Token bypass. | Minor | `fill="var(--crimson)"` (works in inline SVG). |
| 41 | `v3.css:870` · `index.html:1159–1164` | All 6 IG handles render with `@` prefix in Latin mono, but the Arabic name comes after as `meta`. The rapid `@` punctuation in 6 vertical rows creates an unintended vertical "comb" effect at the leading edge. | Minor | Consider dropping the `@` since the column already says "الشبكة" (the network). The `@` is for outside-the-product context. |
| 42 | `v3.css:875` | Platform link `:hover svg { color: var(--crimson) }` — the only color cue on hover. Text doesn't change weight or color. The icon-only signal is weak. | Minor | Add `color: var(--ink)` on the link text on hover, matching the network list pattern. |
| 43 | `index.html:1181` | Threads icon is a generic circle (`<circle cx="12" cy="12" r="11">`). All other platforms have brand-recognizable glyphs. | Minor | Use the actual Threads "@" wordmark or the official squiggle glyph. A blank circle next to "Threads" is amateur. |
| 44 | `index.html:1182–1183` | Two YouTube rows: `@almobadir` and `@almobadir-motivation` — the network already lists `@almobadir_mindset` (the motivation channel?). Possible duplication or two genuinely different YouTubes. The label "YouTube · YouTube" reads as a copy-paste error to scanners. | Minor | Differentiate visually: "YouTube — أساسي" vs "YouTube — تحفيز", or merge both behind one row that opens a YouTube channel hub. |
| 45 | `index.html:1192` | "almobadir.media" link includes a `<span class="ftr3-pill ftr3-pill--ghost">↗</span>` — an external-link arrow as a pill. The rest of the footer's external links (6 IG + 5 platform + 1 mailto) get no such marker. Inconsistent. | Minor | Either mark all external links or none. The 6 IG handles are obviously external (Instagram domain); the `↗` is doing semantic work but only on one link. |
| 46 | `index.html:1194` | Contact value `+966` is the Saudi country code only — no actual phone number. This is a placeholder, not a contact. | Minor | Either ship a real number ("+966 5X XXX XXXX") or remove the row. Showing a country code as a phone field is performative. |

---

## Vanity Engineering Review

| Pattern | Evidence | Severity | Verdict |
|---|---|---|---|
| **Two newsletter forms doing one job** | `nl3-form` + `ftr3-cta__form`; separate CSS, separate JS submit handlers | V2 — Structural | The footer form is a smaller copy of the section form. One component, two mounts. Currently 2× the cost, 1× the value. |
| **Live clock no one needs** | rAF loop + `aria-live="polite"` + green pulse | V1 — Drag | Stripe has no clock. Linear has no clock. Bloomberg has no clock. The user knows what time it is. The clock is here to signal "alive system" but nothing on this site is time-sensitive — the clock is a costume. **Kill criteria:** If the footer ships and we can't point to one reader who said "I checked the Riyadh time on your footer", delete the clock. |
| **Fake lang toggle** | Wires `aria-pressed` and a pill animation but consumes nothing | V2 — Structural | Promising bilingualism without delivering it is worse than an Arabic-only site. The toggle is here because a "real footer has language switching" — a reference-set imitation, not a feature. |
| **"DOC. 001 / EDITORIAL CHARTER" archive aesthetic** | Reused upstream (manif3, fnd3a per discovery); footer's `ARABIC FIRST · DARK MODE NATIVE · MADE WITH INTENT` is the same idiom | V1 — Drag | The "we are a serious archive" affect is a bet on aesthetic mimicry of NYT/Bloomberg without their underlying institutional weight. The tag bar adds nothing readers want. Cut it; nothing breaks. |
| **5-col grid bias toward brand** | `1.45fr .9fr .9fr 1fr .9fr` | V0 — Cosmetic | The grid is honest about its bias (brand col gets 38% more width than link cols). The bias is wrong for an information-dense footer. Equal columns. |
| **34th @keyframes (`ftr3-pulse`)** | One more pulse animation atop the 8 already in the page | V1 — Drag | A green dot doesn't need to ping for a clock to communicate "live". The pulse is here because pulses signal vitality. Mute it. |

**Vanity Score (RCR): 5/10.** The footer was engineered for a reference set the site has not earned operationally. Strip the duplicate form, the lang toggle, the live clock, and the tag bar — RCR drops to 3/10 and the footer becomes faster, smaller, and more honest.

**The Hard Question:** *If the entire `.ftr3-cta` section were deleted and the footer started directly at the sitemap, would conversion drop?* Almost certainly not — the page just had a 1,065-px-tall newsletter section three scrolls above. The footer CTA exists because every reference footer has one, not because almobadir's funnel needs one.

---

## Benchmark Comparison

| Dimension | Almobadir | Stripe | Linear | Apple | NYT | Bloomberg | GitHub |
|---|---|---|---|---|---|---|---|
| CTA banner | Large (60px headline, 96px pad) | None | None | None | Tiny inline | None | Tiny inline |
| Sitemap cols | 5 (1.45/.9/.9/1/.9 fr) | 5 equal | 4 equal | 5 equal | 7 equal (compressed) | 4 equal | 5 equal |
| Wordmark size | ~340px wide | ~100px | ~80px | ~16px (Apple logo) | "The New York Times" ~180px | "Bloomberg" ~160px | Mark only ~32px |
| Live clock | Yes (with pulse) | No | No | No | No | No | No |
| Lang toggle | Fake | Real (per locale) | None | None | Real | Real | Real |
| Newsletter form | Yes (duplicate of section) | No | No | No | Yes (single, in footer only) | No | No |
| Tag/aesthetic strip | "ARABIC FIRST · DARK MODE NATIVE · MADE WITH INTENT" | None | None | None | None | None | None |
| Back-to-top | Yes | No | No | No | No | No | No |

**Read:** Almobadir's footer is heavier than every benchmark on every dimension where heaviness is optional. The two areas where it leads (live clock, tag strip) are the two areas where the benchmarks deliberately chose nothing.

---

## Recommended Actions (Implementation Order)

1. **Delete the duplicate CTA banner** (#4) — single biggest reduction. Keeps the footer doing footer work.
2. **Delete the tag strip** (#38) — removes 3 mono-caps strings of pure aesthetic.
3. **Fix or kill the lang toggle** (#1) — promise less, deliver more.
4. **Fix the live-clock loop** (#3) and `aria-live` (#9) — correctness.
5. **Outline the wordmark to paths + cap at 200px** (#7, #8) — solves font-fidelity gap and over-scaling in one move.
6. **Dedupe the form component** (#2) if the CTA stays.
7. **Equal 5fr columns** (#13).
8. **Phase 2 type/density/contrast pass** (#14–#38).
9. **Phase 3 polish** (#39–#46).

---

## 200-Word Summary

The almobadir footer is structurally competent and visually overcooked. It contains the right pieces (CTA, sitemap, base bar, lang toggle, back-to-top, live clock, brand mark, contact list) but treats every piece as a hero — 60px headlines, 340px wordmarks, animated bullet dots, a pulsing clock, six mono-caps strings on the base bar, and a 96px-padded CTA banner that duplicates the page's primary subscribe section three scrolls above.

The two functional defects are concrete: the lang toggle fires a CustomEvent nothing listens to, and the live-clock loop schedules a setTimeout inside a requestAnimationFrame whose cancel doesn't reach the timeout — visibility flips can spawn concurrent ticks. The newsletter form duplicates `nl3-form` byte-for-byte in intent. The wordmark depends on a 404'd font and renders in Tajawal at 190px. RTL handling is mostly right; the gradient direction on the accent line and the contact-list baseline alignment are the bidi hazards.

Held against Stripe, Linear, Apple, NYT, Bloomberg, and GitHub, this footer is heavier than every benchmark on every dimension where heaviness is optional. The 12 critical fixes get it to GitHub-tier; the full 46 get it to NYT-tier.
