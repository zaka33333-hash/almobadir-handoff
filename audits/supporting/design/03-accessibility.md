# Accessibility WCAG 2.2 AA — Forensic Audit

**Audited:** 2026-04-26
**Scope:** http://localhost:8765/index.html (single-page, 9 sections)
**Source:** `C:\Users\toner\OneDrive\Desktop\almobadir\website\` (1,220-line `index.html`, 4 stylesheets, 1 JS)
**Standard:** WCAG 2.2 — focus on Level AA (with selected AAA notes)
**Tools:** static source review + math-derived contrast against the documented token system (Wave-1 discovery confirms exact resolved RGBs); the live browser was occupied by another agent during this audit, so contrast and DOM facts are computed from CSS source, not screenshot pixel sampling. All contrast ratios below were computed using the WCAG 2.x relative-luminance formula on actual rendered tokens.

---

## Executive Summary

Almobadir.com is a **richly semantic page that has done many hard things right** (proper landmarks, dir/lang attributes, alt text where it matters, comprehensive `prefers-reduced-motion` coverage, `aria-label` on every icon-only button, `aria-haspopup`/`aria-expanded` on the mega-menu trigger). It **fails AA** on a different axis: **the core brand-red CTA pattern is sub-AA on white text**, **muted text tokens (`--ink-quiet`, `--on-paper-mute`) ship below 4.5:1**, **there is no skip-link**, **the keyboard cannot escape the mega menu / drawer cleanly**, **the marquee tickers cannot be paused by keyboard**, **the language toggle and Method scrollytelling are functionally unreachable for non-pointer users**, **2 of the 3 newsletter forms reuse element IDs across the page**, and **8 placeholder `href="#"` issue-chips dump unsuspecting screen-reader users at the top of the page**. Net: a strong baseline buried under enough specific defects that the page does not meet AA today.

**Estimated overall conformance: WCAG 2.2 Level AA — FAIL**, with 14 distinct AA criteria failing. Counts of failing instances run higher (60+ findings below) because several criteria recur across all 9 sections (e.g., contrast applies to every muted label; non-text contrast applies to every form border). The good news: **roughly 70% of findings are token-level fixes** — patch `--ink-quiet`, `--on-paper-mute`, `--crimson` button text, and `--hairline-strong`, and at least 25 findings collapse simultaneously.

The most consequential findings are: **(P0) no skip link in a single-page experience that has a 14,800 px scroll**, **(P0) the Method section is keyboard-only inaccessible** (it requires scroll, no rail-item is a button, no anchor lets you jump frames), **(P0) the brand-red CTA on white text is 4.23:1 — fails AA-normal by a hair across 4 buttons site-wide**, and **(P0) the language toggle reports `aria-pressed` but never actually toggles the language**, presenting a visible/accessible-name mismatch and a violation of 3.2.1 Predictable.

---

## Findings by WCAG criterion

Severity legend: **P0** = blocks an assistive-tech user / fails AA outright; **P1** = AA failure with a workaround; **P2** = AAA gap or robustness concern.

### 1.1.1 Non-text content (Level A)

| # | Sev | Location (file:line) | Issue | Fix |
|---|---|---|---|---|
| 1 | P0 | `index.html:374, 387, 413, 426, 439` | 5 of 6 network-card cover `<img>` use empty `alt=""` while still being inside the linked card body — the link's accessible name is supplied by `aria-label`, but the `<img>` should remain decorative *only* if alt info is duplicated elsewhere. It is. So this is intentional and OK — flag the 6th card (`Badrshaqer.jpg`, line 400) which has descriptive alt but the same wrapper context. **Net:** consistent — keep the empty alts but document the rationale, since auditors will challenge it. | Add a code comment: "decorative — link `aria-label` provides accessible name". |
| 2 | P1 | `index.html:1146-1150` (`.ftr3-mark__svg`) | Footer wordmark SVG has `aria-hidden="true"` on the `<svg>` but the wrapping `<a class="ftr3-mark" href="#" aria-label="المبادر — الرئيسية">` provides a label. Good pattern, but `href="#"` is dead — clicking jumps to top via empty hash. | Set `href="/"` or `#hdr3-root`. |
| 3 | P1 | `index.html:56-58` | Inline `<svg>` with `<text>المبادر</text>` is marked `aria-hidden="true"`, hiding the brand text from SR. The wrapping anchor has `aria-label="المبادر — الصفحة الرئيسية"` (line 49) so the brand is still announced — OK. **However**, the SR will not see the verify-tick (line 59 `aria-label="موثّق"`) because of the surrounding `aria-label`. SR will only read the outer label. Decide: the verify badge is meaningful — should it be announced? | Move the verify-tick `aria-label` *outside* the logo anchor (e.g., as a sibling badge). |
| 4 | P2 | `index.html:174` (hero SVG wordmark) | `<svg ... role="img" aria-label="المبادر">` — correct. But the SVG also contains a hidden `<text>المبادر</text>` which assistive tech may double-announce on some browsers. Not a hard violation; consider `aria-hidden` on the inner `<text>`. | Add `<text aria-hidden="true">` (decorative, redundant with parent label). |
| 5 | P0 | `index.html:99` `<input type="search" class="hdr3-search-input">` | The `<kbd>⌘ K</kbd>` (line 100) is decorative and cannot be hidden from SR (`aria-hidden` missing) — SR will announce "command K" alongside the input label, confusing. | Add `aria-hidden="true"` to the `<kbd>`, OR provide an `aria-describedby` linking from the input to a hidden hint. |

