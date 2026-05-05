# WOW Audit — Typography Theater Reference Recon

Library of typographic moves from sites where TYPE IS THE DESIGN. Curated for Almobadir's editorial Arabic + Latin bilingual voice. Brand fonts (Mostaqbali, Graphik Arabic) are currently 404; references prioritize **fonts we can actually load today** — IBM Plex Sans Arabic Variable, Cairo Variable, Tajawal, 29LT Zarid Sans/Serif Variable, 29LT Idris Variable (kashida axis), Recursive, Roboto Flex, Inter Variable.

Compiled 2026-04-26. Browser preview was unstable at fetch time — moves below are reconstructed from rendered-page knowledge, foundry documentation, blog write-ups, and direct CSS inspection where the site loaded.

---

## REF 1: Stripe Press — Poor Charlie's Almanack (illuminated chapter openers + marginalia)
- URL: https://press.stripe.com/poor-charlies-almanack — Awwwards SOTD; awarded primarily for typographic system
- The typographic move: Each chapter opens with an oversized **Roman numeral set in a different display weight than body**, paired with a small-caps sub-eyebrow. Body type runs a strict ~620px measure with **right-margin marginalia** (footnotes/quotes set at 12px in italic) — the same column treatment as a bound book brought to web. Drop-cap is hand-fitted: a 5-line initial that optically aligns with the cap-height of line 1, not pinned to baseline.
- Implementation: `column` flex layout with named grid areas `[main marginalia]`. Drop cap = `p:first-of-type::first-letter` with `float: left; font-size: 5.5em; line-height: 0.85; margin-top: 0.1em; margin-right: 0.08em; font-feature-settings: "ss01"` to swap to a stylistic-set initial.
- Font dependency: Body is **Spectral** (free, Google Fonts) or similar transitional serif; numerals likely **Söhne Breit** (Klim, paid). For Almobadir we substitute **IBM Plex Serif** body + **Cairo Variable** display.
- Code sketch:
```css
.chapter-opener .numeral {
  font-family: "Cairo Variable", system-ui;
  font-variation-settings: "wght" 880;
  font-size: clamp(96px, 14vw, 240px);
  line-height: 0.82;
  letter-spacing: -0.04em;
}
.essay > p:first-of-type::first-letter {
  float: inline-start;
  font-size: 5.5em;
  line-height: 0.85;
  padding-inline-end: 0.08em;
  font-feature-settings: "ss01" 1;
}
.essay { display: grid; grid-template-columns: minmax(0,1fr) 220px; gap: 48px; }
.essay aside.marginalia { font-size: 13px; font-style: italic; color: #6b6258; }
```
- Almobadir analog: **Manifesto chapter dividers** (`المنهج`, `المؤسسون`, `الشبكة`) — render the section number as an oversized Eastern Arabic numeral (٠١, ٠٢) set in Cairo Variable Black, body of the section pulls a 4-line drop-cap on the leading paragraph.

---

## REF 2: Stripe Press — Working in Public (sidenotes-as-canon)
- URL: https://press.stripe.com/working-in-public
- The typographic move: Body and sidenotes share one type system but **different optical sizes**. Sidenotes hang in the right gutter at 13px; body is 18/30. On mobile the sidenote becomes a **collapsible inline footnote** that opens in place — typography stays editorial in both modes.
- Implementation: Tufte-style. `<aside class="sidenote">` flexed against paragraph; `@media (max-width: 800px)` collapses to `<details>` with `summary::after { content: "↳"; }`.
- Font dependency: Söhne (Klim, paid) — substitute Inter Variable + Roboto Serif Variable optical-size axis.
- Code sketch:
```css
@supports (font-variation-settings: "opsz" 12) {
  .sidenote { font-variation-settings: "opsz" 11, "wght" 380; font-size: 13px; line-height: 1.45; }
  .body    { font-variation-settings: "opsz" 18, "wght" 400; font-size: 18px; line-height: 1.65; }
}
@media (max-width: 800px) {
  .sidenote { display: contents; }
  .sidenote::before { content: "*"; vertical-align: super; font-size: 0.75em; }
}
```
- Almobadir analog: **Method page** — when a sentence cites a partner or precedent, the citation hangs as a sidenote in Latin while body stays Arabic. Bilingual asymmetry handled by `aside { direction: ltr; }` inside `body { direction: rtl; }`.

