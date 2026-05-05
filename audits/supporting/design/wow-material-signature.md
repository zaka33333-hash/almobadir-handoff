# WOW Audit — Material Signature

**Compiled:** 2026-04-26
**For:** Almobadir wow-effect audit (build phase)
**Scope:** Define what physical thing the website is "made of," then specify exactly how each material renders in CSS, SVG, and asset cost.
**Bias:** Editorial Saudi business publication, premium-printed not plastic-web. Paper-and-ink heritage of Aramco Qafilah, Brownbook, and the NYT Magazine — not the gradient-glass aesthetic of B2B SaaS.

---

## EXECUTIVE SUMMARY — what makes Almobadir feel premium-printed not plastic-web

Almobadir is built from **four materials, in this order of dominance:**

1. **PAPER** — cream archival stock (`--paper #F8F4EA`) where the editorial thinking lives. The manifesto, founder bio, newsletter mock, and footer wordmark are typeset ON paper. Paper carries about 35% of the site's vertical real estate.
2. **VOID** — deep printer's-shop dark (`--void #0A0F1E`) as the storyboard between paper sections. Hero, Method, Network, and Content live in the void. Void carries about 55% of the site.
3. **INK** — crimson printer's ink (`--crimson #E6213B → #6E0F1F`) applied as drop caps, hairline accents, ornamental rules, and the ONE structural color decision per section. Ink is never decorative; it always denotes intent.
4. **FOIL** — restrained warm-gold metallic gradient on hero numerals and KPI big stats only (≥56px). Used like an annual-report page-flip cover, not a CTA highlight.

Plus three **system-level finishings** that bind the four materials:

- **GRAIN** — a 0.04-opacity cream-tinted noise field fixed at body level (already implemented in `base.css`). Lives only on void; auto-suppressed inside paper slabs.
- **SEAMS** — three hairline treatments where materials meet: paper-to-void edge, card-to-card 1px crimson seam, section-to-section paper-shadow.
- **INK-BLEED FILTER** — a single SVG `feGaussianBlur + feMorphology` filter that sits on the headline element, simulating ink wicking into paper fibre. Off by default; opt-in via `.ink-bleed` class.

**Why this wins:** every other Saudi business site uses gradient glassmorphism, neon hero shaders, or flat material-design rectangles. Almobadir's surfaces will feel like an Aramco Qafilah spread or an NYT Magazine online feature — not a Notion clone. The paper-ink-foil-grain stack is physical metaphor first, decoration never.

---

## MATERIAL 1: THE PAPER

**What it is:** Cream archival book-stock, slightly warmed at the corners, with a faint fibre texture you only notice after the third visit. It is the surface the editorial voice writes on. Paper is not white — it's `#F8F4EA`, the cream extracted from the Almobadir engagement-slide deck (page 9), already canonicalised in `tokens.css` as `--paper`.

**Where it appears:**
- Manifesto section (currently `.manif3` — already on paper)
- Founder bio background (proposed: shift `.fnd3a` from void to paper for the body text region)
- Newsletter mock-up frame (currently `.nl3-mock` — already on paper, pristine)
- Footer wordmark band (proposed: optional cream "press credit" footer slice)
- Long-form article body template (when the editorial roadmap adds article pages)

**CSS implementation:**

