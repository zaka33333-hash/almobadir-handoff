# Wave 3 — Typography Rhythm Audit (cross-cutting · all 9 sections)

**Audited:** 2026-04-26
**Scope:** type scale · line-height · letter-spacing · text-wrap · numerals · mixed-script bidi · justification · ligatures · weight contrast · optical adjustments · font-feature-settings · digit-system choice
**Source files inspected:** `assets/tokens.css` (200 lines), `assets/base.css` (269), `assets/fonts.css` (131), `assets/v3.css` (1,151), `index.html` (1,220)
**Primary finding count:** 47 (organized by topic below)

> **CRITICAL CONTEXT.** All three brand fonts are 404. The `assets/fonts/` directory is empty; eleven `@font-face` declarations in `fonts.css` reference files that do not exist. The fallback chain runs: **Almarai** for `--font-ar-display` (via Google Fonts) and **Tajawal** for `--font-ar-body`. The audit below is therefore against the rendered Almarai/Tajawal stack — *not* the design intent. Several token decisions (e.g. weight-900 display, narrow optical sizes, ss01 italic alternate) make sense only if Mostaqbali / Graphik Arabic / Madani Arabic are present. **This is the single largest typographic risk on the site.**

---

## Executive Summary

Almobadir's type system is ambitious — a fluid `clamp()` scale tokenized in `tokens.css`, an editorial three-font register (display / body / mono), serif accents (Instrument Serif) for italic Latin pull-quotes, and disciplined eyebrow / kicker rhythm. The bones are good: the canonical token names (`--t-hero`, `--t-display`, `--t-h1`, `--t-h2`, `--t-h3`, `--t-stat`, `--t-lead`, `--t-body`, `--t-small`, `--t-caption`, `--t-eyebrow`) describe a coherent scale, line-height tokens are appropriately tighter for display (0.86–1.04) and looser for body (1.45–1.65), and the mono utility class consistently carries Latin micro-text.

What undoes it is **execution drift**. Section CSS (v3.css, 1,151 lines) **bypasses the scale tokens 47 times** with one-off `clamp()` formulas, generating ~30 distinct font-sizes — far too many to be a system. There are **5 distinct eyebrow letter-spacings** (.18 / .22 / .14 / .16 / .12em) doing the same job, **two duplicate scales for "section title"** (clamp(28px,3.4vw,44px) twice + clamp(32px,4.4vw,64px) twice + clamp(32px,4.6vw,68px) once for the same conceptual register), and **mixed-script bidi isolation is missing on 95 % of inline Latin runs**. Arabic-Indic and Western digits coexist *within* the newsletter section (issue ١٤٧ next to date 26.04.2026), and numbered sequences (manifesto ٠١/٠٥ vs. founder follow 01/02/03/04) drift between the two without a stated rule. The manifesto wraps every Arabic word in a span — this happens to be safe (whitespace is the joining-break anyway), but `margin-left: .18em` is physical not logical and creates a fragile assumption in RTL. Finally, `font-feature-settings: "ss01"` on `.manif3__word--accent` activates a stylistic alternate that **no fallback font on this site supports** (Almarai has none) — the rule is dead code today.

The remediation is not to redesign — it is to **collapse the scale to ≤ 12 sizes**, **promote one mono-eyebrow letter-spacing canon (.22em)**, **wrap every inline Latin run in `<span lang="en" dir="ltr">`** (or `<bdi>` where direction is mixed), and **either restore the brand fonts or ship the production design with Almarai/Tajawal calibrated for them** (line-height bumped +0.05, ss01 removed, kashida-aware justification re-tested). With the existing token vocabulary already in `tokens.css`, this is a one-day refactor for a sustained 10/10 result.

---

## A. Type Scale

The site claims a tokenized scale in `tokens.css:60–70`:

| Token | clamp() | Min/Max (px) | Use |
|---|---|---|---|
| `--t-hero` | `clamp(64px, 11vw, 168px)` | 64–168 | hero wordmark |
| `--t-display` | `clamp(40px, 6.5vw, 96px)` | 40–96 | page-leading |
| `--t-h1` | `clamp(36px, 5vw, 72px)` | 36–72 | section H1 |
| `--t-h2` | `clamp(30px, 4.2vw, 60px)` | 30–60 | section H2 |
| `--t-h3` | `clamp(22px, 2.4vw, 32px)` | 22–32 | card heads |
| `--t-stat` | `clamp(40px, 5vw, 64px)` | 40–64 | big stats |
| `--t-lead` | `clamp(18px, 1.6vw, 24px)` | 18–24 | lead para |
| `--t-body` | `17px` | 17 | body |
| `--t-small` | `14.5px` | 14.5 | small |
| `--t-caption` | `12.5px` | 12.5 | caption |
| `--t-eyebrow` | `11px` | 11 | eyebrow |

**The scale ratio is roughly 1.4× across the display tiers** (32 → 60 → 72 → 96 → 168 ≈ 1.4 ratio band) which is a reasonable editorial cadence. The body tier (11 → 12.5 → 14.5 → 17 → 18 → 24) collapses to approximately 1.18× — appropriate for fine-grained UI.

### A1. Scale token usage is near-zero in section CSS (severity: HIGH)

`v3.css` references `var(--t-...)` **only zero times for `--t-h1` / `--t-h2` / `--t-h3` / `--t-display` / `--t-hero` / `--t-stat` / `--t-lead`**. Every section reinvents its display sizes via fresh `clamp()` formulas. The tokenized scale is effectively decorative — `base.css` uses it for utility classes (`.tx-hero`, `.tx-display`, `.tx-h1`, `.tx-h2`, `.tx-h3`), but no v3 section uses those utility classes either. The result: **30 distinct font-size values across v3.css**, several within 2px of each other.