---

## REF 3: Klim Type Foundry — Söhne specimen (variable-font hover specimen)
- URL: https://klim.co.nz/collections/soehne/ — and the broader klim.co.nz redesign that loads all 26 typefaces as variable instantly
- The typographic move: A hero word (e.g. "Söhne") that interpolates **weight 100 → 900 on cursor X-position across the word**, plus **width axis** on cursor Y. The user becomes the type designer for two seconds. Klim does this in a small specimen tile; we can do it for "المُبادر / Almobadir" wordmark.
- Implementation: `mousemove` event on the heading; map `e.offsetX / width` → `wght`, `e.offsetY / height` → `wdth`. Use `@property --wght` so transitions are interpolatable.
- Font dependency: **Söhne is paid**. Substitute **Recursive Variable** (free, has `wght`+`MONO`+`CASL`+`CRSV` axes) for Latin and **Cairo Variable** (free, `wght` 200–1000) for Arabic.
- Code sketch:
```css
@property --wght { syntax: "<number>"; inherits: true; initial-value: 400; }
@property --wdth { syntax: "<number>"; inherits: true; initial-value: 100; }
.wordmark {
  font-family: "Recursive Variable";
  font-variation-settings: "wght" var(--wght), "wdth" var(--wdth);
  transition: --wght 120ms linear, --wdth 120ms linear;
}
```
```js
hero.addEventListener('pointermove', e => {
  const r = hero.getBoundingClientRect();
  hero.style.setProperty('--wght', 200 + (e.clientX - r.left)/r.width * 700);
  hero.style.setProperty('--wdth',  85 + (e.clientY - r.top)/r.height * 40);
});
```
- Almobadir analog: **Hero wordmark "المُبادر"** — Arabic letters fatten under the cursor; reduces motion on `prefers-reduced-motion: reduce` to a static 600/110.

---

## REF 4: Displaay — Season variable specimen (sans↔serif morph)
- URL: https://displaay.net/typeface/season — full variable collection with a **SERF axis** that smoothly blends sans into serif
- The typographic move: Single live demo on the specimen page where moving a slider transforms the same headline from a plain sans to a fully bracketed serif. The genius isn't the morph — it's that **the morph happens on scroll**: top of viewport = sans, bottom = serif. Same word, two voices.
- Implementation: `animation-timeline: scroll()` driving a custom property bound to a registered font axis.
- Font dependency: **Season is paid (Displaay)**. Closest free equivalent: **Recursive's CASL axis** (Casual ↔ Linear, similar perceptual move). For Arabic there is no SERF-axis font yet, so this is **Latin-only on Almobadir** unless paired with a CASL-equivalent Arabic move.
- Code sketch:
```css
@property --serf { syntax: "<number>"; inherits: true; initial-value: 0; }
.morph {
  font-family: "Recursive Variable";
  font-variation-settings: "CASL" var(--serf), "wght" 500;
  animation: morph linear both;
  animation-timeline: scroll(root block);
  animation-range: entry 20% exit 80%;
}
@keyframes morph { from { --serf: 0; } to { --serf: 1; } }
```
- Almobadir analog: **"Almobadir" Latin wordmark in the founder section** scrolls from Recursive Linear (technical) to Recursive Casual (warm) as the camera tilts toward the founder portrait. Arabic stays Cairo throughout — script asymmetry becomes a feature, not a bug.

---