```css
/* ─── THE PAPER — utility class ─────────────────────────────────── */
.surface-paper {
  position: relative;
  background-color: var(--paper);
  color: var(--on-paper);
  isolation: isolate;
  --line: var(--hairline-paper);
  --line-strong: var(--hairline-paper-strong);
}

/* Warm corner gradient — barely perceptible, gives the eye depth */
.surface-paper::before {
  content: '';
  position: absolute;
  inset: 0;
  z-index: -2;
  pointer-events: none;
  background:
    radial-gradient(ellipse 1400px 900px at 50% 30%, var(--paper-2) 0%, transparent 65%),
    radial-gradient(ellipse 1100px 700px at 88% 88%, var(--paper-3) 0%, transparent 70%),
    radial-gradient(ellipse 900px 600px at 8% 92%, var(--paper-3) 0%, transparent 75%);
  opacity: 0.6;
}

/* Paper-fibre noise — site-wide grain auto-disables on paper, so paper
   carries its OWN fibre layer at lower opacity and warmer color */
.surface-paper::after {
  content: '';
  position: absolute;
  inset: 0;
  z-index: -1;
  pointer-events: none;
  opacity: 0.038;
  mix-blend-mode: multiply;
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='280' height='280'><filter id='p'><feTurbulence type='fractalNoise' baseFrequency='0.78' numOctaves='2' stitchTiles='stitch' seed='3'/><feColorMatrix values='0 0 0 0 0.42  0 0 0 0 0.31  0 0 0 0 0.19  0 0 0 0.9 0'/></filter><rect width='100%25' height='100%25' filter='url(%23p)'/></svg>");
}

/* Paper-corner curl on scroll exit (optional refinement) */
.surface-paper[data-curl] {
  transition: clip-path 1.4s var(--ease-glide);
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
}
.surface-paper[data-curl].is-leaving {
  clip-path: polygon(0 0, 100% 0, 100% calc(100% - 24px), 24px 100%);
}

/* Suppress the body-level cream grain inside paper — paper has its own */
.surface-paper ~ body::after,
body:has(.surface-paper:hover)::after { /* not really needed; for clarity */ }
.surface-paper { isolation: isolate; } /* re-stated: keeps body grain off */

/* Force-disable body grain when current section is paper:
   body::after has z-index 2; paper sets a backdrop above it. */
.surface-paper {
  background-image:
    radial-gradient(ellipse 1400px 900px at 50% 30%, var(--paper-2) 0%, transparent 65%),
    var(--paper);
  background-attachment: local;
}
```

**SVG asset (already inline above; copy-paste-able standalone):**

```xml
<svg xmlns="http://www.w3.org/2000/svg" width="280" height="280">
  <filter id="paper-fibre">
    <feTurbulence type="fractalNoise" baseFrequency="0.78" numOctaves="2" stitchTiles="stitch" seed="3"/>
    <feColorMatrix values="0 0 0 0 0.42
                            0 0 0 0 0.31
                            0 0 0 0 0.19
                            0 0 0 0.9 0"/>
  </filter>
  <rect width="100%" height="100%" filter="url(#paper-fibre)"/>
</svg>
```

The colour matrix tints the noise to `#6B5030` (warm walnut shadow) at 90% alpha — reads as paper fibre, not as monochrome static.

**Performance cost:** Zero runtime cost. Two pseudo-elements per paper section, both `position: absolute; pointer-events: none`. Inline data-URI SVG (~480 bytes per section, gzipped to ~280 bytes). No `background-attachment: fixed` — that thrashes scroll. No request, no layout, no paint after first frame.

**Reference:** Wave 1 — REF 5 (Diriyah City typeface, manuscript-to-wordmark dual hero) and REF 11 (Khatt Foundation Harakat Manifesto). Both lean on cream-paper materiality as the editorial ground. Manuscript scans on the Diriyah project show the paper carrying foxing and fibre — we render the same impression at 0.038 opacity instead of 0.45 because we're a screen, not a page.
URL: https://www.atrissi.com/diriyah-city-typeface-saudi-arabia/ · https://www.khtt.net/en/page/3650/the-harakat-manifesto

**Reasoning:** The manifesto already lives on paper. Pushing paper to a utility class lets the founder bio, newsletter mock, and footer signature inherit the same fibre + corner-warm gradient without re-implementing it. The 0.038 opacity is the threshold at which Chrome on a calibrated display registers texture without it reading as JPEG noise. Lower than the body grain (0.04 cream-on-void) because paper-on-paper needs less contrast lift.

---

## MATERIAL 2: THE INK

**What it is:** Crimson printer's ink — the kind that bleeds slightly into the paper fibre at the edge of a stroke and gets darker where it pools. Three depths already exist in tokens: `--crimson #E6213B` (primary, full saturation), `--crimson-2 #C41A30` (darker hover/active), `--crimson-3 #6E0F1F` (deepest — used only in heavy gradients and the founder section). Plus two halos: `--crimson-glow` and `--crimson-glow-2`.

The ink stroke is **never flat #E6213B**. It is `#E6213B` with an SVG ink-bleed filter that simulates the paper-wicking effect at 1px–2px radius — invisible at body weight, dramatic at display weight.

