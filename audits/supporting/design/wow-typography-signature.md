# WOW Audit — Almobadir Typographic Signature

**Compiled:** 2026-04-26
**Author:** Typographic Signature agent
**Sources fueling this work:**
- `design-audit/wow-research-typography.md` (15 type-theater refs, primary fuel)
- `design-audit/wow-research-arabic.md` (20 Arabic-editorial refs)
- `design-audit/wow-research-motion.md` (REFs 14, 15, 11 inform tactility)
- `design-audit/wow-research-interaction.md` (REF 14 informs the variable-axis specimen pattern)
- `design-audit/03-typography-rhythm.md` (47 findings — what we are replacing)
- `website/index.html`, `website/assets/tokens.css`, `website/assets/v3.css` (current state)

---

## Executive summary

Almobadir's current typography is ambitious in tokens (`--t-hero`, `--lh-display`, eight letter-spacing tiers) and incoherent in execution. Per the rhythm audit: **30 distinct font-sizes**, **5 concurrent eyebrow letter-spacings**, **17 fixed sizes in the small-text band**, three-and-a-half mismatched numeral systems, **40+ Latin-inside-Arabic spans without bidi isolation**, dead `font-feature-settings: "ss01"`, and a brand-font 404 silently downgrading every weight-900 declaration to Almarai-800. There is no single recurring gesture you could screenshot and label "that's Almobadir."

The five signatures below replace that drift with a system designed around three constraints: (1) the brand IS bilingual — every gesture must serve Arabic-Latin pairing, never one-sidedly; (2) the brand fonts are 404 today and the system must work on the IBM Plex Sans Arabic Variable + IBM Plex Sans Variable + Recursive Variable + Roboto Flex chain (all free, all on Google Fonts) and continue to work when Mostaqbali / Graphik Arabic return; (3) every signature must legibly survive from 11px eyebrow to 240px wordmark with `prefers-reduced-motion` honored.

The five signatures are:

1. **The Bilingual Eyebrow** — one `الكلمة · WORD · ٠٠١` pattern, caps-aligned, separator `=· =`, recurring across **every** section eyebrow (8+ surfaces).
2. **The Architectural Drop-Cap** — RTL-correct `::first-letter` that works on `م ا ت ب ك` and Latin caps, paired with a 220px-wide marginalia gutter, on every long-form opener (4+ surfaces).
3. **The Wide-Numeral Stat Frame** — `Roboto Flex wdth: 151` Western numerals OR `Cairo Variable Black` Eastern Arabic, pinned to a `var(--font-mono)` data label, on every counter/stat (6+ surfaces).
4. **The Cream Pull-Quote with Crimson Em-Rule** — Instrument Serif italic over `--paper`, 1px crimson rule on inline-start, attribution in mono, on every quote-bearing section (4+ surfaces).
5. **The Cap-Aligned Bilingual Wordmark Pair** — IBM Plex Sans Arabic + IBM Plex Sans Latin sharing baseline grid via cap-height alignment (not x-height), on every dual-script title and chip (5+ surfaces).

The five gestures share one type system: **two variable-font families** (IBM Plex Sans Arabic Variable + Roboto Flex), **one mono** (JetBrains Mono Variable), **one editorial serif** (Instrument Serif), **one fluid scale on a 1.250 ratio**, and **one canonical eyebrow letter-spacing of 0.22em on Latin only** (never on Arabic).

Lines of CSS to replace in `v3.css`: ~210 (8 eyebrow blocks, 6 stat blocks, 5 wordmark/title blocks, 4 quote blocks, 1 dropcap block).

---

## SIGNATURE 1: The Bilingual Eyebrow

**The gesture**: A single eyebrow pattern that pairs the Arabic register-word (الشبكة, المنهج, المؤسس) with the Latin tag (NETWORK, METHOD, FOUNDER) and an optional Eastern Arabic numeral (٠٠١), separated by an em-rule-and-middle-dot composite (`= · =`). Arabic and Latin are both **caps-aligned to the same optical baseline** so the eyebrow reads as one typographic unit, not as a translation pair.

**Where it appears (8 surfaces)**:
- `.hero3__eyebrow` (Hero — `LIVE · بث مباشر · EST. 1446H`)
- `.manif3__eyebrow` + `.manif3__eyebrow--en` (Manifesto — `§ 001 · المنهج` / `MANIFESTO / 001`)
- `.nw3a-eyebrow` (Network — `الشبكة / Network`)
- `.mtd3__eyebrow` (Method — `المنهج · خمس خطوات`)
- `.cnt3-eyebrow` (Content — `أحدث المحتوى`)
- `.nl3-eyebrow` (Newsletter — `نشرة المبادر · WEEKLY`)
- `.fnd3a__sub-eyebrow` (Founder — `المؤسس · FOUNDER`)
- `.ftr3-cta__eyebrow` (Footer — `النشرة الأسبوعية`)

**Specifications**:
- font-family (Latin segment): `var(--font-mono)` → `'JetBrains Mono Variable', ui-monospace, monospace`
- font-family (Arabic segment): `var(--font-ar-body)` → `'IBM Plex Sans Arabic Variable', 'Tajawal', sans-serif`
- font-size: `var(--t-eyebrow)` (11px clamp), one canonical size — eliminates the current 5-eyebrow-size proliferation
- font-weight: Latin `var(--w-medium)` (500); Arabic `var(--w-semibold)` (600 — Arabic needs +1 step to optically match Latin caps weight)
- line-height: `1.15` (Arabic diacritic-safe; never below 1.0 for any Arabic eyebrow)
- letter-spacing: Latin `0.22em` (canon, replaces .18/.16/.14/.12 chaos); **Arabic `0` always** (audit C2 — positive tracking destroys Arabic joining)
- color: `var(--fg-mute)` for both segments; `var(--crimson)` for the LIVE-style status dot only
- font-feature-settings: Arabic `"ss01" 1, "calt" 1` (Plex Sans Arabic ss01 swaps to a more editorial alif; calt enables contextual joining); Latin `"tnum" 1, "ss01" 1` (Plex/JetBrains tabular figures + their stylistic-set-1 rounded quote)

**CSS pseudocode (utility class)**:

```css
/* Single eyebrow component — replaces 8 bespoke .__eyebrow rules in v3.css */
.eyebrow {
  display: inline-flex;
  align-items: baseline;          /* baseline = cap-height alignment, see Sig 5 */
  gap: 10px;
  font-size: var(--t-eyebrow);
  line-height: 1.15;
  text-transform: none;            /* Arabic has no caps; do not force-uppercase the parent */
  color: var(--fg-mute);
}
.eyebrow [lang="ar"], .eyebrow .eyebrow-ar {
  font-family: var(--font-ar-body);
  font-weight: var(--w-semibold);
  letter-spacing: 0;               /* CRITICAL — never tracked */
  font-feature-settings: "ss01" 1, "calt" 1;
  /* lift Arabic to share Latin caps-baseline (see Signature 5) */
  transform: translateY(-0.06em);
}
.eyebrow [lang="en"], .eyebrow .eyebrow-en, .eyebrow [data-mono] {
  font-family: var(--font-mono);
  font-weight: var(--w-medium);
  letter-spacing: 0.22em;          /* canon; do not invent a 6th value */
  text-transform: uppercase;
  font-variant-numeric: tabular-nums lining-nums;
  font-feature-settings: "tnum" 1, "ss01" 1;
}
.eyebrow .eyebrow-sep {
  color: var(--fg-mute);
  opacity: 0.5;
  /* the sep is content-neutral · or = and never participates in bidi flow */
  unicode-bidi: isolate;
}
.eyebrow .eyebrow-num {
  font-family: var(--font-mono);
  font-feature-settings: "tnum" 1, "lnum" 1;
  letter-spacing: 0.18em;          /* numerals get one notch tighter than letters */
  color: var(--fg);                /* numbers are content; lift contrast */
}
.eyebrow .eyebrow-status::before {
  content: "";
  display: inline-block;
  width: 7px; height: 7px; border-radius: 50%;
  background: var(--crimson);
  box-shadow: 0 0 0 4px rgba(230, 33, 59, .14);
  margin-inline-end: 8px;
  animation: eyebrow-pulse 2.4s var(--ease-glide) infinite;
}
@keyframes eyebrow-pulse { 50% { transform: scale(1.18); opacity: .65; } }
@media (prefers-reduced-motion: reduce) {
  .eyebrow .eyebrow-status::before { animation: none; }
}
```

```html
<!-- Canonical bilingual eyebrow markup -->
<span class="eyebrow">
  <span class="eyebrow-status" aria-hidden="true"></span>
  <span class="eyebrow-ar" lang="ar">الشبكة</span>
  <span class="eyebrow-sep" aria-hidden="true">·</span>
  <span class="eyebrow-en" lang="en" dir="ltr">Network</span>
  <span class="eyebrow-sep" aria-hidden="true">·</span>
  <span class="eyebrow-num" lang="en" dir="ltr">001</span>
</span>
```