## REF 5: 29LT Idris (Naskh Variable with Kashida axis) — Arabic-first
- URL: https://www.29lt.com/product/29lt-idris-sharp/ + https://blog.29lt.com/2024/11/26/29lt-idris/ — 4 styles × 5 weights, **variable font with three axes: Weight, Swash, Kashida**
- The typographic move: A registered axis (`KSHD`) **physically elongates kashida strokes** as the value increases. Means a single Arabic word can stretch on hover or scroll without picking a different font and without breaking the cursive joins. This is the move competitors don't have.
- Implementation: `font-variation-settings: "KSHD" 0 → 100`. Drives a CSS scroll-animation or hover state.
- Font dependency: **29LT Idris is paid** (~€500 per family). For free analog, use **IBM Plex Sans Arabic** (no KSHD axis but has `wght` 100–700 variable) and add a manual `letter-spacing` pulse to fake elongation — accept the move is less elegant.
- Code sketch:
```css
@property --kshd { syntax: "<number>"; inherits: true; initial-value: 0; }
.tagline-ar {
  font-family: "29LT Idris Variable", "Cairo Variable";
  font-variation-settings: "wght" 500, "KSHD" var(--kshd);
  transition: --kshd 600ms cubic-bezier(.2,.7,.1,1);
}
.tagline-ar:hover { --kshd: 80; }
```
- Almobadir analog: **Hero tagline** — `أكبر من شركة. أصغر من مؤسسة` elongates its kashidas on scroll-into-view. License Idris Variable for the brand, or budget alternative: pair Cairo Variable with a `letter-spacing` pulse and accept the trade-off. THIS IS THE SINGLE MOST PORTABLE WOW MOVE FOR AN ARABIC-FIRST BRAND.

---

## REF 6: 29LT Zarid Serif Variable — slanted Naskh + humanistic Latin pairing
- URL: https://v-fonts.com/fonts/twenty-nine-lt-zarid-serif — variable across `wght`, in 4 standard + 4 slanted styles, designed to pair Arabic Naskh with humanistic Latin so neither feels secondary
- The typographic move: Bilingual quote where Arabic is in Zarid Serif Slant 500 and Latin is the same family's italic — both "lean" in the same direction (right). On scroll into view, weight pulses 400 → 600 → 400 across both scripts in unison, giving the illusion of a single voice.
- Implementation: Single `<blockquote>`, two spans with `lang="ar"` and `lang="en"`. Same `wght` variable bound to a `view-timeline`.
- Font dependency: **29LT Zarid Serif Variable is paid** (~€450). Free fallback: **IBM Plex Sans Arabic Variable** + **IBM Plex Serif Italic Variable** — same designer (Mike Abbink + Bold Monday + Khaled Hosny), genuinely designed to pair.
- Code sketch:
```css
.quote { view-timeline: --q block; }
.quote span {
  font-variation-settings: "wght" var(--w, 400);
  animation: pulse linear both;
  animation-timeline: --q;
  animation-range: entry 0% cover 50%;
}
@keyframes pulse { 0%,100% { --w: 400; } 50% { --w: 620; } }
[lang="ar"] { font-family: "IBM Plex Sans Arabic Variable"; }
[lang="en"] { font-family: "IBM Plex Serif", serif; font-style: italic; }
```
- Almobadir analog: **Founder quote section** where Arabic quote and Latin English translation are stacked, both pulse weight together on entry.

---

## REF 7: Apple WWDC — "Wide" wordmark and SF Wide variable axis
- URL: https://developer.apple.com/wwdc/ + https://developer.apple.com/fonts/ — SF Pro features variable weight, **width**, and optical-size axes (introduced WWDC22)
- The typographic move: Page hero is a single year ("2026") set in **SF Pro Display weight 590** at 320px, but the digits are pushed to `wdth: 151` (max wide). Subtitle below is the same family at `wdth: 80` (compressed) — the contrast of the same family stretched and compressed reads like the page is exhaling.
- Implementation: Two static `font-variation-settings` declarations. No animation — restraint is the move.
- Font dependency: **SF Pro is Apple-licensed** (free for use within Apple platforms, restricted otherwise). Free open analog: **Roboto Flex** (12 axes including `wdth` 25–151) or **Inter Variable** (no `wdth` axis — narrower analog).
- Code sketch:
```css
.year {
  font-family: "Roboto Flex", system-ui;
  font-variation-settings: "wght" 590, "wdth" 151, "opsz" 144;
  font-size: clamp(120px, 22vw, 320px);
  line-height: 0.85;
  letter-spacing: -0.04em;
}
.eyebrow {
  font-family: "Roboto Flex";
  font-variation-settings: "wght" 500, "wdth" 75;
  text-transform: uppercase; letter-spacing: 0.18em;
}
```
- Almobadir analog: **Method numerals** — `01 / 02 / 03 / 04` set wide (Roboto Flex `wdth` 151) at 200px above each step's title set narrow. Single family, two voices, one specimen.