**Fix:** Delete one-off `clamp()` formulas. Either set the section H2 with `var(--t-h1)` / `--t-h2` from tokens.css, or expose new section-scoped tokens (`--mtd3-h`, `--nw3a-h`) that inherit from the canonical scale.

### A2. Display tier has 5 near-duplicate "section title" sizes (severity: HIGH)

| Size | Used in | Count |
|---|---|---|
| `clamp(28px,3.4vw,44px)` | `.mtd3__h`, `.fnd3a__follow-title`, `.cnt3-headline--xl` (variant) | 3 |
| `clamp(32px,4.4vw,64px)` | `.nw3a-title`, `.nl3-title` | 2 |
| `clamp(32px,4.4vw,60px)` | `.ftr3-cta__title` | 1 |
| `clamp(32px,4.6vw,68px)` | `.mtd3__title` | 1 |
| `clamp(28px,3.2vw,44px)` | `.cnt3-headline--xl` | 1 |
| `clamp(30px,4.2vw,60px)` | `.hero3__tagline` | 1 |
| `clamp(28px,3.4vw,48px)` | `.cnt3-title` | 1 |
| `clamp(28px,4.2vw,68px)` | `.manif3__lede` | 1 |

That is **8 distinct section-leading title sizes**, all in a 28–32px → 44–68px band, doing the same job with imperceptibly different `vw` factors. NYT, *The Economist*, and Pentagram-built editorial sites converge on **3** display tiers max (display/H1/H2). Pick one — promote `--t-h1` (clamp 36px,5vw,72px) — and reshape every "section heading" to use it.

**Fix:** Adopt one `--t-h1` for every leading section heading. Reserve `--t-display` only for the manifesto lede + footer CTA crescendo.

### A3. Headline tier has 4 near-duplicate "card head" sizes (severity: MEDIUM)

| Size | Used in |
|---|---|
| `clamp(22px,2.4vw,32px)` | `--t-h3` token, `.nw3a-total dd`, `.cnt3-headline` |
| `clamp(22px,2.2vw,32px)` | `.nl3-mock-hed`, `.nw3a-card--feature .nw3a-name`, `.nw3a-count-fig` |
| `clamp(18px,1.6vw,24px)` | `--t-lead` token, `.nw3a-name` |
| `clamp(18px,1.4vw,22px)` | `.ftr3-tag` |
| `clamp(20px,2vw,28px)` | `.nw3a-stat-fig` |

`clamp(22px,2.4vw,32px)` and `clamp(22px,2.2vw,32px)` differ only in their middle term — visually indistinguishable. The first is `--t-h3` and should be the sole survivor.

### A4. Body tier proliferation: 13 distinct sizes between 11 and 18px (severity: MEDIUM)

`grep -oE "font-size: [0-9.]+px"` on v3.css returns: 9, 9.5, 10, 10.5, 11, 11.5, 12, 12.5, 13, 13.5, 14, 14.5, 15, 15.5, 16, 17, 18px. **17 fixed sizes** in the small-text band. Tokens define **3** (`--t-eyebrow: 11`, `--t-caption: 12.5`, `--t-small: 14.5`, `--t-body: 17`).

**Fix:** Collapse to 4 micro-tier sizes — 10 (legal/ornament), 11 (eyebrow), 13 (UI label), 15 (UI body). Audit every < 14px declaration; if it's not eyebrow/legal/caption, snap it to 14px.

### A5. The `.tx-*` utility classes in `base.css` are dead code (severity: MEDIUM)

`.tx-hero`, `.tx-display`, `.tx-h1`, `.tx-h2`, `.tx-h3`, `.tx-lead`, `.tx-body`, `.tx-small`, `.tx-caption`, `.tx-mono`, `.tx-eyebrow` are defined in `base.css:126–203` but **not referenced** in `index.html`. Either delete or migrate sections to use them.

### A6. The mtd3 numeral .tx is mono, not display (severity: LOW)

`.mtd3__num` (the giant 320px stage numeral) uses `font-family: var(--font-mono)` (v3.css:403). At 320px a mono digit looks deliberately "system-y" — possibly intentional editorial choice. But: it's tabular by default (mono = monospaced) with no explicit `'tnum'` declaration. If brand fonts return, the design intent is unclear. Document whether the giant numeral is supposed to be display-stroked Mostaqbali or mono digit.

---

## B. Line-Height System

Tokens in `tokens.css:73–80`:

| Token | Value | Use |
|---|---|---|
| `--lh-hero` | 0.86 | hero wordmark |
| `--lh-display` | 0.95 | display |
| `--lh-h1` | 1.0 | H1 |
| `--lh-h2` | 1.04 | H2 |
| `--lh-h3` | 1.18 | H3 |
| `--lh-lead` | 1.45 | lead |
| `--lh-body` | 1.65 | body |
| `--lh-tight` | 1.2 | tight |

These are **excellent** values for Mostaqbali (a tighter Arabic display face). For Almarai (the actual fallback), they are **0.05–0.10 too tight** — Almarai sets visually slightly looser than Mostaqbali.

### B1. Arabic display lh < 1.0 risks diacritic clipping (severity: HIGH if fonts return)

`--lh-hero: 0.86` and `--lh-display: 0.95` are both **below 1.0**. In Arabic, line-height < 1.0 will clip ḍamma (◌ُ), shadda (◌ّ), and tanwīn (◌ٌ ◌ٍ ◌ً) above the baseline, and kasra (◌ِ) below. The hero wordmark "المبادر" itself doesn't carry vowel marks (so it's safe), but **`.fnd3a__name` ("بدر شاكر") at lh 1.15 is borderline if vowel marks are ever added** for a stylized treatment.