**Reference**: Wave 1 — REF 15 (Typotheque + TPTQ Arabic, https://typotheque.com / https://tptq-arabic.com — bilingual specimen with caps-aligned baseline) and REF 12 (Are.na blog, https://www.are.na/blog — single-system rigor). Reinforced by Arabic-research REF 9 (Brownbook, https://brownbook.me/about/ — Arabic weighted ≥ Latin, never sub-citizen).

**Bilingual proof**: When `الشبكة · Network · 001` renders, the Arabic letters keep their joining contour (letter-spacing 0), the Latin caps are tracked at the canonical .22em, the eastern/western numeral choice is locked to context, and the cap-height of the Latin matches the optical top of the Arabic letterform — no script reads as a translation of the other. Try the same eyebrow in a `dir="ltr"` page (English-first toggle): the markup is identical, the visual order auto-flips, the Arabic still doesn't get tracked, the Latin still does. Same component, both directions, no retrofit.

**Effort to roll out**: ~4hr (delete 8 bespoke eyebrow blocks in v3.css ≈ 80 lines; add 1 component ≈ 35 lines; refactor 8 markup sites in index.html with `lang` + `dir` attributes; verify the Plex ss01 actually loads).

---

## SIGNATURE 2: The Architectural Drop-Cap

**The gesture**: Every long-form section opens with a single Arabic letter sized at 5–6 line-heights, **floated to the inline-start (right in RTL, left in LTR)**, with the body wrapping to its inline-end side. Treated as architecture, not decoration: it sits in a contrast color (`--crimson`), with a thin baseline rule beneath that runs to the column edge. A 220px right gutter holds optional marginalia (citation, side-note, bilingual annotation) in `--font-serif` italic at 13px — Stripe Press's "sidenote-as-canon" technique adapted for RTL. Works equally on Latin first-letters when language toggles.

**Where it appears (4 surfaces)**:
- `.manif3__lede` (Manifesto — opens with "نحن نبني..." → first letter ن)
- `.fnd3a__bio-text` (Founder bio — opens with "بدر..." → first letter ب)
- `.mtd3__body` (Method long-form copy per frame — first letter of each step description)
- `.cnt3-card--featured .cnt3-headline--xl` (Content featured article — first letter of the headline gets dropcap treatment)

**Specifications**:
- font-family: `var(--font-ar-display)` for Arabic first-letter, `var(--font-serif)` for Latin first-letter (Instrument Serif — the only Latin face on this site that matches Cairo Variable's optical weight at display sizes)
- font-size: `5.5em` (5.5 × the parent's line-height — gives a clean 5-line cap, optically aligned to the first line's cap-height)
- font-weight: `var(--w-black)` (900 in Cairo / Mostaqbali; falls back to 800 in Almarai per audit I1)
- line-height: `1.0` (NOT below 1.0 — audit B1: Arabic descenders on ج ر م will clip if line-height < 1; correctness over the Stripe Press 0.85)
- letter-spacing: `0` (Arabic letters that follow the dropcap should NOT join to it — the dropcap is a freestanding architectural object, intentional break, not a bug)
- color: `var(--crimson)` (matches the manifesto rule line, founder portrait gradient, network featured card — single accent across all dropcaps)
- font-feature-settings: `"ss01" 1` (Plex Sans Arabic ss01 swaps to a more architectural alif; for Cairo we get the ornamental contextual variant)
- padding-inline-end: `0.12em` (gives the dropcap breathing room from the body without using physical `margin-right`)
- padding-block-start: `0.05em` (optical lift — Arabic baselines sit higher than Latin x-height, so a small block-start padding aligns the dropcap top with the body's first line cap)

**CSS pseudocode (utility class)**:

```css
/* Architectural drop-cap — one rule, two scripts, both directions */
.editorial > p:first-of-type::first-letter,
.editorial > .editorial-lede::first-letter {
  float: inline-start;                       /* RTL-correct: right in dir=rtl, left in dir=ltr */
  font-family: var(--font-ar-display);
  font-weight: var(--w-black);
  font-size: 5.5em;
  line-height: 1.0;                          /* Arabic descender safety */
  color: var(--crimson);
  padding-inline-end: 0.12em;
  padding-block-start: 0.05em;
  font-feature-settings: "ss01" 1, "calt" 1;
  /* hairline rule under the cap, runs to column-edge */
  border-block-end: 1px solid var(--crimson);
  margin-block-end: 0.05em;
}
/* Latin override — when first-letter is a Latin character, swap to Instrument Serif */
:lang(en) .editorial > p:first-of-type::first-letter {
  font-family: var(--font-serif);
  font-style: normal;
  font-weight: var(--w-bold);                /* Instrument Serif tops out at 700 */
}
/* The marginalia gutter — Stripe-Press sidenote on long-form */
.editorial-with-marginalia {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 220px;
  gap: clamp(32px, 4vw, 64px);
}
.editorial-with-marginalia > aside.marginalia {
  font-family: var(--font-serif);
  font-style: italic;
  font-size: 13px;
  line-height: 1.55;
  color: var(--fg-mute);
  align-self: start;
  /* on RTL, gutter is to the LEFT of body (inline-end), markup unchanged */
}
@media (max-width: 800px) {
  .editorial-with-marginalia { grid-template-columns: 1fr; }
  .editorial-with-marginalia > aside.marginalia {
    font-size: 12.5px;
    border-inline-start: 2px solid var(--crimson);
    padding-inline-start: 14px;
    margin-block-start: 18px;
  }
  /* Mobile dropcap — keep it but cap at 4.5em so it fits a 360px column */
  .editorial > p:first-of-type::first-letter { font-size: 4.5em; }
}
@media (prefers-reduced-motion: reduce) {
  /* Static — no entrance animation. Dropcap is a layout, not a moment. */
}
```

```html
<!-- Manifesto with dropcap + marginalia -->
<section class="manif3 editorial editorial-with-marginalia" dir="rtl" lang="ar">
  <div>
    <span class="eyebrow"><!-- Signature 1 --></span>
    <p class="manif3__lede editorial-lede">
      نحن نبني شركات أصغر من مؤسسة، أكبر من شركة...
    </p>
  </div>
  <aside class="marginalia" lang="en" dir="ltr">
    <em>Editorial Charter, Doc 001 — drafted Riyadh, 2025.
    See also: Brownbook 12 (2008), Aramco Qafilah 2024.</em>
  </aside>
</section>
```

**Reference**: Wave 1 — REF 1 (Stripe Press *Poor Charlie's Almanack*, https://press.stripe.com/poor-charlies-almanack — illuminated chapter opener), REF 2 (Stripe Press *Working in Public*, https://press.stripe.com/working-in-public — sidenotes-as-canon), REF 11 (It's Nice That, https://www.itsnicethat.com — 2-color drop cap in display face vs body serif). Arabic anchor: REF 10 (Aramco *Qafilah Magazine* / Kameel Hawa, https://www.aramcolife.com/en/publications/qafilah-magazine — drop cap as architectural Arabic letter, RTL-native float).

**Bilingual proof**: The selector is `:lang(en) .editorial::first-letter` not `[dir=ltr]::first-letter` — when the toggle flips the page to English, every dropcap inherits the Latin face automatically. Float is `inline-start` not `right`/`left` — physically opposite sides in opposite directions, both correct without retrofit. The marginalia gutter sits in the inline-end column, so on Arabic pages it's on the visual left (correct), on English pages it's on the visual right (correct). Tested mentally: an article opening with "Almobadir" → Instrument Serif "A" floats left in LTR; the same article opening with "المبادر" → Cairo "م" floats right in RTL. Same class, no JavaScript.

**Effort to roll out**: ~5hr (1 component ≈ 50 lines CSS; mark up 4 markup sites with `class="editorial"` + optional aside; remove existing `.fnd3a__dropcap` block ≈ 12 lines; verify Cairo Variable + Instrument Serif both load before shipping; spot-check ج ر م descenders).

---

## SIGNATURE 3: The Wide-Numeral Stat Frame

**The gesture**: Every numeric stat on the site is rendered in **Roboto Flex at `wdth: 151` (the maximum width axis)** for Western digits or **Cairo Variable Black** for Eastern Arabic digits, pinned underneath an ALL-CAPS mono data-label tracked at `0.22em`. The stat itself runs `tabular-nums lining-nums` so digits don't jitter as counters animate. Apple WWDC's quiet swagger — same family stretched and compressed in one composition — adapted for the bilingual numeral system. **The site picks one numeral system per section and locks it** (audit K1, K2 — current state mixes both within `.nl3-*` and `.mtd3__*`).

**Where it appears (6 surfaces)**:
- `.hero3__kpi-value` (Hero KPI panel — `1.5M`, `25M`, `6`, `5,000+` — Western locked)
- `.nw3a-count` + `.nw3a-stat-fig` (Network reach counters — Western locked)
- `.mtd3__num` (Method giant background numerals `01`–`05` — promote to Eastern Arabic `٠١`–`٠٥` for editorial register; current state has Western giant + Eastern Arabic HUD = conflict)
- `.nl3-stat-fig` + `.nl3-block-num` + `.nl3-issue-num` (Newsletter — Eastern Arabic locked, matches issue `العدد ١٤٧`)
- `.fnd3a__stat-val` + `.fnd3a__follow-stat` + `.fnd3a__auth-num` (Founder dossier — Western locked, animated counters)
- `.ftr3-clock__time` (Footer live clock — Western, tabular)

**Specifications**:
- font-family (Western digits): `'Roboto Flex', var(--font-mono)` — Roboto Flex has a `wdth` axis from 25 to 151
- font-family (Eastern Arabic digits): `var(--font-ar-display)` → Cairo Variable Black falls back gracefully to Almarai
- font-size: `var(--t-stat)` (clamp 40px → 64px) for body stats; `clamp(140px, 18vw, 240px)` for hero/method giant numerals
- font-weight: Roboto Flex `var(--w-semibold)` (590 — the WWDC sweet-spot weight at maximum width); Cairo `var(--w-black)` (900 → falls back to 800 Almarai per audit)
- line-height: `0.95` for inline stats (audit B5 caveat: Latin digits only — never apply to mixed-script wrapping); `0.85` for giant numerals (digits-only, no diacritic risk)
- letter-spacing: `-0.04em` for giant Western numerals at 240px (NYT-tight); `-0.02em` at 64px; `0` for Eastern Arabic numerals (Cairo's natural width is correct as-drawn)
- color: `var(--crimson)` for stat figures (matches the dropcap accent — single accent color across the system); `var(--fg)` only when the stat is content-side (e.g. KPI value)
- font-feature-settings: Western `"tnum" 1, "lnum" 1, "ss01" 1`; Eastern Arabic `"tnum" 1, "ss01" 1` — Cairo's ss01 gives the editorial Eastern-Arabic numeral cut, distinct from the default Naskh form
- font-variation-settings: Roboto Flex `"wght" 590, "wdth" 151, "opsz" 144`; Cairo `"wght" 900`

**CSS pseudocode (utility class)**:

```css
/* Stat — the figure */
.stat-fig {
  display: inline-flex;
  align-items: baseline;
  font-variant-numeric: tabular-nums lining-nums;
  font-feature-settings: "tnum" 1, "lnum" 1, "ss01" 1;
  line-height: 0.95;
  letter-spacing: -0.02em;
  color: var(--crimson);
}
.stat-fig[data-script="latn"] {
  font-family: 'Roboto Flex', var(--font-mono);
  font-variation-settings: "wght" 590, "wdth" 151, "opsz" 144;
  font-size: var(--t-stat);
}
.stat-fig[data-script="arab"] {
  font-family: var(--font-ar-display);
  font-variation-settings: "wght" 900;
  font-size: var(--t-stat);
  letter-spacing: 0;               /* Cairo Eastern Arabic — no negative tracking */
}
/* Giant — for Method numeral, hero KPI, manifesto figures */
.stat-fig--giant {
  font-size: clamp(140px, 18vw, 240px);
  line-height: 0.85;
  letter-spacing: -0.04em;
}
.stat-fig--giant[data-script="arab"] { letter-spacing: -0.02em; }

/* Stat unit — % SAR M K BN — runs at half-size in same family */
.stat-unit {
  font-size: 0.5em;
  font-weight: var(--w-medium);
  margin-inline-end: 3px;
  opacity: 0.85;
  letter-spacing: 0;
}
/* Stat label — mono, ALL CAPS, .22em tracked, sits BELOW the figure */
.stat-label {
  display: block;
  font-family: var(--font-mono);
  font-size: var(--t-eyebrow);
  font-weight: var(--w-medium);
  letter-spacing: 0.22em;          /* canon, matches .eyebrow */
  text-transform: uppercase;
  color: var(--fg-mute);
  margin-block-start: 6px;
  font-feature-settings: "tnum" 1;
}

/* Stat block — figure + label as a unit */
.stat {
  display: inline-flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 0;
}

@media (prefers-reduced-motion: reduce) {
  /* Counters animate to final value instantly when reduced */
  .stat-fig[data-counter] { transition: none; }
}
```

```html
<!-- Hero KPI: Western digits -->
<div class="stat">
  <span class="stat-fig" data-script="latn">
    1.5<span class="stat-unit">M</span>
  </span>
  <span class="stat-label" lang="en" dir="ltr">Followers · IG + Threads</span>
</div>

<!-- Newsletter: Eastern Arabic digits -->
<div class="stat">
  <span class="stat-fig" data-script="arab">
    ٤٫٢<span class="stat-unit">٪</span>
  </span>
  <span class="stat-label" lang="en" dir="ltr">Open rate · Q1 2026</span>
</div>

<!-- Method giant numeral: editorial Eastern Arabic -->
<span class="stat-fig stat-fig--giant" data-script="arab">٠١</span>
```

**Reference**: Wave 1 — REF 7 (Apple WWDC + SF Wide, https://developer.apple.com/wwdc/ + https://developer.apple.com/fonts/ — wide hero numeral / compressed eyebrow, same family). Arabic anchor: REF 1 (Aramco Annual Report 2024, https://www.aramco.com/-/media/publications/corporate-reports/annual-reports/saudi-aramco-ara-2024-english.pdf — single oversized financial figure as full-bleed hero) and REF 16 (Eastern-Arabic numeral as graphic — Aramco / Vision 2030 covers).

**Bilingual proof**: The `[data-script]` attribute drives font-family and `font-variation-settings`. Same class, two scripts. The stat-label below stays in mono Latin always (it's the unit register — `FOLLOWERS`, `OPEN RATE`) and never participates in the Arabic numeral area. When `1.5M` and `٤٫٢٪` appear on the same page (e.g. Hero western, Newsletter eastern), they share line-height, weight optical balance, and the `--crimson` accent — the eye reads them as members of the same family even though they are different scripts. The site-wide rule "one numeral system per section, locked" replaces the audit's documented K1/K2 drift.

**Effort to roll out**: ~6hr (delete 12 stat-block CSS rules in v3.css ≈ 95 lines; add 1 stat component ≈ 60 lines; load Roboto Flex variable from Google Fonts via `fonts.css` ≈ 4 lines; refactor 6 markup sites to use `data-script` attribute; lock per-section script choice in editorial guidelines).

---

## SIGNATURE 4: The Cream Pull-Quote with Crimson Em-Rule

**The gesture**: Every quote on the site sits inside a `--paper` cream slab, set in **Instrument Serif italic** at lede size (clamp 22px → 36px), with a **1px crimson rule on the inline-start** running floor-to-ceiling. The attribution beneath the quote is mono caps, `0.22em` tracked, in `--fg-mute` — same canon as eyebrow. When the quote is bilingual (Arabic body + Latin transliteration), both share the cream background and the same rule, but the Arabic uses Cairo Variable Light (300) for editorial italic-equivalent register since Arabic has no italic axis. This is the brand's "we publish" signal — wherever a quote appears, the page becomes a magazine page.

**Where it appears (4 surfaces)**:
- `.fnd3a__quote-text` (Founder pull-quote — currently set in serif italic, missing rule + cream)
- `.nl3-quote blockquote` (Newsletter testimonial — currently lh 1.5 serif, no rule)
- `.manif3__shadow` (Manifesto English shadow paragraph — currently has crimson border-inline-start ✓, but lacks cream slab)
- `.cnt3-card--featured .cnt3-excerpt` (Content featured article excerpt — currently no quote treatment, promote it)

**Specifications**:
- font-family (Latin): `var(--font-serif)` → `'Instrument Serif', 'Times New Roman', Georgia, serif`
- font-family (Arabic): `var(--font-ar-display)` at weight 300 (Cairo Variable Light — Arabic has no italic axis, so we use the lightest weight as the "italic-equivalent" editorial register)
- font-size: `clamp(22px, 2.4vw, 36px)` (Latin); same for Arabic — Arabic running quotes don't need the +10% bump that body Arabic does, since Cairo Light at this size has identical optical weight to Instrument Serif italic
- font-weight: Latin `var(--w-regular)` (400, italic); Arabic `var(--w-light)` (300, regular)
- line-height: `1.5` Latin (audit J3 — correct for serif italic); `1.7` Arabic (Arabic running quotes need looser leading than Latin even at light weight)
- letter-spacing: `-0.005em` Latin (slight tightening, audit ls-lead); `0` Arabic (always)
- color: `var(--on-paper)` (deep ink-on-cream — quotes are content, not micro-text)
- font-feature-settings: Latin `"liga" 1, "dlig" 1, "calt" 1` (Instrument Serif's discretionary ligatures `ct, st`); Arabic `"calt" 1, "ss01" 1`
- background: `var(--paper)` (cream slab — even when nested inside a `--void` section, the quote becomes an island)
- border-inline-start: `1px solid var(--crimson)` (full-height; matches the rule under dropcaps and the manifesto rule)
- padding: `clamp(28px, 4vw, 56px)` block, `clamp(24px, 3vw, 40px)` inline (cream needs breathing room)
- max-inline-size: `42ch` (quote measure — tighter than body's 65ch, audit-aligned)

**CSS pseudocode (utility class)**:

```css
/* Pull-quote — cream slab + crimson em-rule + serif italic */
.pull-quote {
  display: block;
  margin: clamp(32px, 4vw, 64px) 0;
  padding: clamp(28px, 4vw, 56px) clamp(24px, 3vw, 40px);
  background: var(--paper);
  color: var(--on-paper);
  border-inline-start: 1px solid var(--crimson);
  max-inline-size: 42ch;
  font-family: var(--font-serif);
  font-style: italic;
  font-weight: var(--w-regular);
  font-size: clamp(22px, 2.4vw, 36px);
  line-height: 1.5;
  letter-spacing: -0.005em;
  font-feature-settings: "liga" 1, "dlig" 1, "calt" 1;
  text-wrap: pretty;
}
.pull-quote[lang="ar"], .pull-quote :lang(ar) {
  font-family: var(--font-ar-display);
  font-style: normal;                /* Arabic has no italic axis */
  font-weight: var(--w-light);       /* Cairo 300 = "italic equivalent" register */
  line-height: 1.7;
  letter-spacing: 0;
  font-feature-settings: "calt" 1, "ss01" 1;
}
/* Attribution — mono caps, 0.22em, same canon as eyebrow */
.pull-quote-attr {
  display: block;
  margin-block-start: clamp(20px, 2.4vw, 32px);
  font-family: var(--font-mono);
  font-style: normal;
  font-size: var(--t-eyebrow);
  font-weight: var(--w-medium);
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--on-paper-mute);
  font-feature-settings: "tnum" 1;
}
.pull-quote-attr-name {
  color: var(--on-paper);
  font-weight: var(--w-semibold);
}
.pull-quote-attr-sep {
  margin-inline: 6px;
  opacity: 0.5;
}
/* When the quote is dropped onto a dark section, the cream slab breaks the field */
.slab-dark .pull-quote { /* no override needed — cream is intentional break */ }
@media (prefers-reduced-motion: reduce) {
  /* No entrance animation. The slab is a static editorial frame. */
}
```

```html
<!-- Founder bilingual quote -->
<figure class="pull-quote" lang="ar" dir="rtl">
  <p>المبادرة لا تعني الجرأة. تعني الاستعداد لرؤية ما لم يره الآخرون بعد.</p>
  <p lang="en" dir="ltr"><em>Initiative isn't about courage. It's about being ready to see what others haven't seen yet.</em></p>
  <span class="pull-quote-attr">
    <span class="pull-quote-attr-name" lang="en">Badr Shaker</span>
    <span class="pull-quote-attr-sep" aria-hidden="true">/</span>
    <span lang="en" dir="ltr">Founder</span>
    <span class="pull-quote-attr-sep" aria-hidden="true">·</span>
    <span lang="en" dir="ltr">Riyadh, 2025</span>
  </span>
</figure>
```

**Reference**: Wave 1 — REF 11 (It's Nice That, https://www.itsnicethat.com — pull-quotes break the column at full-bleed, contrast through scale + color not family) and REF 6 (29LT Zarid Serif Slant + IBM Plex Serif, https://v-fonts.com/fonts/twenty-nine-lt-zarid-serif — bilingual quote with both scripts leaning the same direction). Arabic anchor: REF 12 (Bahia Shehab × Cairo Review, https://www.thecairoreview.com/midan/the-lam-alif-artist/ — letter-as-portrait subject, art-catalog gravitas in news context).

**Bilingual proof**: The class targets `lang="ar"` first then `:lang(ar)` for descendants, so a quote markup that contains both scripts gets correct font-family per child without forcing the parent into one direction. Cairo Variable's weight 300 reads as the Arabic equivalent of Latin italic — a softer, more reflective register — without requiring a font that doesn't have an italic axis (no Arabic font does). The `--crimson` border-inline-start is on whichever side is the inline-start: physically right for Arabic, physically left for Latin, both correct without `dir`-specific overrides. Attribution stays mono Latin always (it's the metadata register, not the content register).

**Effort to roll out**: ~3hr (1 component ≈ 50 lines CSS; refactor 4 markup sites; remove `.fnd3a__quote-text`, `.nl3-quote blockquote`, partial `.manif3__shadow` overrides ≈ 25 lines; add `--paper` cream slab to founder section's existing dark surface).

---

## SIGNATURE 5: The Cap-Aligned Bilingual Wordmark Pair

**The gesture**: Every place where the brand says its own name in two scripts (`المبادر / Almobadir`, `المنهج / Method`, `الشبكة / Network`, `المؤسس / Founder`, `النشرة / Newsletter`) renders both scripts on a **shared cap-height baseline**, not a shared x-height baseline. Arabic letters (which sit higher and have looser line-height) are lifted by `transform: translateY(-0.08em)` so their tops align with Latin caps — Typotheque/IBM Plex's bilingual rigor. Both scripts use the **same weight** (semibold 600 for chips, black 900 for hero), the same color, and a thin separator slash that participates in `unicode-bidi: isolate`. The result: the script pair reads as one typographic unit, not "Arabic + its English translation."

**Where it appears (5 surfaces)**:
- `.hero3__wordmark` (Hero — `المبادر` SVG + romanized `A L M O B A D I R` underlay)
- Header brand block (`.hdr3-card-meta` adjacent — the masthead pair)
- `.nw3a-eyebrow-ar` + `.nw3a-eyebrow-en` (Network — `الشبكة / Network`)
- `.fnd3a__sub-eyebrow` first/last (`المؤسس · FOUNDER`)
- `.nl3-eyebrow-ar` + `.nl3-eyebrow-en` (Newsletter — `نشرة المبادر · WEEKLY`)

**Specifications** (chip-scale; hero scales the same rules up):
- font-family (Arabic): `var(--font-ar-body)` → `'IBM Plex Sans Arabic Variable'` (designed by Khaled Hosny + Bold Monday, metric-paired with Plex Latin)
- font-family (Latin): `'IBM Plex Sans Variable', var(--font-en)` (same designer team, same metrics — REF 15 anchor)
- font-size: chips `clamp(14px, 1.4vw, 17px)` (matches `--t-small`); hero wordmark `clamp(64px, 11vw, 168px)` (current `--t-hero`)
- font-weight: chips `var(--w-semibold)` (600); hero `var(--w-black)` (900 → 800 Almarai fallback per audit I1)
- line-height: Arabic `1.45` per child; Latin `1.15` per child — DIFFERENT per script, baseline is what we align (audit B-table-aware)
- letter-spacing: Arabic `0`; Latin `0.005em` (Plex Latin tracks slightly tight; audit-suggested compensation)
- color: `var(--fg)` (single color — the script pair is one unit, not a bilingual contrast)
- font-feature-settings: Arabic `"calt" 1, "ss01" 1`; Latin `"ss01" 1, "tnum" 1`
- font-variation-settings: chips `"wght" 600`; hero `"wght" 900`
- transform on Arabic child: `translateY(-0.08em)` — lifts the Arabic glyph so its top aligns with Latin cap-height (NOT x-height)
- separator: `unicode-bidi: isolate` plus `opacity: 0.5` plus `margin-inline: 6px` — bidi-safe per audit F1

**CSS pseudocode (utility class)**:

```css
/* Bilingual word pair — cap-height aligned, NOT x-height aligned */
.wordpair {
  display: inline-flex;
  align-items: baseline;            /* Arabic translateY does the cap-alignment work */
  gap: 6px;
  color: var(--fg);
}
.wordpair > [lang="ar"], .wordpair-ar {
  font-family: var(--font-ar-body);
  font-variation-settings: "wght" 600;
  line-height: 1.45;
  letter-spacing: 0;
  font-feature-settings: "calt" 1, "ss01" 1;
  /* Lift Arabic by 0.08em so its TOP aligns with Latin cap-height,
     not x-height. This is the entire bilingual signature, in one line. */
  transform: translateY(-0.08em);
}
.wordpair > [lang="en"], .wordpair-en {
  font-family: 'IBM Plex Sans Variable', var(--font-en);
  font-variation-settings: "wght" 600;
  line-height: 1.15;
  letter-spacing: 0.005em;
  font-feature-settings: "ss01" 1, "tnum" 1;
}
.wordpair-sep {
  font-family: var(--font-mono);
  opacity: 0.5;
  unicode-bidi: isolate;            /* audit F1 fix — separator never flows */
  margin-inline: 6px;
  color: var(--fg-mute);
}
/* Hero wordmark — same rules, scaled and weight-promoted */
.wordpair--hero > [lang="ar"], .wordpair--hero .wordpair-ar {
  font-size: clamp(64px, 11vw, 168px);
  line-height: 1.0;                  /* Audit B1: never below 1.0 for Arabic display */
  font-variation-settings: "wght" 900;
  letter-spacing: -0.04em;            /* tightening for display only */
  transform: translateY(-0.05em);     /* less lift needed at hero scale */
}
.wordpair--hero > [lang="en"], .wordpair--hero .wordpair-en {
  font-size: clamp(64px, 11vw, 168px);
  line-height: 0.95;
  font-variation-settings: "wght" 900;
  letter-spacing: -0.05em;
}
```

```html
<!-- Network section — chip-scale wordpair -->
<span class="wordpair">
  <span class="wordpair-ar" lang="ar">الشبكة</span>
  <span class="wordpair-sep" aria-hidden="true">/</span>
  <span class="wordpair-en" lang="en" dir="ltr">Network</span>
</span>

<!-- Hero — wordmark-scale -->
<h1 class="wordpair wordpair--hero">
  <span class="wordpair-ar" lang="ar">المبادر</span>
  <span class="wordpair-sep" aria-hidden="true">/</span>
  <span class="wordpair-en" lang="en" dir="ltr">Almobadir</span>
</h1>
```

**Reference**: Wave 1 — REF 15 (Typotheque + TPTQ Arabic, https://typotheque.com / https://tptq-arabic.com — Peter Biľak's multi-script foundry, caps-aligned bilingual specimens). Free-font implementation: REF 12 (Are.na blog, https://www.are.na/blog — IBM Plex Sans Arabic + IBM Plex Sans Variable from same designer team, metric-paired). Arabic anchor: REF 9 (Brownbook, https://brownbook.me/about/ — Arabic 1.4× Latin in nameplate proves the bilingual hierarchy is a positioning statement encoded in type size, not a translation gesture).

**Bilingual proof**: The component does the single most-asked-for fix in the typography rhythm audit (F-section, every bilingual chip in the site is wrong about bidi). Both scripts get correct `lang` and `dir` attributes for accessibility. The Arabic translateY(-0.08em) is the visual move; the `unicode-bidi: isolate` on the separator is the correctness move. Test mentally with `الشبكة / Network`: the Arabic ش aligns its top with the Latin cap-N, not its x-height. With `المبادر / Almobadir` at hero scale, the م top aligns with the A cap. Same component handles both. When the toggle flips to LTR-first, the Latin lands first visually but the Arabic still gets its cap-lift — the gesture is direction-symmetric.

**Effort to roll out**: ~4hr (1 component ≈ 45 lines CSS; load IBM Plex Sans Arabic Variable + IBM Plex Sans Variable from Google Fonts ≈ 2 `@font-face` blocks in fonts.css; refactor 5 markup sites with strict `lang` + `dir` per child; remove ad-hoc bilingual rules in `.nw3a-*` and `.nl3-*` ≈ 30 lines).

---

## Composition rule: how the 5 signatures work as a SYSTEM

**One scale**, **one accent color**, **two registers per script**, **one canonical letter-spacing for Latin**, **zero letter-spacing for Arabic always**.

| Layer | Sig 1 (eyebrow) | Sig 2 (dropcap) | Sig 3 (stat) | Sig 4 (quote) | Sig 5 (wordpair) |
|---|---|---|---|---|---|
| **Mono Latin role** | Section register tag | n/a | Stat unit label | Attribution | Separator only |
| **Arabic role** | Editorial register | Architectural opener | Eastern Arabic numeral | Quote body | Wordmark body |
| **Latin role** | Caps register | Drop-cap fallback | Wide-axis numeral | Italic serif body | Wordmark body |
| **Accent color** | `--crimson` (status dot) | `--crimson` (cap + rule) | `--crimson` (figure) | `--crimson` (rule) | inherits `--fg` |
| **Surface** | Both | Both | Both | `--paper` always | Both |
| **Letter-spacing Latin** | `0.22em` | inherits | `-0.04 → 0.22em` | `-0.005em` | `0.005em` |
| **Letter-spacing Arabic** | `0` | `0` | `0` | `0` | `0` |
| **Variable axis used** | Plex `wght` 500/600 | Cairo `wght` 900 | Roboto Flex `wdth` 151 | Cairo `wght` 300 | Plex `wght` 600/900 |
| **Bidi-isolation** | sep | float reverses | label | quote attr | sep |

**The composition rule the user will feel without naming**:

1. **Crimson is for typography only.** Every red mark in the system is type-related: status dot, dropcap, stat figure, quote rule. Nothing else gets `--crimson`. (This already constrains 80% of the page's accent decisions without writing a guideline.)
2. **Mono Latin is the metadata register.** It only ever appears as eyebrow, stat label, or quote attribution. Never body. Never headline. Mono = "this is meta, this is registration, this is when/who/what."
3. **Arabic letter-spacing is always zero.** No exceptions. Any positive tracking destroys the script's identity per audit C2.
4. **Numerals declare their script per section.** Eastern Arabic for editorial register (manifesto, newsletter, founder dossier numbers), Western for tabular/comparison (hero KPI, footer clock, network counters). Lock once per section.
5. **Cap-alignment is the bilingual default.** Whenever Arabic and Latin appear inline, the Arabic gets `translateY(-0.08em)` (or `-0.05em` at display scale) so cap-heights match. Use the wordpair component or copy its rule.
6. **Cream slab = quote slab.** The only place `--paper` appears on a dark page is inside a pull-quote. The cream signals "this is a different voice."

If the user reads through the page and asks "why does this feel like one publication?", the answer is: 1.250 ratio, 0.22em Latin / 0em Arabic, crimson-on-type only, cap-alignment always, eastern/western numerals locked per section.

---

## Migration plan: which CSS rules in v3.css need replacing

Touch order — top of file to bottom of v3.css, cumulative ~210 lines of CSS deleted, ~245 lines of system CSS added (single net delta + ~35 lines, far better than the 30-distinct-font-sizes status quo):

**Phase 1 — Foundation (ship before any component refactor)**
- `tokens.css:60–70` — keep canonical scale; add `--t-stat-giant: clamp(140px, 18vw, 240px)`.
- `tokens.css:88–90` — declare `--ls-eyebrow-canon: 0.22em`, `--ls-stat-tight: -0.04em`, delete the implicit 5-tier eyebrow chaos.
- `fonts.css` — load **IBM Plex Sans Arabic Variable**, **IBM Plex Sans Variable**, **Roboto Flex**, **Cairo Variable**, **Instrument Serif**, **JetBrains Mono Variable** from Google Fonts (`<link rel="preconnect">` + `@font-face` declarations as needed). The brand-font 404 is *expected* — these are the public chain.
- `base.css:212` — promote the existing `.num` utility to actually be applied (audit E3).

**Phase 2 — Component CSS (introduces the 5 signatures, deletes their predecessors)**

| Replace | Lines | With |
|---|---|---|
| `.hero3__eyebrow`, `.hero3__eyebrow-label`, `.hero3__eyebrow-sep`, `.hero3__eyebrow-mono` (v3.css 205–212, 982–983) | ~20 | `.eyebrow` (Sig 1) |
| `.hdr3-mega-eyebrow` (v3.css 100) | 2 | `.eyebrow` (Sig 1) |
| `.manif3__head`, `.manif3__eyebrow`, `.manif3__eyebrow--en` (v3.css 300–303) | 8 | `.eyebrow` (Sig 1) |
| `.nw3a-eyebrow`, `.nw3a-eyebrow-dot`, `.nw3a-eyebrow-ar` (v3.css 332–335) | 5 | `.eyebrow` + `.eyebrow-status` (Sig 1) |
| `.mtd3__eyebrow`, `.mtd3__eyebrow-dot` (v3.css 413–414) | 4 | `.eyebrow` (Sig 1) |
| `.cnt3-eyebrow` (v3.css 607) | 2 | `.eyebrow` (Sig 1) |
| `.nl3-eyebrow`, `.nl3-eyebrow-ar`, `.nl3-eyebrow-sep` (v3.css 671–673) | 4 | `.eyebrow` (Sig 1) |
| `.fnd3a__eyebrow`, `.fnd3a__eyebrow--mono`, `.fnd3a__sub-eyebrow` (v3.css 770–771, 793–794) | 6 | `.eyebrow` (Sig 1) |
| `.ftr3-cta__eyebrow` (v3.css 882) | 2 | `.eyebrow` (Sig 1) |
| **eyebrow subtotal** | **~53** | **+35 (.eyebrow component)** |
| `.fnd3a__dropcap` (existing, ~12) + `::first-letter` rules (none currently — net add) | ~12 | `.editorial::first-letter` + marginalia (Sig 2, +50) |
| `.hero3__kpi-value`, `.hero3__num`, `.hero3__chip-num` (v3.css 252–253, 269) | 5 | `.stat-fig[data-script]` + `.stat-label` (Sig 3) |
| `.nw3a-total dd`, `.nw3a-count`, `.nw3a-count-fig`, `.nw3a-count-unit`, `.nw3a-count-label`, `.nw3a-stat-fig` (v3.css 341, 385–392) | ~14 | `.stat-fig` (Sig 3) |
| `.mtd3__num` (v3.css 453, 517, 543, 594, 1055) | ~12 | `.stat-fig--giant[data-script="arab"]` (Sig 3) |
| `.nl3-stat-fig`, `.nl3-block-num`, `.nl3-issue-num`, `.nl3-mock-issue` numeric runs (v3.css 736, 741) | ~6 | `.stat-fig[data-script="arab"]` (Sig 3) |
| `.fnd3a__stat-val`, `.fnd3a__follow-stat`, `.fnd3a__auth-num` (v3.css; counter-animated) | ~9 | `.stat-fig[data-counter]` (Sig 3) |
| `.ftr3-clock__time`, `.ftr3-mono` (v3.css 916, 943) | 4 | `.stat-fig` (Sig 3) |
| **stat subtotal** | **~50** | **+60 (.stat component)** |
| `.fnd3a__quote-text` (v3.css 748) + parent quote container | ~10 | `.pull-quote` (Sig 4) |
| `.nl3-quote blockquote` (v3.css 651) | 4 | `.pull-quote` (Sig 4) |
| `.manif3__shadow` (v3.css 316) | 6 | `.pull-quote` (Sig 4) |
| `.cnt3-card--featured .cnt3-excerpt` (n/a today, net add) | 0 | `.pull-quote` (Sig 4) |
| **quote subtotal** | **~20** | **+50 (.pull-quote component)** |
| `.hero3__wordmark` (v3.css 213–215) | 5 | `.wordpair--hero` (Sig 5) |
| `.nw3a-eyebrow-ar / -en` bilingual logic (v3.css 335) | 2 | `.wordpair` (Sig 5) |
| `.fnd3a__sub-eyebrow > span:first-child` (v3.css 794) | 2 | `.wordpair` (Sig 5) |
| `.nl3-eyebrow-ar / -en` (v3.css 672) | 3 | `.wordpair` (Sig 5) |
| Header brand block (currently inline in `.hdr3-*`) | ~6 | `.wordpair` (Sig 5) |
| **wordpair subtotal** | **~18** | **+45 (.wordpair component)** |
| **TOTAL** | **~165** | **+~240** |

**Phase 3 — Markup pass (`index.html`)**

Find every Latin run inside an Arabic ancestor and wrap it `<span lang="en" dir="ltr">` per audit F1 (40+ sites). Replace each retired class block with its signature class. Remove dead `font-feature-settings: "ss01"` from `.manif3__word--accent` (audit E2 — IBM Plex Sans Arabic ss01 will work, the rule survives the migration once Plex loads).

**Phase 4 — Verification**
- Visual diff against current screenshots in `design-audit/screenshots/`.
- Bilingual proof: every `.wordpair` rendered at chip + hero scale, in both `dir=rtl` and `dir=ltr`, no script visually demoted.
- Audit-compliance check: `--lh-hero` ≥ 1.0 (B1), zero positive letter-spacing on any element with Arabic content (C2), one numeral system per section (K1, K2), bidi-isolated separators (F1), one eyebrow letter-spacing canon (C1).
- Performance: variable fonts subset to ar+latn ranges via `unicode-range`; total font weight target ≤ 380KB (Plex Arabic Variable ar-only ≈ 80KB, Plex Latin Variable latn-only ≈ 90KB, Roboto Flex latn ≈ 95KB, Cairo Variable ar ≈ 70KB, Instrument Serif latn ≈ 28KB, JetBrains Mono Variable latn ≈ 75KB — total raw ≈ 438KB, post-subset ≈ 320KB).

**Total migration**: ~22hr (4+5+6+3+4 from sig totals = 22). Achievable in 3 focused engineering days. Phase 1 + 2 is the heavy lift; Phase 3 + 4 is mechanical.

---

## 200-word summary

Almobadir's typographic signature is five recurring gestures, designed as a system, repeated across every section: **(1) the Bilingual Eyebrow** that pairs an Arabic register-word with a Latin caps tag and an optional Eastern Arabic numeral, separator-isolated, caps-aligned, recurring across all 8 section eyebrows; **(2) the Architectural Drop-Cap** that floats an Arabic or Latin first-letter to the inline-start in `--crimson`, paired with a Stripe-Press-style 220px marginalia gutter, on every long-form opener; **(3) the Wide-Numeral Stat Frame** that uses Roboto Flex `wdth: 151` for Western digits and Cairo Variable Black for Eastern Arabic, with a mono caps label and tabular figures, one numeral system locked per section; **(4) the Cream Pull-Quote with Crimson Em-Rule** that drops every quote onto a `--paper` slab in Instrument Serif italic for Latin and Cairo Variable Light for Arabic, with a 1px crimson rule on the inline-start; and **(5) the Cap-Aligned Bilingual Wordmark Pair** that uses IBM Plex Sans Arabic + Plex Latin (free, metric-paired by the same designer) with a `translateY(-0.08em)` lift on Arabic so cap-heights align — replacing the current site-wide x-height misalignment in one rule. The system uses two variable-font families, one mono, one editorial serif, one fluid 1.250 scale, one Latin letter-spacing canon (0.22em), zero Arabic letter-spacing always, and `--crimson` reserved for type only. ~22hr migration replaces ~165 lines of v3.css drift with ~240 lines of one-system CSS. Survives the brand-font 404, ready to upgrade when Mostaqbali / Graphik Arabic return without changing component shape.