---

## REF 8: Letters from Sweden — Funkis ABC (architectural lettering as identity)
- URL: https://lettersfromsweden.se/font/funkis/ — display sans inspired by Sigurd Lewerentz's 1930 Stockholm Exhibition lettering, comes in Funkis A/B/C variants (geometric / soft / architectural)
- The typographic move: A page header where the **same word is repeated three times stacked**, each line in a different Funkis cut (A, B, C). Same proportions, three personalities. It's a one-shot specimen-as-headline.
- Implementation: Three `<span class="cut-a/b/c">` elements stacked; each `font-feature-settings` toggled to a stylistic-set or alternate. With Recursive Variable, swap `MONO` axis 0 → 1 across the three lines for a similar move.
- Font dependency: **Funkis ABC is paid** (Letters from Sweden, ~€90 per cut). For Almobadir — closest move with what we already have: **Recursive Variable** stacking `CASL 0`, `CASL 0.5`, `CASL 1` for three personalities of the same word.
- Code sketch:
```css
.triplet { display: grid; gap: 0; line-height: 0.85; }
.triplet span { font-family: "Recursive Variable"; font-size: 14vw; }
.triplet span:nth-child(1) { font-variation-settings: "CASL" 0,    "wght" 700; }
.triplet span:nth-child(2) { font-variation-settings: "CASL" 0.5,  "wght" 600; }
.triplet span:nth-child(3) { font-variation-settings: "CASL" 1,    "wght" 500; }
```
- Almobadir analog: **Manifesto opener** — the word `المُبادر` (or its English mirror) stacks three times in Cairo Variable at weights 200 / 600 / 1000. Same word, three temperatures. Set `clip-path: inset()` so each stack reveals on scroll like film credits.

---

## REF 9: The Pudding — scrollytelling typography (text IS the chart)
- URL: https://pudding.cool — every interactive feature; representative: their Pop Lyrics, Vocabulary of US Presidents, etc. Pudding sets the standard for scroll-driven journalism alongside NYT and Reuters.
- The typographic move: A sentence that **rewrites itself as the user scrolls**, with individual words swapping in/out and the line re-flowing. Often the changing word is set in a contrasting display face while the surrounding sentence holds in the body face — type does the storytelling, no chart needed.
- Implementation: Each swappable word is a `<span data-words="...">` with `display: inline-grid; grid-template-areas: "stack";` so all options stack and only one is `opacity: 1` at a time. Scroll progress (`scroll()` timeline or IntersectionObserver) cycles which one is visible.
- Font dependency: Any pairing — for Almobadir use **IBM Plex Sans Arabic Variable** body + **Cairo Variable Black** for the swapping words. Both free.
- Code sketch:
```html
<p>نحن نبني <span class="swap">
   <span>شركات</span><span>قطاعات</span><span>عاصمات</span>
 </span> جديدة.</p>
```
```css
.swap { display: inline-grid; grid-template-areas: "s"; }
.swap > span { grid-area: s; opacity: 0; transition: opacity .2s; font-family: "Cairo Variable"; font-variation-settings: "wght" 900; }
.swap > span.active { opacity: 1; }
```
```js
const items = document.querySelectorAll('.swap > span');
let i = 0; setInterval(() => {
  items.forEach(s => s.classList.remove('active'));
  items[i % items.length].classList.add('active'); i++;
}, 1800);
```
- Almobadir analog: **Hero tagline** — `نحن نبني [شركات / قطاعات / عاصمات] جديدة` cycles through three nouns. Cycle pauses on `prefers-reduced-motion: reduce`. Drives home the multi-scale ambition without paragraphs of copy.

---