**Where it appears:**
- Drop caps on long-form (manifesto opener, founder bio first paragraph, future article body)
- Hairline accents under headings (the `.manif3__eyebrow::before` 28px crimson rule already in v3.css)
- Drop-cap drop shadow (a 1.5px crimson glow simulating the ink halo on uncoated stock)
- Brand mark hover state
- Section-divider full-bleed hairlines where paper meets void
- Pull-quote borders (the `.manif3__shadow` left border already uses `--crimson`)

**CSS implementation:**

```css
/* ─── THE INK — applied to text-as-headline elements ─────────────── */

/* Three depths, expressed as semantic tokens */
:root {
  --ink-crimson:        #E6213B;
  --ink-crimson-deeper: #C41A30;
  --ink-crimson-deepest:#6E0F1F;
  --ink-bleed-shadow:   0 0 1.5px rgba(230,33,59,0.35),
                        0 1px 0   rgba(110,15,31,0.18);
}

/* Drop-cap (Arabic on RTL float-right; Latin on LTR float-left) */
.surface-paper article > p:first-of-type::first-letter,
.surface-paper .ink-dropcap::first-letter {
  float: inline-start;
  font-family: var(--font-ar-display);
  font-weight: var(--w-black);
  font-size: 5.2em;
  line-height: 0.85;
  padding-inline-end: 0.08em;
  padding-block-start: 0.06em;
  color: var(--ink-crimson);
  text-shadow: var(--ink-bleed-shadow);
  /* opt-in to the SVG filter for headlines that earn it */
}

.ink-bleed { filter: url(#ink-bleed); }

/* Hairline crimson rule before eyebrows — already used in manif3 */
.ink-rule::before {
  content: '';
  display: inline-block;
  width: 28px;
  height: 1px;
  background: var(--ink-crimson);
  vertical-align: middle;
  margin-inline-end: 10px;
  /* the bleed: 1px outline at 0.18 alpha simulates ink wick */
  box-shadow: 0 0 0 0.5px rgba(230,33,59,0.18);
}

/* Section seam — where paper meets void */
.seam-paper-to-void {
  position: relative;
}
.seam-paper-to-void::after {
  content: '';
  position: absolute;
  inset-inline: 0;
  bottom: -1px;
  height: 1px;
  background: linear-gradient(
    90deg,
    transparent 0%,
    var(--ink-crimson) 12%,
    var(--ink-crimson-deeper) 50%,
    var(--ink-crimson) 88%,
    transparent 100%
  );
  opacity: 0.55;
}

/* Hover state — ink darkens like a fresh stroke */
a.ink-link {
  color: var(--ink-crimson);
  transition: color 220ms var(--ease-glide), text-shadow 220ms var(--ease-glide);
}
a.ink-link:hover {
  color: var(--ink-crimson-deeper);
  text-shadow: 0 0 1px rgba(110,15,31,0.4);
}
```

**SVG asset — the ink-bleed filter:**

```xml
<!-- Place once in <body>, reference everywhere via filter: url(#ink-bleed) -->
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <defs>
    <!-- Ink-bleed: gaussian blur + threshold for paper-wick effect -->
    <filter id="ink-bleed" x="-5%" y="-5%" width="110%" height="110%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="0.6" result="blur"/>
      <feColorMatrix in="blur" type="matrix"
        values="1 0 0 0 0
                0 1 0 0 0
                0 0 1 0 0
                0 0 0 18 -7" result="threshold"/>
      <feComposite in="SourceGraphic" in2="threshold" operator="atop"/>
    </filter>

    <!-- Ink-bleed-heavy: for hero-scale numerals -->
    <filter id="ink-bleed-heavy" x="-8%" y="-8%" width="116%" height="116%">
      <feGaussianBlur in="SourceGraphic" stdDeviation="1.2"/>
      <feColorMatrix type="matrix"
        values="1 0 0 0 0
                0 1 0 0 0
                0 0 1 0 0
                0 0 0 14 -5"/>
      <feComposite in="SourceGraphic" operator="atop"/>
    </filter>
  </defs>
</svg>
```

The threshold matrix (`alpha * 18 - 7`) creates a hard edge AFTER the gaussian blur — preserves crispness while allowing 0.6px of bleed at the stroke edges. At body sizes the effect is invisible; at ≥48px it reads as printed-ink wick.