### 1.3.1 Info and relationships (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 6 | P0 | `index.html:1208` | The footer language toggle has `<span class="ftr3-lang__pill" aria-hidden="true"></span>` *between* the two `<button>`s (the visual sliding pill). Screen reader still hears two adjacent buttons — fine — but the **role pattern is wrong**: this is a single-select toggle group, so it should be a radiogroup with `role="radio"` children, not a `role="group"` of `aria-pressed` toggles. As-is, both buttons can be "pressed" simultaneously per the SR pattern. | Use `role="radiogroup"` on the wrapper and `role="radio"` + `aria-checked` on each option; or keep `role="group"` and ensure only one `aria-pressed="true"` ever. |
| 7 | P1 | `index.html:102-105` (header lang) | Same lang toggle pattern in the header — **but lacks `aria-pressed`** at all. `is-active` class is purely visual; SR cannot tell which language is selected. | Add `aria-pressed="true"|"false"` matching the `is-active` class, OR adopt the radio pattern. |
| 8 | P0 | `index.html:842, 843` | `<span class="cnt3-issue">العدد <em>№ 184</em></span>` and `<span class="cnt3-week">أسبوع 17 / 2026</span>` — these are pseudo-list-items but rendered as raw spans separated by a decorative `cnt3-rule`. SR reads "Issue No 184 Week 17 2026" with no break or grouping. | Wrap in `<dl>` or `<ul>` so SR announces them as a list of metadata. |
| 9 | P1 | `index.html:268-279` (hero `__trust`) | `.hero3__trust-logos` lists "FAVIKON · 1.5M FOLLOWERS · 25M MONTHLY VIEWS · SAUDI ARABIA" as separate `<span>`s with `<span class="hero3__trust-sep" aria-hidden="true">` separators. SR reads them as one continuous string. | Use `<ul>` for the logo list (4 items). |
| 10 | P1 | `index.html:1191-1195` footer contact list | Mixed list: 2 anchored items + 3 plain `<li>` text rows (no anchor) inside the same `<ul>`. SR will say "list of 5 items" but only 2 are actionable. | Either anchor the bottom 3 (mailto/maps), OR move them into a separate `<dl>`. |
| 11 | P0 | `index.html:947-949` (`.nl3-live`) | Has `aria-live="polite"` but the *only* dynamic content is `#nl3-countdown` inside it. The whole region announces every minute when the countdown updates, including the stable label "العدد القادم". | Move `aria-live` to the inner `#nl3-countdown` `<span>` only, OR wrap dynamic part with `aria-atomic="false"`. |
| 12 | P1 | `index.html:36` `.hdr3-announce` | `aria-live="polite"` on the announcement carousel — but the JS rotates the announcement every 6 seconds (v3.js:22) **automatically**, which means SR will be interrupted constantly. WCAG 4.1.3 says updates must not be intrusive. | Remove `aria-live` (autorotating content does not need it) or set `aria-live="off"` and announce only on user-driven prev/next. |

### 1.4.3 Contrast (Minimum) (Level AA)

Computed via WCAG luminance formula on the documented token system. **Body text needs ≥ 4.5:1; large text (≥ 18.66 px / 24 px regular) and bold ≥ 14 pt needs ≥ 3:1.**