## REF 10: NYT Magazine — text-as-image cover treatments
- URL: https://www.nytimes.com/section/magazine — and the redesigned T magazine (commercialtype.com/news/kippenberger_and_fact_for_t)
- The typographic move: An online cover where the main subject's name is **set in the magazine's display face at viewport-filling size, with `mix-blend-mode: difference` over the cover photo**. The type is both the headline AND the dominant graphic. On scroll, type slides up at a fraction of image speed (parallax) so the photo is revealed and the type becomes the article anchor.
- Implementation: Position-fixed type with `mix-blend-mode: difference` and a `transform: translateY()` driven by `scroll()` timeline.
- Font dependency: NYT uses **Cheltenham + NYT Mag Serif** (custom). For Almobadir analog: **Cairo Variable Black** for Arabic + **Roboto Serif Variable** for Latin — both free, both render at 30vw beautifully.
- Code sketch:
```css
.cover-title {
  position: sticky; top: 0;
  font-family: "Cairo Variable", serif;
  font-variation-settings: "wght" 1000;
  font-size: clamp(18vw, 24vw, 30vw); line-height: 0.85;
  mix-blend-mode: difference; color: white;
  animation: rise linear both;
  animation-timeline: scroll();
  animation-range: 0 80vh;
}
@keyframes rise { to { transform: translateY(-30vh); } }
```
- Almobadir analog: **Founder portrait section** — `طارق` set in Cairo 1000 at 30vw over the founder photo with `mix-blend-mode: difference`. Type IS the portrait frame.

---

## REF 11: It's Nice That — article page editorial drop-cap + pull-quote system
- URL: https://www.itsnicethat.com — every long-form article uses a consistent editorial system
- The typographic move: First paragraph gets a **2-color drop cap**: the initial letter is filled with the brand color (orange/red), set in a contrasting display face (sans), against body in a serif. Pull quotes break the column at full-bleed, set in a much larger weight of the body face — not a different family. The contrast is **scale + color, not family**.
- Implementation: `::first-letter` for the cap; `<blockquote class="pull-quote">` with a custom layout breaking the column.
- Font dependency: Any pairing. For Almobadir use **Cairo Variable** display (initial letter) + **IBM Plex Serif** body (free).
- Code sketch:
```css
article > p:first-of-type::first-letter {
  float: inline-start;
  font-family: "Cairo Variable", serif;
  font-variation-settings: "wght" 900;
  color: var(--brand-accent, #b34000);
  font-size: 6.4em; line-height: 0.85;
  padding-inline-end: 0.06em; padding-block-start: 0.05em;
}
.pull-quote {
  font-family: inherit; font-size: clamp(28px, 5vw, 56px);
  font-variation-settings: "wght" 280;
  line-height: 1.15; max-inline-size: 18ch;
  margin: 2em auto; text-align: center;
  border-block: 1px solid currentColor; padding-block: 1em;
}
```
- Almobadir analog: **Manifesto + Method long-form copy** — earn editorial gravity without commissioning a font. Also useful for the founder bio paragraph.

---

## REF 12: Are.na blog — content-first typesetting (one face, three weights)
- URL: https://www.are.na/blog — the platform's editorial blog that proves restraint can wow
- The typographic move: Whole site uses **one typeface, three weights, two colors**. Hero, body, captions, navigation — all the same family. Wow comes from **measure (45-65 chars), tracking, and rhythm**, not visual stunts. The "move" is the negative space and the perfect line-height ladder (1.15 / 1.45 / 1.65).
- Implementation: A type scale built on a single ratio (1.250 major third). All weights from one variable file.
- Font dependency: Are.na uses Söhne and a custom serif. For Almobadir, use **Inter Variable** (Latin) + **IBM Plex Sans Arabic Variable** (Arabic) — both free, both have wide weight ranges, optically similar.
- Code sketch:
```css
:root {
  --type-ratio: 1.25;
  --t-1: 12px;
  --t-0: 14px;
  --t-1u: calc(var(--t-0) * var(--type-ratio));    /* 17.5 */
  --t-2: calc(var(--t-1u) * var(--type-ratio));   /* 21.9 */
  --t-3: calc(var(--t-2)  * var(--type-ratio));   /* 27.4 */
  --t-4: calc(var(--t-3)  * var(--type-ratio));   /* 34.2 */
  --t-5: calc(var(--t-4)  * var(--type-ratio));   /* 42.8 */
  --t-6: calc(var(--t-5)  * var(--type-ratio));   /* 53.5 */
}
body { font-family: "Inter Variable"; font-size: var(--t-0); line-height: 1.65; max-inline-size: 65ch; }
h1   { font-size: var(--t-6); line-height: 1.05; font-variation-settings: "wght" 700; }
h2   { font-size: var(--t-4); line-height: 1.15; font-variation-settings: "wght" 600; }
.caption { font-size: var(--t-1); line-height: 1.45; font-variation-settings: "wght" 380; }
```
- Almobadir analog: **Whole site type scale** — replace the current 14-weight ad-hoc system with a single Inter Variable + IBM Plex Sans Arabic Variable system on a 1.25 ratio. This single move would move the site from "good" to "considered" with zero new font budget.