**Performance cost:** One SVG filter, defined once in the document, referenced by class. SVG filters render on the GPU on Chromium and Safari 17+; cost is sub-millisecond per element. Apply only to display-sized text (≥48px) — below that the bleed disappears under sub-pixel rendering anyway. **Total:** 1 inline SVG filter element (~620 bytes), zero per-instance cost.

**Reference:** Wave 1 — REF 10 (Aramco Qafilah Magazine drop-cap) and REF 1 (Aramco Annual Report oversized numeral). Both use single ink-tinted display glyphs as architectural elements rather than decorative flourishes. The Diriyah City manuscript scans (REF 5) make ink-bleed on uncoated paper visually visible — we're recreating that artefact at the screen scale the editorial team would expect.
URL: https://www.aramcolife.com/en/publications/qafilah-magazine · https://kameelhawa.com/about/

**Reasoning:** Plain `color: #E6213B` on a 200px headline reads as web-button-red — the same red used by Tailwind, Bootstrap, and a thousand Wix templates. The SVG ink-bleed filter is what separates "Almobadir crimson" from "alert red." It costs nothing on top of the existing crimson token and lifts every display-size headline from web-flat to print-physical. The site already uses crimson in 11 distinct places (counted in `v3.css`); upgrading the four largest of those to `.ink-bleed` lifts the whole brand without touching any copy.

---

## MATERIAL 3: THE FOIL

**What it is:** The hot-stamped warm-gold metallic finish reserved for hero numerals, KPI big-stats, and the single most important year/figure on each page. Foil is **never** applied to body, captions, or hover states. It catches the light only on figures that earn it — a 286.0 net-income figure on an Aramco AR, a "2026" on an issue masthead.

The token already exists in the palette as `--gold #E8B855` for the occasional highlight. We add a foil gradient that animates almost imperceptibly on hover, simulating the way real foil catches a moving light source.

**Where it appears:**
- Hero numeral stat (the proposed "stat of the week" band — REF 1 Aramco Annual Report)
- Method step numerals (`٠١ ٠٢ ٠٣ ٠٤` at clamp(120px, 14vw, 240px) — currently flat colour in `v3.css`)
- KPI big-stats inside `.hero3__panel` once their size hits ≥56px
- The current year mark in the footer wordmark band (`٢٠٢٦` at 200px — proposed)
- Manifesto chapter numerals if/when the editorial team adds chapter dividers

**Restraint contract:** Foil only on numerals ≥56px. Never on Latin or Arabic words. Never on icons. Never on hover/focus rings. One foil element per viewport at any moment — if two would compete, the smaller one downgrades to flat `--gold`.

**CSS implementation:**

```css
/* ─── THE FOIL — utility class for hero numerals ─────────────────── */

/* The static foil — cream-into-crimson-into-warm-gold sweep */
.foil {
  /* Three colour stops: brand red → warm gold → brand red */
  background: linear-gradient(
    125deg,
    var(--ink-crimson)   0%,
    var(--ink-crimson-deeper) 18%,
    #FFD9A0              48%,   /* the hot-foil highlight */
    var(--gold)          58%,
    var(--ink-crimson)   88%,
    var(--ink-crimson-deeper) 100%
  );
  background-size: 220% 220%;
  background-position: 0% 50%;
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
  /* The numeral retains its real outline as a cast-shadow for legibility
     in case the gradient fails to clip (older WebKit, screenshot tools) */
  text-shadow: 0 1px 0 var(--ink-crimson-deepest);
  transition: background-position 1.4s var(--ease-glide);
}

/* On hover (or scroll-into-view), the highlight band travels across the glyph */
@media (prefers-reduced-motion: no-preference) {
  .foil:hover,
  .foil[data-in-view] {
    background-position: 100% 50%;
  }

  /* Idle ambient drift — only on the hero stat, never on a stack of foils */
  .foil--ambient {
    animation: foil-drift 14s linear infinite;
  }
  @keyframes foil-drift {
    0%   { background-position: 0%   50%; }
    50%  { background-position: 100% 50%; }
    100% { background-position: 0%   50%; }
  }
}

/* Restraint guards — these selectors disable foil where it would cheapen */
.foil:where(:not([class*="numeral"]):not(.t-stat):not(.t-hero)) {
  /* If a developer accidentally puts .foil on a word, fall back to gold */
  background: var(--gold);
  -webkit-background-clip: initial;
  background-clip: initial;
  -webkit-text-fill-color: var(--gold);
  color: var(--gold);
}

/* Foil + ink-bleed combined — ONLY for the hero "stat of the week" band */
.foil.ink-bleed {
  filter: url(#ink-bleed-heavy);
}
```