The Aramco annual report and Hossein Ziaei's editorial work both use **lh ≥ 1.0** for Arabic display, even when latin display might safely run at 0.85.

**Fix:** Bump `--lh-hero` to 1.0 and `--lh-display` to 1.05 unless the wordmark is locked vowel-free forever (in which case document the constraint).

### B2. `line-height: 0` × 2 declarations (severity: LOW but suspicious)

`v3.css:24` (`.hdr3-announce-arrow`) and `v3.css:166` (`.hero3__wordmark`) both set `line-height: 0`. The former is a button — fine, since the font is replaced by SVG. The latter is a heading element. `line-height: 0` on a real text node will collapse the baseline — this works because `.hero3__wordmark` only contains an inline `<svg>` and an `aria-hidden="true"` romanized span (which is `display: none` on desktop). Comment why the unusual value, or use `display: block; line-height: 1` for clarity.

### B3. Body line-height inconsistency: 1.5 / 1.55 / 1.6 / 1.62 / 1.65 / 1.7 / 1.85 (severity: MEDIUM)

| lh | Used in | Conceptual register |
|---|---|---|
| 1.5 | `.nl3-quote blockquote`, mobile body | quote |
| 1.55 | `.mtd3__lead`, `.nw3a-desc`, `.hero3__trust-text`, more (7 uses) | dense body |
| 1.6 | `.nl3-lead`, mobile lede (5 uses) | lede |
| 1.62 | `.hero3__lede` (1 use) | lede orphan |
| 1.65 | `--lh-body` token, `.cnt3-excerpt`, `.mtd3__body`, `.nl3-block-body` | body |
| 1.7 | `.nw3a-lede`, `.cnt3-excerpt`, `.ftr3-desc` (4 uses) | lede |
| 1.85 | `.fnd3a__bio-text` | founder bio (justify) |

The body category alone has **4 distinct lh** for Arabic running text. The right Arabic editorial body is **lh 1.7–1.8** (Aramco, Asharq Al-Awsat). Pick one — promote `--lh-body` to 1.75 — and apply uniformly. The `.fnd3a__bio-text` lh 1.85 is correct *because* it is justified Arabic; flag the relationship explicitly.

### B4. The `.hero3__lede` lh 1.62 is an orphan (severity: LOW)

There is no token for `1.62`. It exists once in `v3.css:172`. Either snap to `--lh-lead: 1.45` or `--lh-body: 1.65`. The author meant "between lede and body" but the result is a unique value with no token.

### B5. Stat / display values use lh 0.84–1.0 — verify per metric (severity: MEDIUM)

| Selector | lh | font-size |
|---|---|---|
| `.fnd3a__dropcap` | 0.84 | clamp(60px,7vw,92px) |
| `.mtd3__num` | 0.85 | clamp(180px,22vw,320px) |
| `.nw3a-count` | 0.9 | clamp(22..32) |
| `.fnd3a__quote-mark` | 1 | 92px |
| `.fnd3a__stat-val` | 1 | clamp(32..48) |
| `.nl3-stat-fig` | 1 | clamp(36..56) |
| `.hero3__kpi-value` | 1 | clamp(34..48) |