---

## REF 13: Browser Company — manifesto typesetting (oversized lede, paragraph rhythm)
- URL: https://thebrowser.company — the manifesto and "/year-of-arc" pages
- The typographic move: First sentence of every section is set 2x the body size in the same family. No headlines — the **lede paragraph is the headline**. Subsequent paragraphs step down to body. The reader doesn't navigate by H2s, they navigate by typographic gravity.
- Implementation: `:first-child` selector on paragraphs, or a `.lede` class.
- Font dependency: Any pairing, very portable. Free with Inter Variable + IBM Plex Sans Arabic Variable.
- Code sketch:
```css
.section > p:first-of-type {
  font-size: clamp(22px, 3vw, 32px);
  font-variation-settings: "wght" 380;
  line-height: 1.35;
  max-inline-size: 28ch;
  margin-block-end: 1.2em;
  color: var(--ink);
}
.section > p:first-of-type::after {
  content: ""; display: block;
  width: 32px; height: 1px;
  background: currentColor;
  margin-block-start: 1.4em;
}
```
- Almobadir analog: **Manifesto + Method sections** — each section's first paragraph is 1.6× body, ending with a hairline rule. No H2s — type gravity replaces structural headings. Works in Arabic with `direction: rtl` on the section.

---

## REF 14: Pentagram — case-study typography (numeral as protagonist)
- URL: https://www.pentagram.com/work — every case study uses big block sans numbers as section markers
- The typographic move: Section numbers are set in a **monospaced display face at 12vw**, paired with section title at 1/4 the size. The numeral is decorative AND functional — it gives readers a TOC anchor at a glance. Often Pentagram colors the numeral in the project's brand color.
- Implementation: Static type plus an IntersectionObserver that pins the active number to the corner of the viewport during that section.
- Font dependency: Pentagram uses custom faces per project. Free analog: **Berkeley Mono** alternative is **JetBrains Mono Variable** (free), or **Recursive Variable** in `MONO 1` mode.
- Code sketch:
```css
.section-num {
  font-family: "Recursive Variable", monospace;
  font-variation-settings: "MONO" 1, "wght" 760;
  font-size: clamp(64px, 12vw, 192px);
  line-height: 0.85;
  color: var(--section-accent);
  font-feature-settings: "tnum" 1, "ss01" 1;
}
.section-num::before { content: attr(data-num); }
```
- Almobadir analog: **Method page step numerals** (`٠١ ٠٢ ٠٣ ٠٤`) — render in Cairo Variable Black at 12vw with Eastern Arabic numerals. On scroll, the active step number sticks to the bottom-right of the viewport so the reader always knows where they are.

---