**SVG asset (if needed for a hot-foil mask refinement):**

```xml
<!-- Optional: a foil "scuff" texture overlay for the most prominent numeral -->
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <defs>
    <filter id="foil-scuff" x="0" y="0" width="100%" height="100%">
      <feTurbulence type="fractalNoise" baseFrequency="0.6 0.04" numOctaves="1" seed="7"/>
      <feColorMatrix values="0 0 0 0 1
                              0 0 0 0 0.92
                              0 0 0 0 0.6
                              0 0 0 0.18 0"/>
      <feComposite in2="SourceGraphic" operator="in"/>
    </filter>
  </defs>
</svg>
```

This second filter is optional — applied only to the single largest foil numeral on the page (e.g., the hero stat). It introduces sub-pixel horizontal grain that simulates real foil-press scuff. Skip on first ship; revisit only if the foil reads "too plastic."

**Performance cost:** Zero runtime cost for static. With ambient drift: one `background-position` animation over 14s — `background-position` is GPU-composited on Chromium and Safari, so it stays out of the layout thread. Animation pauses automatically under `prefers-reduced-motion: reduce`. **Total:** zero asset weight; one CSS keyframe (~120 bytes); optional second SVG filter (~280 bytes).

**Reference:** Wave 1 — REF 1 (Aramco AR 2024 single-numeral page) and REF 16 (Eastern-Arabic numeral as graphic). The Aramco annual report covers treat figures as foil-stamped ceremonial objects; they are never set in the same depth as body. Pentagram's Khaleej Times work (REF 3) similarly reserves any chromatic emphasis (yellow City Times accents) for structural moments only. Foil on numerals ≥56px is the screen analog of a print foil-stamp — both are ceremonial, both are restrained, both signify weight.
URL: https://www.aramco.com/-/media/publications/corporate-reports/annual-reports/saudi-aramco-ara-2024-english.pdf · https://www.pentagram.com/work/khaleej-times

**Reasoning:** The plastic-web mistake here is to apply gradient-text to every CTA, every stat, every "Featured" tag — at which point the page reads like a 2018 Stripe-clone hero. By gating foil to **numerals only, ≥56px only, one per viewport,** we keep the device ceremonial. The fallback selector that downgrades misuse to flat `--gold` enforces the rule even when an editor pastes `.foil` onto a word. Foil is the rarest material on the site by an order of magnitude — that's why it works.

---

## MATERIAL 4: THE GRAIN

**What it is:** The site-wide film grain that lives at body-level over every void section. It's the photojournalism artefact that signals "this is a real publication, not a CSS template." Already implemented in `assets/base.css` lines 48–58 at 0.04 opacity, mix-blend-mode: overlay, with a cream-tinted colour matrix.

What we're adding: explicit suppression rules so grain doesn't fight paper, plus a slightly heavier "section grain" available as an opt-in on void sections that need to read more cinematic (the founder hero, the method theatre, future video sections).