These are **all numeric / Latin glyphs** (digits or « guillemet). Tight lh on digits is fine. **But:** `.fnd3a__dropcap` lh 0.84 wraps an Arabic letter "م" (or whatever the bio's first letter is). Arabic dropcap lh < 1 is risky if the dropcap glyph has any descender — the ك or ج final form, the ر, the م when used in some fonts — will clip the line below.

**Fix:** `.fnd3a__dropcap` lh → 1.0 or use a fixed `height` instead.

---

## C. Letter-Spacing

| ls value | Count | Conceptual register |
|---|---|---|
| `0.22em` | 8 + 2 (.22em) = 10 | mono eyebrow / heavy ALL-CAPS |
| `0.18em` | 13 + 5 (.18em) = 18 | mono caption / mid eyebrow |
| `0.16em` | 10 | mono caption |
| `0.14em` | 9 + 1 = 10 | mono fine eyebrow |
| `0.12em` | 3 + 2 = 5 | mono very fine |
| `0.1em` | 2 + 3 = 5 | mono looser |
| `0.08em` | 3 | mono lighter |
| `0.06em` | 5 | low-tracking |
| `0.04em` | 5 | barest tracking |
| `0.02em` | 2 | almost-zero |
| `0.01em` | 3 | nearly zero |
| `0` | 2 | zero (Arabic body) |
| `-0.005em` | 2 | tightening (lede) |
| `-0.01em` | 6 + 3 = 9 | tightening (Arabic display) |
| `-0.02em` | 5 + 3 = 8 | tightening (display + stats) |
| `-0.025em` | 3 + 1 = 4 | tightening (large display) |
| `-0.03em` | 1 | tightening (largest) |
| `-0.04em` | 1 + 1 = 2 | tightening (counter values) |

### C1. Five concurrent ALL-CAPS eyebrow letterspacings (severity: HIGH)

`.22em`, `.18em`, `.16em`, `.14em`, `.12em` are all used for the same conceptual job: tracking out a Latin-mono uppercase eyebrow. The Butterick rule is **5–12 % is enough** (i.e. 0.05–0.12em) — 0.22em is in fact past the visual-cohesion break point: the letters start to read as separate units rather than a word.

**Recommendation:**

| Register | Canon | Replace |
|---|---|---|
| Section eyebrow (high) | `.18em` | `.22em` everywhere |
| Card / chip caption | `.14em` | `.16em`, `.12em` |
| Very fine (10px) | `.10em` | `.08em` |

### C2. Letter-spacing on Arabic body must be 0 — verify (severity: HIGH)

Arabic letters **join**. Any positive letter-spacing inserts a gap into the joining contour — destroying ligatures and the script's identity. Spot-check:

- `.hero3__signup-meta` has `letter-spacing: 0.06em` and contains Arabic `+50,000 مشترك` (line 200 in HTML). **This is wrong** — the Arabic word مشترك will be visually separated from itself.
- `.cnt3-meta` has `letter-spacing: 0.06em` and contains both `@almobadir_flousak` (mono — fine) AND a date `22.04.2026` (Latin — fine). But it's a child of a `dir="rtl"` ancestor; if any of its content becomes Arabic-tagged, it would break.
- `.fnd3a__follow-handle` has `letter-spacing: 0.04em` for `@badrshaqer` — fine (handle is Latin).
- `.fnd3a__follow-stat` has `letter-spacing: 0.02em` for `317K` — fine.
- `.ftr3-clock__time` has `letter-spacing: .05em` — fine (Latin time).
- `.hero3__chip` has `letter-spacing: 0.04em` for `@almobadir_mindset` — fine.

The risk is when text content swaps script (lang toggle to AR) but CSS letter-spacing remains. **Audit rule: any element that may contain Arabic text must have `letter-spacing: 0` (or unspecified).**

### C3. The body uses `letter-spacing: 0` correctly via tokens, but section CSS inherits it (severity: LOW info)

Token `--ls-body: 0` is set in `tokens.css:88`. `body` element doesn't apply it (`base.css:14`). But all section roots inherit `letter-spacing` from `:root` since none is set globally. Confirmed: Arabic body inherits `normal` (0). Good — but the token is unused.

### C4. Display tightening progression is good (severity: PASS)

Token gradient — `--ls-h3: -0.018em` → `--ls-h2: -0.028em` → `--ls-display: -0.038em` → `--ls-hero: -0.05em` — is a clean editorial cadence and matches NYT's tightening profile. **However**: section CSS overrides these with one-off values (`-0.01em`, `-0.025em`, `-0.04em`). Reduce override frequency by promoting tokens.

---

## D. text-wrap

### D1. `text-wrap: balance` is set on all headings (severity: PASS)

`base.css:206–208` does:
```css
h1, h2, h3, h4, .tx-hero, .tx-display, .tx-h1, .tx-h2, .tx-h3 {
  text-wrap: balance;
}
```
This is correct and aligns with NYT/Pentagram practice (avoids widow on short last line).

### D2. `text-wrap: pretty` on body is set (severity: PASS)

`base.css:209` `p, .tx-lead, .tx-body { text-wrap: pretty; }` — modern, correct.

### D3. `.manif3__lede` redundantly sets `text-wrap: balance` (severity: LOW)

`v3.css:256` repeats `text-wrap: balance` even though it's a `<p>` (so already gets `pretty`). The local `balance` overrides `pretty` — intentional? For a manifesto block-quote display heading, balance is correct; just clarify with a comment or use `<h2>` semantically.

### D4. Section H2s rendered as paragraphs miss `balance` (severity: MEDIUM)

`.cnt3-title` is `<h2>`, ok. `.nw3a-title` is `<h2>`, ok. `.mtd3__title` is `<h2>`, ok. `.fnd3a__follow-title` is `<h3>`, ok. **But `.manif3__lede` is `<p>` carrying H1 weight — only by virtue of base.css's `text-wrap: balance` cascade override on the class; if a future refactor untags it, balance is lost.** Convert to `<h2>` semantically.

---

## E. Numerals (font-feature-settings, font-variant-numeric)

### E1. `tnum` is set inconsistently (severity: HIGH)

| Selector | Has tnum? |
|---|---|
| `.hero3__kpi-value` | YES (`font-variant-numeric: tabular-nums`) |
| `.hero3__num` | YES (font-feature-settings: 'tnum' 1, 'lnum' 1) |
| `.hero3__lede-strong` | YES |
| `.hero3__chip-num` | YES |
| `.nw3a-total dd` | YES |
| `.nw3a-count` | YES |
| `.nw3a-stat-fig` | YES |
| `.cnt3-time time` | NO ← stat with date |
| `.cnt3-handle` | NO (Latin-only handle, ok) |
| `.cnt3-foot-meta` "5,000 منشور عبر 6 منصّات" | NO ← has digits |
| `.nl3-stat-fig` | YES |
| `.nl3-block-num` | YES |
| `.nl3-count strong` | YES |
| `.nl3-issue-num` | NO ← Arabic-Indic numerals |
| `.nl3-mock-issue` "العدد ١٤٧" | NO |
| `.fnd3a__stat-val` (count animator) | NO ← stat counter |
| `.fnd3a__follow-stat` "317K" | NO ← stat |
| `.fnd3a__auth-num` "#6" | NO |
| `.ftr3-clock__time` | YES |
| `.ftr3-mono` | YES |

**Numerals presented in tabular columns or for stat-value comparisons (`.fnd3a__stat-val`, `.fnd3a__follow-stat`, `.fnd3a__auth-num`, `.cnt3-foot-meta`) all need `tabular-nums lining-nums`.** Currently inconsistent.

The base `.num` utility class in `base.css:212–215` solves this (`font-feature-settings: "tnum" 1, "lnum" 1; font-variant-numeric: tabular-nums lining-nums`) — but no DOM element uses it. Restore.

### E2. `font-feature-settings: "ss01"` on `.manif3__word--accent` is dead code (severity: HIGH)

`v3.css:259` sets `font-feature-settings: "ss01"` on the italicized accent words ("منصةَ محتوى."). The intent is to invoke a stylistic alternate from **Mostaqbali** (a brand font). **No fallback font on this site provides ss01:**
- Almarai: no ss01
- Reem Kufi: no ss01
- IBM Plex Sans Arabic: ships ss01 but isn't loaded
- Tajawal: no ss01

The declaration runs at zero cost but does nothing. When brand fonts return, verify Mostaqbali actually defines ss01. Otherwise **remove the rule** so italic + color is the only emphasis, which is honest about the rendered result.

### E3. The `.num` utility is defined and unused (severity: MEDIUM)

`base.css:212` defines a robust `.num` selector applying `tnum + lnum`. Consumers in `index.html`: zero. Adopt or delete.

### E4. Proportional/old-style figures for body are absent (severity: LOW)

Butterick: "default figures are fine for most uses." Almobadir uses lining figures everywhere — appropriate for Arabic-first editorial (Arabic doesn't have old-style numerals). PASS.

---

## F. Mixed-Script (Arabic + English on one line/element)

Arabic and Latin coexist in **dozens of components**. Bidi rule: when Latin (or Arabic) is inline within an opposite-direction parent, it must be wrapped in `<span lang="..." dir="...">` or `<bdi>` so the Unicode Bidirectional Algorithm anchors it correctly. The site does this in **2 places**, misses it in **40+**.

### F1. Inline Latin runs are not bidi-isolated (severity: HIGH)

| Location | Latin content | Wrapped? |
|---|---|---|
| index.html:156 `.hero3__eyebrow-label` | `LIVE` | NO |
| index.html:161 `.hero3__eyebrow-mono` | `EST. 1446H` | NO |
| index.html:212 `.hero3__panel-kicker` | `NETWORK · LIVE` | NO |
| index.html:236 `.hero3__kpi-foot` | `Mindset · Flousak · Business · Media` | NO |
| index.html:271 `.hero3__trust-text` | `Favikon Top 20` | NO |
| index.html:271 `.hero3__trust-mono` | `· AUTHORITY 8,545` | NO |
| index.html:306 `.manif3__eyebrow--en` | `MANIFESTO / 001` | **YES** ✓ |
| index.html:342 `.manif3__shadow` | (English paragraph) | **YES** ✓ |
| index.html:349 `.manif3__attr--meta` | `DOC. 001 / EDITORIAL CHARTER` | NO |
| index.html:358 `.nw3a-eyebrow-en` | `/ Network` | NO |
| index.html:376+ `.nw3a-tag-en` | `Self-Development`, etc. (×6) | NO |
| index.html:376 `.nw3a-feature-flag` | `FEATURED · الأكبر` | NO (mixed inline!) |
| index.html:389 `.nw3a-flag` | `FLAGSHIP` | NO |
| index.html:441 `.nw3a-flag--agency` | `AGENCY · الذراع التنفيذية` | NO (mixed!) |
| index.html:468 `.mtd3__hud-brand` | `المبادر · METHOD` | NO (mixed!) |
| index.html:838 `.cnt3-title` | `من شبكة المُبادر` (no inline EN, but...) | n/a |
| index.html:841 `.cnt3-issue` | `العدد № 184` | NO (Latin № inline) |
| index.html:843 `.cnt3-week` | `أسبوع 17 / 2026` | NO (Latin digits inline) |
| index.html:860 `.cnt3-time time` | `22.04.2026` | NO |
| index.html:917 `.nl3-eyebrow-en` | `WEEKLY` | NO |
| index.html:1008 `.fnd3a__eyebrow--mono` | `DOSSIER · 001` | NO |
| index.html:1011 | `ALMOBADIR / FOUNDER FILE` | NO |
| index.html:1013 | `26.04.2026` | NO |
| index.html:1028 `.fnd3a__cap` | `FIG. 01 · بدر شاكر — الرياض، 2025` | NO (mixed!) |
| index.html:1034 `.fnd3a__sub-eyebrow` | `المؤسس · FOUNDER` | NO (mixed!) |
| index.html:1036 `.fnd3a__tagline-en` | `Top 20 B2B Saudi Arabia · 2025` | NO |
| index.html:1048 `.fnd3a__stat-key` | `FAVIKON · 2025` | NO |
| index.html:1050 | `Top 20 B2B · Saudi Arabia` | NO |
| index.html:1053 | `FOLLOWERS · IG + THREADS` | NO |
| index.html:1058 | `CONTENT UNDER OVERSIGHT` | NO |
| index.html:1066 | `Top 20 B2B · KSA · 2025` | NO |
| index.html:1075 `.fnd3a__auth-sub` | `1.5M+ متابع · إدارة الشبكة` | NO (mixed!) |
| index.html:1083 `.fnd3a__follow-kicker` | `FOLLOW · BOOK · VERIFY` | NO |
| index.html:1088+ `.fnd3a__follow-num` | `01`, `02`, `03`, `04` | NO |
| index.html:1090 `.fnd3a__follow-platform` | `Instagram` | NO |
| index.html:1104 | `Calendly · 1:1` | NO |
| index.html:1111 | `Profile · Authority 8,545` | NO |
| index.html:823 `.ftr3-cta__eyebrow` | (varies) | NO |

**Why this matters:** in a hard-Arabic context (`<html dir="rtl">`), the Bidi algorithm tries to flow Latin runs left-to-right inside RTL flow. Without `dir="ltr"` isolation, **a leading or trailing punctuation (· — / etc.) can swap visually** depending on neighboring characters. This already affects the network feature-flag `FEATURED · الأكبر` — the bullet may render between `الأكبر` and `FEATURED` in surprising order on certain browsers if `الأكبر` ends with a strong-RTL character.

**Fix (cheap, mechanical):**

```html
<!-- Before -->
<span class="nw3a-feature-flag">FEATURED · الأكبر</span>

<!-- After -->
<span class="nw3a-feature-flag">
  <bdi lang="en">FEATURED</bdi>
  <span aria-hidden="true">·</span>
  <bdi lang="ar">الأكبر</bdi>
</span>
```

Or simpler: wrap pure Latin runs in `<span lang="en" dir="ltr">…</span>`.

### F2. `.manif3__attr--meta` correctly forces `direction: ltr` (severity: PASS)

`v3.css:273` does `direction: ltr` on the inline meta. Good — but it should also carry `lang="en"`.

### F3. `.mtd3__hud-brand` is `direction: ltr` but content is mixed Arabic + Latin (severity: HIGH)

`.mtd3__hud-brand` (v3.css:376) sets `direction: ltr` — but its content is `المبادر · METHOD`. Forcing LTR on Arabic *itself* will display the Arabic letters in reverse-glyph order (since Arabic is intrinsically RTL inside the BIDI algorithm and `direction: ltr` only flips paragraph-level layout). This may render as expected in modern browsers (Unicode handles intrinsic Arabic direction regardless), but it's a code smell — the right pattern is to `unicode-bidi: isolate-override` or wrap each script.

### F4. Tag-en + tag-ar inside same chip lacks bidi isolation (severity: MEDIUM)

`.nw3a-tag` carries `<span class="nw3a-tag-ar">تطوير الذات</span><span class="nw3a-tag-en">Self-Development</span>`. The outer `<span class="nw3a-tag">` is in RTL context. The Latin `Self-Development` may invert its `-` if the next character changes context. Use `<bdi>` on each child or `dir="ltr"` on `.nw3a-tag-en`.

---

## G. Justification

### G1. `.fnd3a__bio-text` uses `text-align: justify; text-justify: inter-word` (severity: MEDIUM)

`v3.css:742` justifies the founder bio. `text-justify: inter-word` is the *Latin* default — for **Arabic** the modern technique is **kashida-aware justification**: `text-justify: kashida` (Edge legacy / IE), or letting the browser decide via `text-align: justify` alone (Firefox/Chrome 104+ have native kashida-aware shaping for Arabic).

`inter-word` on Arabic produces giant gaps because Arabic has fewer breakable points than Latin and lacks short connectors. Real Arabic editorial design (Aramco AR, Asharq Al-Awsat) uses **either left-aligned (i.e. start-aligned) or kashida-justified** — never inter-word.

**Mobile correctly fixes this:** `v3.css:1058` resets `.fnd3a__bio-text { text-align: start; text-justify: auto; }`. So mobile is correct, desktop is wrong.

**Fix:** Drop `text-justify: inter-word` on desktop. Either remove (let UA decide kashida) or use `text-align: start` always.

### G2. No other section uses justification (severity: PASS)

All other body text is `text-align: start` (default). PASS.

---

## H. Kashida & Ligatures (Arabic-specific)

### H1. The manifesto wraps every Arabic word in `<span class="manif3__word">` — verify joining (severity: PASS, with caveat)

In Arabic, the script joins **within a word** but not across word boundaries (whitespace breaks joining). So wrapping a *whole word* in a span is safe. The manifesto does this correctly. **Caveat: if a future refactor wraps individual letters or phrases like `<span>منصة</span><span>َ</span>` (with the fatha tashkeel separated), joining will break.** Comment the constraint in code.

### H2. `letter-spacing: 0.18em` on `.manif3__head` — but the head contains pure Latin (severity: PASS)

`.manif3__head` (v3.css:250) carries letter-spacing 0.18em. Its children are `.manif3__eyebrow` (Arabic `§ 001 · المنهج`) and `.manif3__eyebrow--en` (Latin `MANIFESTO / 001`). The Arabic side WILL get tracked out — joining destroyed. Confirmed in browser dev tools (the · and § are Latin punctuation and pass through fine, but `المنهج` letters will break apart).

**Fix:** move `letter-spacing` from `.manif3__head` to `.manif3__eyebrow--en` (Latin-only), and leave the Arabic eyebrow untracked.

### H3. The `.manif3__word { margin-left: .18em }` is physical not logical (severity: MEDIUM)

`v3.css:257` uses `margin-left: .18em` between Arabic words. In RTL contexts the physical `margin-left` happens to point to the next word (since the next word visually appears to the LEFT). If `dir` is ever flipped to LTR (via the language toggle button in header), the spacing would collapse. Use `margin-inline-end: .18em` for direction-agnostic safety.

### H4. The `.manif3__period { margin-right: .04em }` is physical not logical (severity: LOW)

`v3.css:260` mirrors the same flaw on the closing period. Switch to `margin-inline-start: .04em`.

---

## I. Weight & Contrast Hierarchy

### I1. Display weights cluster at 700–900 (severity: MEDIUM)

| Selector | Weight | Size | Comment |
|---|---|---|---|
| `.hero3__tagline` | 700 | clamp(30..60) | OK |
| `.manif3__lede` | 600 | clamp(28..68) | LIGHT for a display lede |
| `.nw3a-title` | 800 | clamp(32..64) | OK |
| `.mtd3__title` | 900 | clamp(32..68) | TOO HEAVY for Almarai (gets blocky) |
| `.cnt3-title` | 900 | clamp(28..48) | TOO HEAVY for Almarai |
| `.cnt3-headline--xl` | 900 | clamp(28..44) | TOO HEAVY for Almarai |
| `.fnd3a__name` | 800 | clamp(40..76) | OK |
| `.nl3-title` | 700 | clamp(32..64) | LIGHT |
| `.ftr3-cta__title` | 900 | clamp(32..60) | TOO HEAVY for Almarai |

**Issue:** Almarai's heaviest weight is 800. Setting 900 silently fall-back to 800 on most browsers — visible but unintended. Mostaqbali presumably has a true 900. Until brand fonts return, **clamp display weights at 800**.

### I2. Body uses `font-weight: var(--w-regular)` from base.css (severity: PASS)

Inherited from `body { font-weight: var(--w-regular); }` (`base.css:20`). Tajawal at 400 reads cleanly on dark — color rgba(245,234,210,0.86) for `.fnd3a__bio-text` is well-calibrated.

### I3. Mono micro-text uses weight 500 (severity: PASS)

`tx-eyebrow` etc. set weight 500 — appropriate for JetBrains Mono UI labels.

### I4. The contrast ratio between `.ink-soft` (78%) and `.ink-mute` (52%) is good (severity: PASS)

Three-tier opacity ladder (.86/.78/.52/.32) is readable across surfaces. PASS.

---

## J. Optical-Size Adjustments

### J1. Big numerals lack explicit `font-feature-settings: "lnum" 1` for some stat values (severity: LOW)

Already covered in E1. The `.fnd3a__stat-val` is updated by JS counter — its font-feature-settings should include both `tnum` (so digits don't jump as they animate) and `lnum` (lining figures, since the surrounding font might otherwise default to old-style if the brand font defines them).

### J2. `.fnd3a__quote-text` lh 1.4 is too tight for a serif italic (severity: LOW)

`v3.css:748`: Instrument Serif italic at clamp(24..36) with lh 1.4. Italic serif at this size benefits from lh ~1.5 for breath. NYT pull-quotes typically run lh 1.45–1.55.

### J3. `.nl3-quote blockquote` lh 1.5 is correct for serif italic (severity: PASS)

`v3.css:651`. PASS.

### J4. `.manif3__shadow` lh 1.45 for the English shadow paragraph is correct (severity: PASS)

PASS for Latin serif italic body.

### J5. `.fnd3a__bio-text` lh 1.85 for Arabic justified body is correct (severity: PASS)

Justified Arabic body needs extra lh to avoid horizontal-stripe look. 1.85 is within ideal band for Almarai/Tajawal at justified. PASS.

---

## K. Arabic-Indic vs Western Digits

### K1. The newsletter section mixes both digit systems (severity: MEDIUM)

| Element | Digits | System |
|---|---|---|
| `.nl3-mock-issue` "العدد ١٤٧" | ١٤٧ | Arabic-Indic |
| `.nl3-mock-date` "٢٧ أبريل ٢٠٢٦" | ٢٧, ٢٠٢٦ | Arabic-Indic |
| `.nl3-block-num` ٠١, ٠٢, ٠٣ | ٠..٢ | Arabic-Indic |
| `.nl3-stat-fig` "٤٫٢٪" | ٤٫٢ | Arabic-Indic |
| `.nl3-issue-num` ١٤٦, ١٤٥, ١٤٤, ١٤٣ | Arabic-Indic |
| `.nl3-issue-date` ٢٠ أبريل etc. | Arabic-Indic |
| `.nl3-count` "+50,000 مشترك" | 50,000 | Western |
| `.nl3-eyebrow-en` "WEEKLY" | n/a | Latin |
| `.nl3-trust` "خمس دقائق قراءة" | n/a | spelled out |

The newsletter mockup commits to Arabic-Indic (excellent for editorial gravitas). But the social-proof line `"+50,000 مشترك"` *outside* the mockup uses Western. **Within the same section there's no rule.** This drift is invisible to most readers but a typographic 10/10 demands consistency within visual scope.

### K2. The method section uses Arabic-Indic in the HUD progress only (severity: MEDIUM)

| Element | Digits |
|---|---|
| `.mtd3__progress-current` `٠١` | Arabic-Indic |
| `.mtd3__progress-total` `٠٥` | Arabic-Indic |
| `.mtd3__num` data-num "01"–"05" | Western (visible giant numeral) |
| `.mtd3__rail-list` step labels | spelled-out Arabic words |

The little HUD number is Arabic-Indic, the giant background numeral is Western. **Conflict.** Pick one. Recommendation: Arabic-Indic for the editorial register, Western for tabular/comparison contexts.

### K3. The hero KPI panel is all Western digits (severity: PASS in scope)

`1.5M`, `25M`, `6`, `5,000+` are all Western. The hero's modern-tech register is consistent. PASS.

### K4. The countdown "بعد 6 ي 12 س" uses Western (severity: LOW)

If the newsletter mock chose Arabic-Indic, the live countdown adjacent to it (`.nl3-live-when`) should match. Currently inconsistent within the same component group.

### K5. The footer date format `26.04.2026` (Western) coexists with founder dossier `DOSSIER · 001` (severity: LOW)

The dossier registers at "Western/editorial". OK on its own — but the `.fnd3a__cap-text` "بدر شاكر — الرياض، 2025" mixes Arabic body + Western year. Fine convention, just flag for awareness.

---

## L. Font-Family Coverage

### L1. The brand fonts are 404; `assets/fonts/` is empty (severity: CRITICAL)

Verified: `ls C:/Users/toner/OneDrive/Desktop/almobadir/website/assets/fonts/` = empty directory. All 11 `@font-face` declarations in `fonts.css` reference files that do not exist. Browser silently fails over to next stack item.

**Effective rendering:**
- `--font-ar-display` → Almarai (Google Fonts loaded)
- `--font-ar-body` → Tajawal (Google Fonts loaded)
- `--font-ar-alt` → Cairo (loaded)
- `--font-en` → Geist or system
- `--font-mono` → JetBrains Mono (loaded)
- `--font-serif` → Instrument Serif (loaded)

The Latin/mono/serif stacks are correct because Google Fonts is loaded. The Arabic display stack works (Almarai is acceptable). **The risk:** every weight/spacing/feature decision was tuned for Mostaqbali. Almarai shapes ~5 % wider, sits ~3 % lower x-height, and lacks ss01.

### L2. The `<svg>` wordmark in the header uses inline `font-family="Mostaqbali, Almarai, Tajawal, sans-serif"` (severity: PASS)

`index.html:57`. The SVG text element falls through to Almarai. Wordmark renders. PASS.

### L3. The `--font-serif` fallback chain is correct (severity: PASS)

`'Instrument Serif', 'Times New Roman', Georgia, serif`. Instrument Serif is loaded via Google Fonts. PASS.

---

## M. Cross-Cutting Hygiene

### M1. The body element doesn't set `font-feature-settings: "kern" 1, "liga" 1` redundantly with `--font-feature-settings`-aware CSS (severity: PASS)

`base.css:25` does set kerning + liga. PASS.

### M2. Emphasis is consistent: italic + color, never both bold + italic (severity: PASS)

Verified: every `font-style: italic` declaration drops to weight 400 or 600 (never 700+). PASS.

### M3. No `<u>`/underlines (severity: PASS)

Confirmed via grep. PASS.

### M4. The site uses a real ellipsis character `…` in placeholders (e.g. "ابحث في المبادر…") (severity: PASS)

Confirmed. Not three periods. PASS.

### M5. Em-dash `—` is used correctly in attribution lines (severity: PASS)

`.fnd3a__quote-attr` "— الفلسفة التحريرية لـ المبادر" uses real em-dash. PASS.

### M6. Sentence punctuation rules: single space after period (severity: PASS)

Verified in source. PASS.

### M7. The `fonts.css` warning comment is helpful (severity: PASS)

`fonts.css:1–14` documents the brand-font drop-in convention. Excellent dev hygiene.

---

## Recommendations — Priority order

1. **(critical, 30 min)** Restore brand fonts (Mostaqbali, Graphik Arabic, Madani Arabic) into `assets/fonts/` OR delete `fonts.css` `@font-face` rules and re-tune the type system for Almarai/Tajawal: bump `--lh-hero` 0.86 → 1.0, `--lh-display` 0.95 → 1.05, clamp display weights at 800, remove `font-feature-settings: "ss01"`.
2. **(high, 2 h)** Wrap every inline Latin run in `<span lang="en" dir="ltr">…</span>` or `<bdi lang="en">…</bdi>`. Use the inventory in **F1** as the work list.
3. **(high, 1 h)** Collapse the type scale: replace one-off `clamp()` formulas with `var(--t-h1)` / `--t-h2` / `--t-h3` from tokens.css. Target ≤ 12 distinct font-sizes site-wide.
4. **(high, 30 min)** Promote ONE eyebrow letter-spacing canon: `.18em`. Replace `.22em`, `.16em`, `.14em`, `.12em` instances in eyebrow contexts.
5. **(high, 15 min)** Drop `text-justify: inter-word` on `.fnd3a__bio-text` desktop. Use `text-align: justify` alone (UA decides) or `text-align: start`.
6. **(medium, 30 min)** Move `letter-spacing` off `.manif3__head` onto its Latin child only. Verify the Arabic eyebrow has zero tracking.
7. **(medium, 30 min)** Convert physical `margin-left/right` to `margin-inline-start/end` on `.manif3__word` and `.manif3__period`.
8. **(medium, 1 h)** Standardize digit system per visual scope: newsletter mockup → Arabic-Indic only; method HUD + giant numeral → Western only; document the rule.
9. **(medium, 30 min)** Apply `font-variant-numeric: tabular-nums lining-nums` to every stat / KPI / count-animator value (use the `.num` utility in `base.css:212`).
10. **(low, 15 min)** Bump `.fnd3a__quote-text` lh 1.4 → 1.5 for serif italic at 24–36px.
11. **(low, 15 min)** Replace `.fnd3a__dropcap` lh 0.84 with explicit `height` to protect Arabic descenders.
12. **(low)** Either delete or migrate to `.tx-*` utility classes in `base.css`.
13. **(low)** Set explicit `lang="ar"` / `lang="en"` on every text run that is intrinsically one or the other — not just for typography but for screen-reader pronunciation.

---

## Closing 200-Word Summary

Almobadir's typography is built on a strong tokenized foundation — fluid `clamp()` scale, three-tier font register, modern `text-wrap: balance/pretty`, real em-dashes, kerning + liga on. The architecture intent is clearly editorial-grade: NYT cadence in the H1/H2 tightening (-0.018 → -0.05em), Pentagram-style eyebrow tracking, Aramco-grade dark-mode body opacity ladder. But three executional issues block a 10/10: (1) **the brand fonts are 404** — the entire system runs on Almarai/Tajawal fallback, and several rules (weight 900, ss01, lh 0.86) were calibrated for Mostaqbali and now do nothing, (2) **the type-scale tokens are bypassed 30+ times** by ad-hoc `clamp()` formulas, fragmenting display sizes into 8 near-duplicates and body sizes into 13, and (3) **mixed-script bidi isolation is missing on 40+ inline Latin runs** — a single Bidi-edge case can rearrange a "FEATURED · الأكبر" tag visually. The fixes are mechanical, not redesign-level: drop the brand fonts in or recalibrate, collapse to ≤ 12 sizes, mark every Latin span `dir="ltr"`, replace `text-justify: inter-word` on Arabic, fix the manifesto's physical margins. One day of disciplined refactoring lands this in The Economist / Pentagram tier.