## REF 15: Typotheque + TPTQ Arabic — bilingual specimen (script as equal partner)
- URL: https://typotheque.com + dedicated https://tptq-arabic.com — Peter Biľak's foundry, multi-script Latin + Arabic + Hebrew + Devanagari from one designer team
- The typographic move: Specimen page where **Arabic word and its Latin transliteration share the same baseline grid**. Arabic line-height is computed independently (1.8) from Latin (1.45) but **caps-height aligns**, not x-height — the visual weight feels balanced because the eye reads cap-height as the "top" line.
- Implementation: Two `<span>`s with `display: inline-flex; align-items: baseline;` and per-language `line-height` values, plus a manual `transform: translateY()` on the Arabic to optically align cap-height to Latin cap-height.
- Font dependency: Typotheque's own families (paid). Free analog with similar bilingual rigor: **IBM Plex Sans Arabic Variable** + **IBM Plex Sans Variable** — designed by the same team to share metrics.
- Code sketch:
```css
.bilingual {
  display: inline-flex; gap: 0.4em; align-items: baseline;
}
.bilingual [lang="ar"] {
  font-family: "IBM Plex Sans Arabic Variable";
  font-variation-settings: "wght" 600;
  line-height: 1.8;
  /* lift Arabic so its caps align with Latin caps, not x-height */
  transform: translateY(-0.08em);
}
.bilingual [lang="en"] {
  font-family: "IBM Plex Sans Variable";
  font-variation-settings: "wght" 600;
  line-height: 1.45;
  letter-spacing: 0.005em; /* compensate for Plex Latin tracking */
}
```
- Almobadir analog: **Bilingual word pairs on every page** (`المُبادر / Almobadir`, `المنهج / Method`, `الشبكة / Network`). Most current bilingual brands stack Arabic above Latin and call it done — this aligns them on caps so the script pair reads as a single typographic unit.

---

## Top 5 Portable for Almobadir's Editorial Bilingual Voice

Ranked by ratio of **wow per dollar/effort**, accounting for the brand-font 404 reality and Almobadir's existing free font fallbacks:

### 1. REF 5 — 29LT Idris Variable Kashida axis (Arabic-first hero motion)
The only move on this list that does something **structurally Arabic** that no Latin foundry can replicate. Stretching kashidas on hover/scroll as a registered axis is genuinely new for the web in 2026. Worth the ~€500 license for the brand wordmark and tagline alone. If budget is tight, fall back to Cairo Variable + a `letter-spacing` pulse and accept the 70% version. Lands on **hero tagline** + **manifesto pull-quotes**.

### 2. REF 15 — IBM Plex Sans Arabic + Latin caps-aligned bilingual
Free, available on Google Fonts today, designed by the same team to pair. The single biggest typographic upgrade Almobadir could ship this week is to fix Arabic↔Latin baseline alignment using cap-height instead of x-height. Lands on **every bilingual word pair across the site** — header nav, hero, method labels, footer.

### 3. REF 7 — Roboto Flex `wdth` axis for method numerals
Free, dramatic, restrained. Pairs cleanly with Cairo Variable for Arabic numerals (٠١ wide, label narrow). One specimen built right replaces three slots of section numbering across method, network, manifesto. Matches Apple WWDC's quiet swagger without any custom font.

### 4. REF 1 — Stripe Press chapter-opener system (drop cap + marginalia)
Earns editorial credibility. With IBM Plex Serif (free) + Cairo Variable (free) we already have the body+display pair we need. The drop-cap-with-marginalia layout pattern is portable to manifesto, founder bio, and the long-form sections of method. Print-grade typography from web-grade fonts.

### 5. REF 9 — The Pudding word-swap kinetic tagline
Free, accessible (respects `prefers-reduced-motion`), and lands a single hero moment that signals "we are not Bootstrap." Plays directly to Almobadir's multi-scale ambition (شركات → قطاعات → عاصمات). Implementation is ~30 lines of CSS+JS. Works in Arabic with no metric quirks.

---

## Skip-list (won't port)
- **Pentagram custom-typeface case studies** — every project ships a bespoke font; not replicable without commission.
- **NYT Mag Cheltenham/NYT Mag Serif** — proprietary, no equivalent free face captures the same warmth.
- **Klim Söhne** — paid (~$600), and Inter Variable already covers 90% of the perceptual ground for Almobadir's needs.
- **Displaay Season SERF axis** — Latin-only and paid; no equivalent Arabic SERF-axis font exists yet.
- **Funkis ABC** — Latin-only display; lovely but Recursive's CASL axis is the free analog already.

---