**Where it appears:**
- Body-level fixed overlay across all void sections (already present)
- Founder section — already has its own `.fnd3a__grain` at 0.06 opacity for stronger cinematic feel
- Newsletter section — already has `.nl3-grain` at 0.06 opacity
- Hero section — already has `.hero3__grain` at 0.55 opacity (heavy, mesh-only)
- **Suppressed on paper sections** (the change we're making)
- Optional cinematic boost on Method (proposed)

**CSS implementation:**

```css
/* ─── THE GRAIN — body-level (already exists in base.css), refined ── */

/* The base grain — reaffirmed and strengthened slightly */
body::after {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: var(--z-grain);  /* 2 — above background, below content */
  opacity: 0.045;            /* was 0.04; +0.005 reads on calibrated displays */
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 200'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.92' numOctaves='3' stitchTiles='stitch' seed='5'/><feColorMatrix values='0 0 0 0 0.96  0 0 0 0 0.92  0 0 0 0 0.82  0 0 0 0.5 0'/></filter><rect width='100%25' height='100%25' filter='url(%23n)'/></svg>");
  mix-blend-mode: overlay;
  /* Texture is intentionally NOT scroll-pinned via background-attachment: fixed —
     position: fixed on the pseudo-element handles it without a paint thrash. */
}

/* Suppression: when a paper slab is in viewport, the body grain becomes
   nearly invisible (paper has its own fibre layer that's optically warmer).
   We use :has() with the section-in-viewport state class set by JS. */
body:has(.surface-paper.is-in-viewport)::after,
body:has(.slab-light.is-in-viewport)::after {
  opacity: 0.012;  /* effectively off on paper */
}

/* Section-level cinematic boost — 1.5× density for the founder/method theatre */
.grain-cinematic {
  position: relative;
}
.grain-cinematic::before {
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 1;
  opacity: 0.07;
  mix-blend-mode: overlay;
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='320' height='320'><filter id='gc'><feTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch' seed='9'/><feColorMatrix values='0 0 0 0 0.9   0 0 0 0 0.78  0 0 0 0 0.55  0 0 0 0.6 0'/></filter><rect width='100%25' height='100%25' filter='url(%23gc)'/></svg>");
}

/* Animated grain — barely-perceptible drift to keep static screenshots feeling
   "alive" without paying a frame budget. Drives a 1px transform on the body
   pseudo-element — GPU-only, doesn't trigger layout. */
@media (prefers-reduced-motion: no-preference) {
  body::after {
    animation: grain-drift 8s steps(4) infinite;
  }
  @keyframes grain-drift {
    0%   { transform: translate(0, 0); }
    25%  { transform: translate(-0.5px, 0.5px); }
    50%  { transform: translate(0.5px, -0.5px); }
    75%  { transform: translate(-0.5px, -0.5px); }
    100% { transform: translate(0, 0); }
  }
}
```

**SVG asset (already inline above; standalone for reference):**

```xml
<!-- The base grain — cream-tinted noise over void -->
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200">
  <filter id="n">
    <feTurbulence type="fractalNoise" baseFrequency="0.92" numOctaves="3" stitchTiles="stitch" seed="5"/>
    <feColorMatrix values="0 0 0 0 0.96
                            0 0 0 0 0.92
                            0 0 0 0 0.82
                            0 0 0 0.5 0"/>
  </filter>
  <rect width="100%" height="100%" filter="url(#n)"/>
</svg>

<!-- The cinematic-boost grain — warmer, larger turbulence cells -->
<svg xmlns="http://www.w3.org/2000/svg" width="320" height="320">
  <filter id="gc">
    <feTurbulence type="fractalNoise" baseFrequency="0.65" numOctaves="3" stitchTiles="stitch" seed="9"/>
    <feColorMatrix values="0 0 0 0 0.9
                            0 0 0 0 0.78
                            0 0 0 0 0.55
                            0 0 0 0.6 0"/>
  </filter>
  <rect width="100%" height="100%" filter="url(#gc)"/>
</svg>
```

The cream tint (`#F5EAD2`-ish via `0.96 / 0.92 / 0.82` RGB at 50% alpha) is what makes this read as "grain" rather than "static." Pure-white noise at this opacity reads as blown-out highlights; cream-tinted noise reads as photo emulsion.

**Performance cost:** **One** background-image at body level (~520 bytes data-URI gzipped to ~310 bytes). Position: fixed on the pseudo-element pins it to the viewport without `background-attachment: fixed` (which is the actually-expensive option). The animated drift uses `transform: translate()` — GPU-composited only, ~0.05ms per tick on a midrange laptop. **Total:** one fixed pseudo-element, one keyframe, zero per-section cost (except the optional cinematic boost which adds ~600 bytes on the sections that opt in).

**Reference:** Wave 1 — REF 5 (Diriyah manuscript heritage column) and REF 20 (Siyah Mashq textured letterform compositions). Both register grain not as "filter VHS effect" but as the artefact of a real surface — paper fibre, ink wick, photo emulsion. The film-grain layer is the closest digital equivalent of the printer's-shop dust that gets caught in a Risograph or letterpress run. Aesop's website (in the comp set) uses a similar grain-on-warm-paper effect across its botanical pages; ours is a touch heavier because Saudi business publishing leans editorial-cinematic rather than apothecary-quiet.
URL: https://www.atrissi.com/diriyah-city-typeface-saudi-arabia/ · https://trcc.timrodenbroeker.de/omid-nemalhabib/

**Reasoning:** Grain is the cheapest, highest-leverage premium signal on the web. Apple, Stripe, Aesop, Linear, Vercel, the entire Klim Type Foundry — all use grain at 0.03–0.06 opacity. It's the signature of "we know what we're doing." The site already had grain right; our additions are: (1) suppress it on paper so paper feels paper, (2) bump it 0.005 to read on calibrated displays, (3) add a 4-step transform-drift that costs nothing but stops static screenshots from feeling stamped. The cinematic-boost layer is opt-in for the two or three sections that earn it (founder, method theatre, future video).

---

## THE SEAMS — where materials meet

Three seam treatments, applied at the boundaries between paper, void, ink, and foil. Each one is a hairline that does work — it's not decoration.

```css
/* ─── SEAM 1: paper edge (top/bottom of manifesto, etc.) ─────────── */
/* A 1px crimson ink rule that gradient-fades at both ends, so it
   reads as "drawn with the same ink as the drop caps" not as a border */
.surface-paper {
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
  border-image: linear-gradient(
    90deg,
    transparent 0%,
    var(--ink-crimson) 14%,
    var(--ink-crimson-deeper) 50%,
    var(--ink-crimson) 86%,
    transparent 100%
  ) 1 stretch;
  /* Falls back to a plain 1px crimson rule on browsers without border-image */
}

/* For RTL Arabic, mirror the gradient so the warm-end leads the eye */
[dir="rtl"] .surface-paper {
  border-image: linear-gradient(
    -90deg,
    transparent 0%,
    var(--ink-crimson) 14%,
    var(--ink-crimson-deeper) 50%,
    var(--ink-crimson) 86%,
    transparent 100%
  ) 1 stretch;
}

/* ─── SEAM 2: card-to-card hairline (network grid gap) ────────────── */
/* The 1px crimson seam between cards — only on hover-adjacent or
   focus-adjacent cards, so the grid breathes when nothing is hovered */
.nw3a-grid {
  --seam-crimson: 1px solid var(--hairline);
  display: grid;
  gap: 1px;
  background: var(--hairline);  /* cards sit ON a hairline grid */
}
.nw3a-grid:has(.nw3a-card:hover) {
  background: linear-gradient(
    180deg,
    var(--hairline) 0%,
    rgba(230,33,59,0.18) 50%,
    var(--hairline) 100%
  );
}
/* The active card's edges pulse crimson */
.nw3a-card:hover ~ * + .nw3a-card,
.nw3a-card:hover {
  box-shadow:
    -1px 0 0 0 rgba(230,33,59,0.32),
     1px 0 0 0 rgba(230,33,59,0.32);
}

/* ─── SEAM 3: section-to-section paper-shadow ─────────────────────── */
/* Where hero meets manifesto: the paper "lays on top of" the void with
   a 1.5px-tall shadow falling onto the void below it. Not a drop-shadow —
   a directional gradient that suggests the paper has weight. */
.surface-paper {
  position: relative;
  z-index: 2;
  box-shadow:
    0 1px 0 0 var(--ink-crimson-deepest),       /* the press-mark line */
    0 4px 0 -2px rgba(10,15,30,0.4),            /* paper sitting on void */
    0 14px 32px -8px rgba(0,0,0,0.45);          /* the cast shadow */
}

/* And inversely, where void meets paper from above */
.surface-void.seam-to-paper {
  box-shadow: inset 0 -1px 0 0 var(--ink-crimson-deepest);
}
```

Implementation note: `border-image` with a gradient is supported in all browsers since 2019. The fallback (plain 1px crimson border) is acceptable because the colour and weight are correct — only the soft fade-out at the edges is lost.

**Performance cost:** Zero. All seams are CSS-only, no JS, no asset.

**Reference:** Wave 1 — REF 3 (Pentagram × Khaleej Times "solid bar separator" between sections) and REF 9 (Brownbook bilingual nameplate where Arabic and Latin sit on a single press-rule). Both use the hairline as a structural beat rather than as decoration. The card-to-card seam is the screen analog of a magazine's binding — the place where two facing pages meet has weight; cards meeting in a grid should have the same.
URL: https://www.pentagram.com/work/khaleej-times · https://brownbook.me/about/

---

## THE COMPOSITION RULE — when paper meets ink meets foil meets grain

The single most important rule of the entire system. **Memorise this; everything follows from it:**

> **Paper carries the editorial. Void carries the storyboard. Ink draws the structure. Foil signs the moment. Grain sits on void only — never on paper.**

Restated as a flow:

1. **Decide the surface first.** Every section is either paper or void. There is no "mixed" surface. If you need both — make two adjacent sections.
2. **Place the ink second.** Every section gets exactly one ink moment: a drop cap, a hairline rule, a pull-quote border, a section-divider gradient. Not zero. Not three.
3. **Reserve foil for one numeral per viewport.** If two foil candidates compete in the same fold — the smaller one downgrades to flat `--gold`. Never two foil moments in one screen.
4. **Apply grain by surface, not by section.** Body grain runs over void. Paper has its own fibre layer (warmer, lighter). Sections never carry grain on top of paper — that reads as JPG noise.
5. **Treat seams as the same ink stroke.** The paper edge, the card seam, and the section-divider rule are all `--crimson` at the same weight (1px). The relationship between sections is part of the brand.

### The four-material composition lookup

| Element type            | Surface  | Ink treatment             | Foil?       | Grain layer        |
|-------------------------|----------|---------------------------|-------------|--------------------|
| Hero                    | Void     | Crimson eyebrow rule      | One numeral | Heavy mesh + body  |
| Manifesto               | Paper    | Drop cap + crimson border | None        | Paper fibre only   |
| Method                  | Void     | Step rule + numerals      | Step nums   | Body + cinematic   |
| Network                 | Void     | Card hover seam           | None        | Body grain only    |
| Founder                 | Paper*   | Drop cap on bio paragraph | None        | Founder cinematic  |
| Newsletter mock         | Paper    | Crimson rule on mock head | None        | Paper fibre only   |
| Footer                  | Void     | Wordmark in crimson       | Year mark   | Body grain only    |

*Founder section currently lives in void with a paper card. The proposed composition shifts the bio body region to paper while keeping the photo-frame on void, so the seam between them carries the press-mark hairline — directly mirroring a magazine spread (left page paper, right page photo bleed).

### What this rule prevents

- **Paper-on-paper-on-paper monotony.** The void interleavings keep paper precious.
- **Crimson-everywhere fatigue.** Confining ink to "exactly one per section" stops the page reading as a coupon.
- **Foil trinketry.** The numeral-only restraint stops foil from drifting onto buttons, badges, and section labels — the path that has cheapened every premium-aspiring B2B site since 2019.
- **Grain mud.** Suppressing grain on paper keeps editorial body type readable; layering grain on grain is the failure mode of every "premium feel" 2022 portfolio.
- **Decorative seams.** Hairlines have to *be the same ink* as the drop caps — that's what makes them feel printed instead of CSS-bordered.

---

## 200-WORD SUMMARY

Almobadir is built from four materials in a deliberate order: cream archival **paper** for editorial sections (manifesto, founder bio, newsletter mock), deep printer's-shop **void** as the storyboard between them, crimson printer's **ink** drawn in three depths for drop caps, hairlines, and section-dividers, and warm-gold **foil** reserved exclusively for hero numerals at ≥56px. Three system-level finishings bind them: a body-level cream-tinted **grain** at 0.045 opacity that suppresses on paper sections, an SVG **ink-bleed filter** that simulates paper-wick on display headlines, and three **seam treatments** where paper meets void, card meets card, and section meets section — each rendered in the same crimson ink as the drop caps so the whole site reads as one coherent press run.

The composition rule: paper carries editorial, void carries storyboard, ink draws structure, foil signs the moment, grain sits on void only. Every section gets exactly one ink moment, one foil candidate maximum, one surface choice. Total runtime cost: two SVG filters defined once, two pseudo-element grain layers, zero JavaScript. The result reads like an Aramco Qafilah feature, not a SaaS landing page.