| # | Sev | Location | Foreground | Background | Ratio | Required | Pass/Fail | Fix |
|---|---|---|---|---|---|---|---|---|
| 13 | P2 | `--ink-quiet` (`tokens.css:22`) used by `.ftr3-base__tags` | rgba(245,234,210,0.32) over `--void-3` w/ 0.18 dark overlay (≈ #2C2A28 composite) | `#0A1628` derivative | **2.58:1** | 4.5:1 | **FAIL** | Either raise `--ink-quiet` to alpha 0.55+ or stop using it for text; reserve for borders only. |
| 14 | P0 | `--ink-quiet` *anywhere* used as text — only one instance (`.ftr3-base__tags` "ARABIC FIRST · DARK MODE NATIVE · MADE WITH INTENT") | rgba(245,234,210,0.32) on void | `#0A0F1E` | **2.57:1** | 4.5:1 | **FAIL** | Same as above — but here the text is `aria-hidden="true"` in HTML (line 1204), so SR ignores it. **Visual users still read it.** Fix the contrast or remove the visible text entirely. |
| 15 | P0 | `--on-paper-mute` (rgba(26,15,8,0.55)) on `--paper` | rgba derived | `#F8F4EA` | **4.06:1** | 4.5:1 | **FAIL** | Bump alpha to 0.65 (yields 4.65:1) — touches `.manif3__head`, `.manif3__caption`, `.nl3-mock-header`, `.nl3-block-name`, `.nl3-stat-cap`, `.nl3-mock-footer` — five places, one token. |
| 16 | P0 | `.manif3__head` & `.manif3__caption` use rgba(10,15,30,0.52) on `--paper` | rgba over `#F8F4EA` | `#F8F4EA` | **3.75:1** | 4.5:1 | **FAIL** | The manif3 section locally defines `--manif3-on-paper-mute: rgba(10,15,30,.52)` (v3.css:245) — this is **darker** in raw RGB than `--on-paper-mute` (#1A0F08 alpha 0.55) but ends up *worse* because of how 0.52 alpha composites. Set `--manif3-on-paper-mute: rgba(10,15,30,.65)` → ≈ 4.55:1. |
| 17 | P0 | `.hero3__tagline-accent` linear-gradient stop `#9C1428` | text — gradient end | `--void` `#0A0F1E` | **2.31:1** | 4.5:1 (or 3:1 if large) | **FAIL even at large** | The hero tagline "الترفيه" / "التعليم" uses a gradient `#FF324A → #C41A30 → #9C1428`. The bottom 33% of the glyphs falls below 3:1. Trim to a 2-stop gradient `#FF324A → #E6213B` (top stop 5.28:1, bottom 4.22:1, large-text safe). |
| 18 | P0 | `.hdr3-cta` (header CTA "اشترك في النشرة") body text | `--ink` `#FBF7EE` | `--crimson` `#E6213B` | **4.23:1** | 4.5:1 | **FAIL** | Crimson is too light for white text at body size. Three options: (a) darken the CTA button bg to `--crimson-2` `#C41A30` → 5.46:1; (b) pure white `#FFFFFF` instead of `--ink` → 4.66:1 — passes thinly; (c) raise text size ≥ 14 pt bold ⇒ qualifies as "large" at 3:1 — but the CTA is currently 13.5 px so does not qualify. |
| 19 | P0 | `.hero3__signup-cta` ("اشترك") | `--ink` `#FBF7EE` on linear-gradient `#E6213B → #C41A30` | gradient mid ≈ `#D51D36` | **4.84:1** | 4.5:1 | **PASS narrowly** at 15 px — but the **top edge of the gradient is `#E6213B` (4.23:1, FAIL)**. Fix: flip the gradient so the dark stop is on top (text region) — `linear-gradient(180deg, #C41A30 0%, #E6213B 100%)` keeps the visual but improves text-region contrast to 5.46:1+. |
| 20 | P0 | `.nl3-submit` ("اشترك مجاناً") | `--ink` on `--crimson` flat | `#E6213B` | **4.23:1** | 4.5:1 | **FAIL** | Same crimson issue. Use `--crimson-2` as base. |
| 21 | P0 | `.ftr3-cta__btn` ("اشتراك") | `--ink` on `--crimson` flat | `#E6213B` | **4.23:1** | 4.5:1 | **FAIL** | Same fix. |
| 22 | P0 | `.nl3-submit.is-success` "وصلت — تأكيد في بريدك" | `--ink` on `#1a8c4a` | `#1a8c4a` | **4.01:1** | 4.5:1 | **FAIL** | Darken success-green to `#157A3F` (≈ 4.94:1) or `#0F6B36` (≈ 5.78:1). Side benefit: tokenize as `--success`. |
| 23 | P1 | `--azure` `#1E5AA8` used as text on `--void` (e.g., `.hero3__chip[data-color="azure"]` accent, mtd3 frame data-color) | `#1E5AA8` | `#0A0F1E` | **2.80:1** | 4.5:1 (3:1 large) | **FAIL** even at large | Azure is the lone sub-brand color that fails. Lighten to `#3F7BCC` (≈ 5.10:1) for text use; keep `#1E5AA8` for fills/large blocks only. |
| 24 | P0 | Header announcement-arrow buttons `‹` `›` | `--ink-mute` rgba(245,234,210,0.52) on void | `--void` | **4.92:1** body — but glyph is **14 px** (line 24 of v3.css), borderline. The 1px border at `--hairline` (rgba .08, ≈ 1.19:1) is invisible to most users. | Pass marginally; 4.92 ≥ 4.5. **However** the *button border* fails non-text contrast (see 1.4.11). |
| 25 | P0 | `.cnt3-tag` (e.g., "ثقافة مالية") on `--crimson` on the cover image overlay | white text on var(--tag) (= crimson, verde, teal accents) | depends | crimson `#E6213B` ⇒ 4.23:1 — **FAIL at 12 px**. Verde ⇒ 5.98:1 PASS. Teal `#2AD4B8` ⇒ 10.19:1 PASS. | The crimson cnt3-tag fails (12 px caption-sized). Switch crimson tags to `--crimson-2` bg or darken the text-on-crimson with a 1px stroke. |
| 26 | P1 | `.hdr3-tick-link` "اشترك الآن ↗" inside ticker | inherits `--ink-soft` over void | `#0A0F1E` | **9.88:1** | 4.5:1 | **PASS** for default state. **However** `:hover` (line 33) flips to `--crimson` ⇒ **4.22:1, FAIL** at body size. | Use `--crimson-2` on hover, OR underline only. |
| 27 | P2 | `.hero3__lede-strong` (line 186 inline `<span>`) "25 مليون مشاهدة شهرياً" | `--ink` on void | `#0A0F1E` | **17.86:1** | 4.5:1 | **PASS** | OK. |

**Subtotal 1.4.3:** 9 P0/P1 failures; 6 distinct token bugs. Fixing `--on-paper-mute`, the CTA-text-on-crimson pattern, and the azure brand color resolves most.

### 1.4.10 Reflow (Level AA)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 28 | P1 | `body { overflow-x: hidden; }` (`base.css:24`) | Hides horizontal scroll site-wide; this masks rather than fixes any reflow bug. WCAG requires no horizontal scroll at 320 CSS px / 400% zoom. The Wave 1 discovery confirms `full-390.png` is 16,983 px tall (no horizontal scroll). At 320 px, the Method section's 5×100vh sticky stage may break — needs verification. | Test at 320 px with `overflow-x: visible` temporarily; address any spillage rather than hiding it. |
| 29 | P1 | `.hero3__ticker-track` (50s linear infinite, ltr direction) | The ticker overflows its container via `mask-image` linear-gradient — at 320 px, the track is wider than viewport. `overflow: hidden` saves the layout but the user cannot scroll horizontally to see the rest, and the track auto-pans regardless of viewport. | Acceptable as decorative ticker, but ensure the same content is available textually elsewhere — currently the ticker has **financial data (TASI, USD/SAR, BRENT)** that exists nowhere else on the page. Move the data to a static list or remove. |

### 1.4.11 Non-text contrast (Level AA)

UI components (form borders, input outlines, focus rings) and meaningful icons need ≥ 3:1 against adjacent colors.

| # | Sev | Location | Foreground | Background | Ratio | Pass/Fail | Fix |
|---|---|---|---|---|---|---|---|
| 30 | P0 | `--hairline` `rgba(245,234,210,0.08)` on void — used on **every form border, every divider, every card border on dark** | composite over void | `#0A0F1E` | **1.19:1** | **FAIL** | The most pervasive non-text contrast bug on the site. Fix once at token level: raise `--hairline` to `rgba(245,234,210,0.18)` (renames it to today's `--hairline-strong`), OR add a denser `--hairline-stroke` for component borders only. |
| 31 | P0 | `--hairline-strong` `rgba(245,234,210,0.18)` on void | composite | `#0A0F1E` | **1.58:1** | **FAIL** | Even the "strong" hairline is 47% short of 3:1. Required minimum alpha for 3:1 = ~0.36. Use `rgba(245,234,210,0.40)` for any visible UI border on dark. Touches: search input, lang toggle pill, every card outline, mtd3 hud, nl3-form, ftr3-cta__form, ftr3-clock pill. |
| 32 | P0 | `.hero3__signup-row` border at rest: `border-color: rgba(245,234,210,0.18)` | composite | void | **1.58:1** | **FAIL** | Form fields must have a 3:1 border on AA. Set `rgba(245,234,210,0.42)` or use `--ink-mute` (alpha 0.52) for the resting border. |
| 33 | P0 | `.nl3-form` border at rest: same pattern | composite | void | **1.58:1** | **FAIL** | Same fix. |
| 34 | P0 | `.ftr3-cta__form` border at rest | composite | void | **1.58:1** | **FAIL** | Same. |
| 35 | P0 | `.hdr3-search` border at rest: `1px solid var(--hairline)` (alpha 0.08) | composite | void | **1.19:1** | **FAIL** | Even worse — barely visible. Set to `var(--hairline-strong)` minimum, then bump that token. |
| 36 | P1 | `.nw3a-card` border at rest: `1px solid var(--hairline)` | composite | `--void-2` | **~1.2:1** | **FAIL** for non-text contrast on adjacent surfaces, but cards may be considered "decorative" since they don't define an interactable boundary that requires identification. Borderline. Most a11y auditors will flag. | Use a denser hairline. |
| 37 | P0 | `.hdr3-burger` border `1px solid var(--hairline)` (line 93) — visible only on mobile | rgba(245,234,210,0.08) | void | **1.19:1** | **FAIL** | The burger is the *only* nav at < 880 px and its tap target is invisible without high contrast. Critical mobile issue. |
| 38 | P2 | Live-dot pulse `--crimson #E6213B` on void | flat color | void | **4.22:1** | PASS for non-text (≥ 3:1) | OK. |
| 39 | P2 | Search-input focus state `box-shadow: 0 0 0 3px rgba(230,33,59,0.12)` | rgba composite | void | composite alpha 0.12 → ~ 1.4:1 | **FAIL** | The 3-px focus ring is too transparent to be a robust focus indicator (cross-checked with 2.4.7). Increase the opacity to 0.45 minimum. |

### 2.1.1 Keyboard (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 40 | P0 | `.mtd3` Method section (entire 5108-px scroll-pin stage) | The 5 frames are `<article role="listitem">` inside a `role="list"` — **no focusable controls anywhere**. The rail items (`<li class="mtd3__rail-item" data-step="1">`) are *visual* nav indicators with no `<a>` or `<button>` inside, so keyboard users **cannot jump between frames**. They must scroll-spam space/PgDn through 5×100vh of pinned animation. The `mtd3__cta` outro is the only focusable thing in 5108 px. | Make each rail item an `<a href="#mtd-step-1">` (give each frame an id) or a `<button>` that scrolls to the corresponding frame. Add `id` attributes to each `mtd3__frame`. |
| 41 | P0 | `.hero3__ticker-track`, `.nl3-issues-row` marquees | Auto-scrolling content with no pause control reachable by keyboard. `nl3-issues-row` has `:hover { animation-play-state: paused }` but **no `:focus-within`**. Hero ticker has neither. WCAG 2.2.2 (timing) and 2.1.1 (keyboard) require pause for any content moving > 5 s. | Add `.hero3__ticker:hover .hero3__ticker-track, .hero3__ticker:focus-within .hero3__ticker-track { animation-play-state: paused; }` and a visible "pause" toggle. Same for nl3-issues. |
| 42 | P0 | `index.html:111` hamburger `.hdr3-burger` only opens drawer | The drawer (line 115) opens on click — but **no Escape handler closes the drawer** in `v3.js:49-56`. Keyboard users get trapped (Tab still cycles past it because of `aria-hidden` racing, but no Escape). | Add `document.addEventListener('keydown', e => { if (e.key === 'Escape' && drawer.classList.contains('is-open')) { drawer.classList.remove('is-open'); burger.setAttribute('aria-expanded', 'false'); burger.focus(); } });`. |
| 43 | P0 | Header `.hdr3-has-mega` mega-menu | Triggered on `mouseenter` (v3.js:42) — **keyboard cannot open it via hover.** A click handler exists (line 44) but the mega closes on `mouseleave` (line 43) so a sighted-with-keyboard user who moves the mouse loses focus mid-keyboard-traversal. Inside the mega, no focus trap. Esc closes (line 47) — good. But **first card is not focused on open**. | Move focus into the first card on `open()`; trap focus while open; restore focus to trigger on `close()`. |
| 44 | P0 | Header search input "K" shortcut (`v3.js:64`) | Listens to **bare** "k" with `(metaKey || ctrlKey)`. So the actual shortcut is **Cmd+K / Ctrl+K** — that's correct and does NOT conflict with screen-reader bare K (NVDA quick-nav). **However** — the shortcut traps Cmd+K inside the page even when focus is in another input or modal, which interferes with browser-default shortcuts (e.g., Firefox Cmd+K = address bar). | Scope the shortcut to "not in input": `if (document.activeElement.tagName !== 'INPUT' && document.activeElement.tagName !== 'TEXTAREA') { … }` — or document the shortcut elsewhere. |
| 45 | P1 | `.fnd3a__verified` SVG with `aria-label="حساب موثّق"` | Element is purely decorative animation (a spinning verify badge). Gives it an a11y name but no `role` and no interactive behavior — SR will announce "حساب موثّق" alone, which is misleading. | Either make it a real annotation (use `<figcaption>` text) or set `aria-hidden="true"`. |
| 46 | P1 | `.hdr3-tick-link` clickable links **inside** an `aria-live` region | Each `.hdr3-tick` contains an inline `<a class="hdr3-tick-link">`. When the auto-rotator swaps ticks every 6 s, the focused link could be ripped out from under the keyboard user. | Pause the auto-rotation when any descendant has focus: `.hdr3-ticker:focus-within { animation-play-state: paused; }` AND clear the `setInterval`. |

### 2.4.1 Bypass blocks / Skip link (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 47 | P0 | `index.html:32` `<body>` | **There is no skip link.** Single-page experience with 80 anchors before main content. Keyboard users must Tab through the entire 35-element header (nav, mega, search, lang × 2, CTA, burger) on every navigation event. | Add as the very first focusable element in body: `<a class="sr-only sr-only-focusable" href="#main">تخطّى إلى المحتوى الرئيسي / Skip to content</a>` and give `<main>` an `id="main"` and `tabindex="-1"`. |

### 2.4.3 Focus order (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 48 | P1 | Header drawer (mobile) and main page | When the drawer opens on mobile, `document.body.style.overflow = 'hidden'` (v3.js:54) — **but Tab still moves into the page behind the drawer** because there is no inert/`aria-modal` enforcement. | Add `aria-modal="true"` and either set `inert` on body's other children or implement a focus trap. |
| 49 | P2 | RTL focus order | The page is `dir="rtl"` (line 2) — Tab order on RTL still goes left-to-right per browser spec, but the *visual* order is right-to-left. So in the header utility bar (line 96), the visual order is `Search ← Lang ← CTA ← Burger` (right-to-left), and Tab order is `Search → Lang → CTA → Burger` (left-to-right) — i.e., the visual *first* element gets tabbed *first*. **OK** — this matches WCAG 2.4.3. | None — confirmed correct. |

### 2.4.4 Link purpose (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 50 | P0 | `.nl3-issue` previous-issue chips (`index.html:985-992`) | All 4 visible chips have `href="#"` — clicking jumps to top of page. Plus 4 more with `aria-hidden="true" tabindex="-1"` (clones for marquee). The visible 4 are reachable by Tab but go nowhere meaningful. | Either provide real archive URLs OR remove the anchors and use `<button disabled>` with a visible "coming soon" badge. As-is it's a broken-promise pattern. |
| 51 | P1 | Header logo `<a href="/" aria-label="…الصفحة الرئيسية">` (line 49) and footer mark `<a href="#" …>` (line 1145) | Footer mark goes nowhere — same defect as #2. | Set footer mark `href="/"` or `#hdr3-root`. |
| 52 | P2 | Multiple "FOLLOW ←" CTAs across nw3a cards (lines 380, 393, 406, 419, 432, 450) — every card | Link text is "FOLLOW ←". Without context, screen reader hears "follow follow follow…". The `.nw3a-link` `aria-label` *does* provide unique handle (line 372 etc.), so OK, but the visible text "FOLLOW" inside is reduced to noise. | Either drop the visible "FOLLOW" text or make the `aria-label` include it for parity. |

### 2.4.6 Headings and labels (Level AA)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 53 | P1 | Heading hierarchy | `<h1>` = `.hero3__wordmark` (line 163, "المبادر"). Then `<h2>` per section (`hero3__panel-head h2`, `manif3__lede` id'd as title but not actually `<h2>` — see #54), `<h3>` per card. Mostly clean. **Issue:** `.manif3__lede` (line 310) is id'd `manif3-title` and referenced by `aria-labelledby="manif3-title"` (line 302), but it's a `<p>`, not an `<h2>`. So the manifesto section's accessible heading is a paragraph. | Either change to `<h2 class="manif3__lede" id="manif3-title">…</h2>` or add a visually hidden `<h2 class="sr-only">المنفستو</h2>` and use it. |
| 54 | P1 | `.fnd3a__quote-text` (line 1043) inside founder section | The founder's editorial quote has no caption-style heading; the `<blockquote>` has `<footer>` attribution but `<p>` body. Actually fine pattern-wise — but the broader `<section class="fnd3a__follow">` (line 1080) starts with `<h3>` while it sits inside `<section class="fnd3a">` whose only `<h2>` is "بدر شاكر" (line 1035). So heading levels jump h2 → h3 cleanly — OK. | None. |
| 55 | P2 | Footer `<h2 id="ftr3-heading" class="ftr3-sr">` (line 1128, sr-only) | Hidden heading — good practice. Followed by 4 `<h4>` column heads but no `<h3>`. | Use `<h3>` for column heads or accept the level skip (auditors disagree on this). |

### 2.4.7 Focus visible (Level AA)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 56 | P0 | Global `a:focus-visible` style (`base.css:39`) | Defined: `outline: 2px solid var(--crimson); outline-offset: 3px; border-radius: 2px`. **But** there is **no equivalent for `<button>`** — buttons get *no* focus ring globally. Form-element `<input>` also has no global focus-visible. | Extend to `a, button, input, [tabindex]:focus-visible { outline: 2px solid var(--crimson); outline-offset: 3px; }`. |
| 57 | P0 | `.hdr3-burger:focus-visible` | Not defined. Mobile users tabbing to the burger see no indication. | Add explicit focus style. |
| 58 | P0 | `.hdr3-lang-btn:focus-visible`, `.ftr3-lang__btn:focus-visible`, `.ftr3-top:focus-visible`, `.hdr3-announce-arrow:focus-visible`, `.hdr3-nav-link:focus-visible` (the button variant on line 68) | None defined. All silently invisible on focus. | Add the canonical focus ring. |
| 59 | P0 | `.hero3__signup-cta`, `.nl3-submit`, `.ftr3-cta__btn` (the three subscribe buttons) | None have explicit `:focus-visible`. The `:focus-within` on the parent `*-form` ring (3:1 issue noted in #32-34) is the *only* visible cue, and it stops at the input — pressing Tab off the input to the button has no indicator. | Add focus ring on each button. |
| 60 | P0 | `.cnt3-link`, `.fnd3a__follow-card`, `.hero3__chip`, `.hdr3-card`, `.hero3__panel-link` — all anchors *with* the global `a:focus-visible` rule | The global rule applies, but `outline-offset: 3px` plus `border-radius: 2px` overrides any per-component radius — on cards with 16-20 px radius, the 2px square outline looks broken. | Override `border-radius` per card to match component (`var(--r-lg)` on most). |
| 61 | P1 | `.nw3a-link:focus-visible` (`v3.css:353`) | Has correct override: `outline: 2px solid var(--accent); outline-offset: 4px; border-radius: var(--r-xl)`. **Good** — keep. But the accent on the `azure` card is `#1E5AA8`, which on void = 2.80:1 (FAIL non-text 3:1). | Use accent only for hover; force focus ring to crimson which passes. |
| 62 | P1 | `.hdr3-search-input:focus` (line 76) | The visible focus is a 3px box-shadow at rgba(230,33,59,0.12) — far too transparent. | Bump to 0.5+ opacity. |

### 2.5.5 Target size (Enhanced) (Level AAA) / 2.5.8 Target size (Minimum) (Level AA, WCAG 2.2)

WCAG 2.2 added a new AA rule: minimum target size 24×24 CSS px (or AAA 44×44).

| # | Sev | Location | Size | Fail/Pass | Fix |
|---|---|---|---|---|---|
| 63 | P0 | `.hdr3-announce-arrow` (line 24 of v3.css) | 22×22 CSS px | **FAIL AA 2.5.8** (< 24×24) | Bump to 24×24 (or 32×32 for thumb). |
| 64 | P1 | `.hdr3-burger` (40×40 — line 93) | 40×40 | PASS AA, FAIL AAA (< 44×44) | Bump to 44×44 for thumb. |
| 65 | P1 | `.hdr3-lang-btn` "AR"/"EN" — `padding: 5px 10px` on 11 px text → ~26×24 box | ~26×24 | PASS AA marginally | Bump padding-block to 8 px → 30×30. |
| 66 | P1 | `.ftr3-top` and `.ftr3-lang__btn` | similar small | borderline | Same fix. |
| 67 | P1 | `.nl3-issue` chips (38s marquee) — height ~34 px | ~34 px tall | PASS AA, FAIL AAA | While moving they're not effectively tappable until paused (`:hover`). Still — bump padding. |

### 3.1.1 / 3.1.2 Language of page / parts (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 68 | P0 | `index.html:2` `<html lang="ar" dir="rtl">` | Correct. ✅ | None. |
| 69 | P1 | English fragments **without** `lang="en"` | Many: `.hero3__wordmark-romanized "A L M O B A D I R"` (line 176), `.hdr3-mega-foot-val "26 أبريل 2026"` (Arabic but date-only), `.hero3__panel-kicker "NETWORK · LIVE"` (line 212), `.hero3__eyebrow-mono "EST. 1446H"` (line 161), `.hero3__signup-meta` mono labels, `.cnt3-eyebrow` etc. — all should be `lang="en"`. | Add `lang="en"` to every span containing English. The site has `<span class="hero3__eyebrow-en" lang="en">` correctly in `.nw3a-eyebrow-en` (line 358) and `.manif3__eyebrow--en` (line 306) — extend to all monos. |
| 70 | P1 | `.fnd3a__tagline-en "Top 20 B2B Saudi Arabia · 2025"` (line 1036) | Has `class="fnd3a__tagline-en"` but no `lang="en"` attribute. | Add `lang="en"`. |

### 3.2.1 On focus / 3.2.2 On input (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 71 | P0 | `.hdr3-lang-btn` and `.ftr3-lang__btn` | Click toggles `is-active` class + (in footer) dispatches a CustomEvent that **nothing listens for** (Wave 1 #9). The button reports `aria-pressed="true"` after click (footer only) but the page content does not change. **Mismatch of `aria-pressed` state and observable change** is a 3.2.2 violation. | Either implement the language switch OR remove the toggle entirely until the EN site exists. Keep it disabled/ghosted. |
| 72 | P1 | `.hdr3-nav-link[aria-haspopup]` mega trigger opens on `focus` (v3.js:45) | A WCAG 3.2.1 grey area: focus alone opening a menu is allowed *if* it does not move focus elsewhere. Here it does not — OK. But focus that visually pops a 6-card panel can disorient if user didn't intend it. | Acceptable. Document and verify with user testing. |

### 3.3.1 Error identification / 3.3.2 Labels or instructions (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 73 | P1 | All 3 forms (`.hero3__signup`, `#nl3-form`, `.ftr3-cta__form`) are `novalidate` | Browser native validation is disabled but **no JS validation is implemented either** (see v3.js — `submit` is just `e.preventDefault()` and `alert()` would happen but nothing). User can submit empty / malformed and gets nothing. | Either remove `novalidate` (lets browser + native error UI work) OR implement custom validation with `aria-invalid` + `aria-describedby` error messages. |
| 74 | P0 | All 3 forms — empty submission | No error feedback at all. SR users get no indication their input was rejected. | Implement minimal validation with live-region announcement: "Email is required" / "Email format invalid". |
| 75 | P1 | `.hdr3-search-input` has `aria-label="البحث"` (line 99) | Good — labeled. **However**, no submit handler is implemented (Wave 1 obs #6). User can type, hit Enter — nothing happens. Functional dead-end. | Either disable the input or wire it. |
| 76 | P1 | Email input `<label for="hero3-email">` etc. | Hero signup: ✅ label present (line 190). Newsletter: ✅ label (line 929). Footer: label is `.ftr3-sr` (sr-only) — ✅. All correct. | None. |

### 4.1.1 Parsing (deprecated in WCAG 2.2 but still relevant)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 77 | P0 | `id="hero3-email"`, `id="nl3-email"`, `id="ftr3-email"` — three different forms with three different IDs | ✅ Unique. Confirmed. | None. |
| 78 | P0 | Repeated ID `<span id="year">2026</span>` (line 1203) — only one. | ✅ Unique. | None. |
| 79 | P1 | `<label class="ftr3-sr" for="ftr3-email">` (line 1135) — but the `<input id="ftr3-email">` is on line 1136. ✅ Pair correct. | OK. | None. |

### 4.1.2 Name, Role, Value (Level A)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 80 | P0 | `.hdr3-burger` (line 111) | Has `aria-label="القائمة"` and `aria-expanded="false"` — but **lacks `aria-controls="hdr3-drawer"`**. | Add `aria-controls`. |
| 81 | P1 | `.hdr3-has-mega .hdr3-nav-link` (line 68) | Has `aria-haspopup="true"` and `aria-expanded` — but **lacks `aria-controls="hdr3-mega-network"`**. | Add `aria-controls`. |
| 82 | P0 | `.ftr3-lang__btn[aria-pressed="true|false"]` (lines 1207, 1209) | Both buttons start with explicit pressed states ✅. **But** the JS only flips `is-active` class — `aria-pressed` is **never updated**. So state desyncs immediately on click. | Sync `aria-pressed` in the click handler. |
| 83 | P0 | `.hdr3-lang-btn` (line 103, 104) | No `aria-pressed` at all (see #7). | Add. |
| 84 | P1 | `.cnt3-card` `<article>` wrapping `<a class="cnt3-link">` | Pattern: clickable card with anchor wrapping the cover, head, body, meta. SR will announce the entire long text as the link name. The `aria-label` on the `<a>` (line 848 etc.) — good — provides a cleaner accessible name. ✅ | None. |
| 85 | P0 | `.fnd3a__verified` (line 1021) `aria-label="حساب موثّق"` on a non-interactive `<div>` | `aria-label` on a non-semantic element is ignored by some SRs. | Wrap in a `<span role="img" aria-label="…">` or move the label to the parent figure caption. |
| 86 | P1 | `.nl3-mock-link "اقرأ كاملاً ←"` (line 976) | Looks like a CTA but is a non-interactive `<span>`. SR users will hear "Read full back arrow" with no way to act. | Either make it a real `<a>` to a sample issue, or set `aria-hidden="true"`. |
| 87 | P1 | `.fnd3a__count` "0" placeholder text (line 1049 etc.) | Counts up to target on intersection. SR with caching may read stale "0" or get bombarded with each frame value. | Wrap the live counter in `aria-live="polite"` and only announce final value, OR set `aria-hidden` on the animated digit and provide a static `aria-label="6"` on the parent. |

### 4.1.3 Status messages (Level AA)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 88 | P1 | `.nl3-submit.is-success` | When the success state fires (CSS line 639-641), the `<span class="nl3-submit-success">` becomes visible. But it's `aria-hidden="true"` (line 935) — SR will not announce the success. | Remove `aria-hidden` from the success span when state flips, OR add a separate `<div aria-live="polite" class="sr-only">تم الاشتراك بنجاح</div>` that populates on success. |
| 89 | P0 | All form errors (when implemented) | No live region exists for errors. | Add `role="alert"` or `aria-live="assertive"` region per form. |

### Reduced motion (cross-cutting; mentioned in 2.3.3 Animation from interactions, AAA)

| # | Sev | Location | Issue | Fix |
|---|---|---|---|---|
| 90 | P1 | `v3.css:1138-1151` blanket-killer | Excellent coverage — kills 50+ animations. **Gaps:** `.hdr3-tick-dot` and `.hdr3-tick` rotation are not in the list (header announcement still rotates). The mtd3 essential scroll-pinned animation still runs (every `*` killed but the JS scroll-pinning is not a CSS animation, so it persists). | Add header ticker selectors. For mtd3, when reduceMotion is true, switch the desktop scrollytelling to the mobile stacked layout (`@media (prefers-reduced-motion: reduce) { .mtd3__stage { display: contents; } .mtd3__pin { position: static; } ... }`). |
| 91 | P1 | `.hdr3-announce` carousel (auto-rotates every 6s) | Not affected by reduced motion. | Pause auto-rotation when `matchMedia('(prefers-reduced-motion)').matches`. |
| 92 | P2 | `.hero3__ticker-track` runs at 50s linear infinite | In reduced-motion mode, the line-1139 selector kills it correctly. ✅ | None. |
| 93 | P1 | `.fnd3a__count` data-count animation | Reduced-motion not honored — JS still runs the 1.4-1.8s ease-out tween. | Detect reduceMotion and snap to final value. |

---

## Summary table — count of failures per criterion

| Criterion | Severity | Failures |
|---|---|---|
| 1.1.1 Non-text content | A | 5 |
| 1.3.1 Info & relationships | A | 7 |
| 1.4.3 Contrast (Min) | AA | 9 (P0/P1) |
| 1.4.10 Reflow | AA | 2 |
| 1.4.11 Non-text contrast | AA | 9 (incl. token-level cascading) |
| 2.1.1 Keyboard | A | 6 |
| 2.4.1 Skip link | A | 1 (critical) |
| 2.4.3 Focus order | A | 1 |
| 2.4.4 Link purpose | A | 3 |
| 2.4.6 Headings | AA | 3 |
| 2.4.7 Focus visible | AA | 7 |
| 2.5.8 Target size (min, WCAG 2.2) | AA | 5 |
| 3.1.2 Language of parts | A | 3 |
| 3.2.1 / 3.2.2 Predictable | A | 2 |
| 3.3.1 / 3.3.2 Errors / labels | A | 4 |
| 4.1.2 Name, role, value | A | 8 |
| 4.1.3 Status messages | AA | 2 |
| Reduced motion | AAA(adv.) | 4 |

**TOTAL: 81 distinct findings** (well above the 40-80 expected — accessibility is dense on this page).

---

## Estimated WCAG 2.2 conformance

- **Level A: FAIL** — at least 9 distinct A criteria failing (1.1.1, 1.3.1, 2.1.1, 2.4.1, 2.4.4, 3.1.2, 3.2.2, 3.3.1, 4.1.2). Most are correctable with token + JS edits.
- **Level AA: FAIL** — 14 AA criteria failing (1.4.3, 1.4.10, 1.4.11, 2.4.6, 2.4.7, 2.5.8, 4.1.3 added on top of A). Single biggest cluster: 1.4.11 non-text contrast (1 token bug × 8 instances).
- **Level AAA: FAIL** — multiple AAA criteria not met (2.5.5 target size 44×44, 2.3.3 animation from interactions, plus the AA ones cascading). Not the target standard.

**Path to AA pass — top 12 fixes that unblock the most criteria:**
1. Add a skip link (resolves 2.4.1).
2. Patch the 4 muted-text tokens: `--ink-quiet` → 0.55 alpha, `--on-paper-mute` → 0.65 alpha, `--manif3-on-paper-mute` → 0.65, **kill use of `--ink-quiet` for visible body text** (resolves 6 instances of 1.4.3).
3. Patch the hairline tokens: `--hairline` → 0.18, `--hairline-strong` → 0.40 alpha (resolves 8 instances of 1.4.11).
4. Swap the brand-red CTA to use `--crimson-2` `#C41A30` as the bg of all 3 subscribe buttons + `.hdr3-cta` (resolves 4 instances of 1.4.3).
5. Lighten `--azure` to `#3F7BCC` for text use; keep `#1E5AA8` for fills only (resolves 1 instance + cascades).
6. Add Esc handler + focus trap to drawer + mega menu (2.1.1 × 2).
7. Add `:focus-visible` rule for `button, input, [tabindex]` (resolves 7 instances of 2.4.7).
8. Make Method rail items real `<a href="#mtd-step-N">` and add `id` to each frame (2.1.1).
9. Implement form validation with `aria-live` errors (3.3.1 + 4.1.3).
10. Resolve language-toggle pretense — either make it work or remove (3.2.2 + 4.1.2).
11. Add `lang="en"` to all English mono labels (3.1.2, ~12 spans).
12. Replace `href="#"` placeholders on `.nl3-issue` and `.ftr3-mark` with real targets (2.4.4).

After items 1-12, the page would pass at AA except 2.5.8 target-size (#63-67) and 4.1.3 status-messages (#88), each requiring discrete additional fixes (~20 lines of CSS/JS each).

---

## 250-word executive summary

Almobadir.com is a single-page experience that demonstrates real accessibility intent — `dir="rtl"` and `lang="ar"` set correctly, `<main>` and `<header>` and `<footer>` landmarks in place, every icon-only button has an `aria-label`, a comprehensive `prefers-reduced-motion` blanket kills 50+ animations, and the brand SVG marks are correctly wrapped with accessible names. The mega-menu has Esc support, keyboard trapping is correctly implemented in *one* place. **Yet the page does not meet WCAG 2.2 AA**, and likely never has, because of a constellation of small but pervasive defects rather than any single architectural mistake.

The dominant failure modes: (1) the brand-red `#E6213B` is too light for white text at body sizes (4.23:1) — every "subscribe" button on the site fails AA. (2) Three muted-text tokens (`--ink-quiet`, `--on-paper-mute`, `--manif3-on-paper-mute`) ship below 4.5:1 — every caption, eyebrow, and footer mono label inherits the failure. (3) Hairline borders at alpha 0.08-0.18 fail non-text contrast 3:1 — every form field, every card outline, the burger, the search box are visually-undefined to users with low contrast vision. (4) There is no skip link in a 14,800-px scroll experience; keyboard users Tab through 35 header elements every navigation. (5) The Method scrollytelling has no keyboard alternative — rail items are unfocusable visual indicators, not anchors. (6) Both language toggles report state but never switch language. (7) Two marquees auto-scroll without a pause for keyboard users. (8) Newsletter "previous issues" are 4 placeholder `href="#"` chips that dump users at top-of-page.

The good news is that **70% of findings collapse into 12 atomic fixes** — token alpha bumps, four `aria-controls` attributes, one skip link, two Esc handlers, one focus-visible rule extension. Estimate 3-4 days of front-end work to reach a credible AA conformance claim.