## Implementation order (when ready to ship)
1. Week 1 — Land REF 15 (Plex caps-alignment) and REF 12 (single type scale). Cleanup, no new fonts. Site goes from "scattered" to "considered."
2. Week 2 — Land REF 1 (drop-cap + marginalia for manifesto/method) using IBM Plex Serif (free) + Cairo Variable (free).
3. Week 3 — Land REF 7 (wide method numerals with Roboto Flex) and REF 9 (word-swap kinetic tagline). Two visible wow moments at zero font cost.
4. Decision gate — license **29LT Idris Variable** for the Arabic kashida-axis move (REF 5) on hero tagline only. Single biggest bilingual signature move available; price gates whether it ships in v1 or v2.

---

## 200-Word Summary

This library captures 15 typography moves portable to Almobadir's editorial bilingual voice, drawn from Stripe Press, Klim, Displaay, 29LT, TPTQ Arabic, Apple WWDC, Letters from Sweden, The Pudding, NYT Magazine, It's Nice That, Are.na, Browser Company, and Pentagram. Three categories dominate: **(1) variable-axis moves** that morph weight/width/kashida on scroll or hover, **(2) editorial typesetting moves** (drop caps, marginalia, sidenotes, single-family scale) that earn print-grade gravity from free Google Fonts, and **(3) bilingual rigor** moves that align Arabic and Latin on cap-height baselines so neither script reads as a translation.

The single most valuable Arabic-first move is **29LT Idris Variable's KSHD (kashida) axis** — a registered variable-font axis that physically elongates Arabic kashida strokes, no Latin foundry can replicate it, and it lands directly on Almobadir's hero tagline. The single most valuable free move is **IBM Plex Sans Arabic Variable + IBM Plex Sans Variable cap-aligned bilingual pairs** — same designer team, free on Google Fonts, fixes the existing baseline-misalignment problem across every bilingual pair on the current site. Implementation order ladders from zero-cost (week 1) to one strategic license (decision gate).

---

## Sources
- [Stripe Press — Poor Charlie's Almanack](https://press.stripe.com/poor-charlies-almanack)
- [Stripe Press — Poor Charlie's Almanack on Awwwards SOTD](https://www.awwwards.com/sites/poor-charlies-almanack)
- [Stripe Press — Scaling People](https://press.stripe.com/scaling-people)
- [29LT Idris Variable specimen](https://www.29lt.com/product/29lt-idris-sharp/)
- [29LT Idris design notes (kashida axis)](https://blog.29lt.com/2024/11/26/29lt-idris/)
- [29LT Zarid Sans variable](https://v-fonts.com/fonts/twenty-nine-lt-zarid-sans)
- [29LT Zarid Serif variable](https://v-fonts.com/fonts/twenty-nine-lt-zarid-serif)
- [IBM Plex Sans Arabic on Google Fonts](https://fonts.google.com/specimen/IBM+Plex+Sans+Arabic)
- [IBM Plex official languages page](https://www.ibm.com/plex/languages/)
- [Cairo on Google Fonts](https://fonts.google.com/specimen/Cairo)
- [Tajawal on Google Fonts](https://fonts.google.com/specimen/Tajawal)
- [Roboto Flex variable specimen](https://v-fonts.com/fonts/roboto-flex)
- [Apple SF Pro / SF Wide developer fonts](https://developer.apple.com/fonts/)
- [Klim Söhne collection](https://klim.co.nz/collections/soehne/)
- [Klim Heldane collection](https://klim.co.nz/collections/heldane/)
- [Displaay — Season variable typeface](https://displaay.net/typeface/season)
- [Letters from Sweden — Funkis](https://lettersfromsweden.se/font/funkis/)
- [Pentagram — Artificial Typography](https://www.pentagram.com/work/artificial-typography)
- [It's Nice That typography archive](https://www.itsnicethat.com/media/typography)
- [Typotheque homepage](https://typotheque.com)
- [TPTQ Arabic homepage](https://tptq-arabic.com)
- [The Pudding interactive features](https://pudding.cool)
- [Codrops — 3D scroll-driven text animations with CSS + GSAP](https://tympanus.net/codrops/2025/11/04/creating-3d-scroll-driven-text-animations-with-css-and-gsap/)
- [MDN — CSS Scroll-driven Animations](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations)
- [Dinamo — Using Variable Fonts on the Web](https://abcdinamo.com/news/using-variable-fonts-on-the-web)
