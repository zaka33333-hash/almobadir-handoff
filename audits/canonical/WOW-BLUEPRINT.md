# Almobadir — WOW Blueprint

> **The canonical synthesis.** 16 upstream reports collapsed into one ship-ready plan.
> Built on top of `PUNCH-LIST.md` (the forensic audit) — this layer is the WOW pass that takes the site from "very good" to "GitHub-tier wow and signature-ownable."
> Compiled 2026-04-27. Owner: Badr Shaker. Bar: 10/10.

---

## Table of contents

- [Executive summary](#executive-summary)
  - The signature (one paragraph)
  - The three system layers (typography / motion / material)
  - Top-10 ranked moves
  - Total budget and phasing
- [Part I — The Signature Layer (system-wide)](#part-i--the-signature-layer-system-wide)
  - 1.1 Typography signature
  - 1.2 Motion signature
  - 1.3 Material signature
- [Part II — Per-Section Moves (ranked)](#part-ii--per-section-moves-ranked)
  - Header / Hero / Manifesto / Network / Method / Content / Newsletter / Founder / Footer
- [Part III — The Top 10 (the bag-tag list)](#part-iii--the-top-10-the-bag-tag-list)
- [Part IV — Migration plan](#part-iv--migration-plan)
  - Phase 0 / Phase 1 / Phase 2 / Phase 3
- [Part V — Negative space (what we explicitly REJECT)](#part-v--negative-space-what-we-explicitly-reject)
- [Part VI — Verification checklist](#part-vi--verification-checklist)
- [Part VII — Open questions for the founder](#part-vii--open-questions-for-the-founder)

---

## Executive summary

### The signature

Almobadir's surface is **printer's-shop editorial, rendered in Arabic-first variable type, animated under Notion's discipline.** The page reads as cream paper interleaved with deep printer's void, lit by one crimson ink color that is reserved for type only, signed in foiled gold on numerals ≥56px, and bound by a paper-fibre grain that suppresses on cream. Every animation belongs to one of four named rituals (Reveal / Hover / Transition / Counter), all curves are five named bezíers with explicit owners, and all durations are three tiers (140 / 320 / 880ms). Every Arabic gesture is structurally Arabic — kashida elongation, RTL drop caps, Eastern Arabic numerals, cap-height bilingual alignment — never a Latin trick bolted onto RTL. The wordmark is path-outlined and immune to font 404. The result: the page reads as Aramco *Qafilah* online, not as a Notion clone.

### The three system layers

- **Typography** — five recurring gestures (Bilingual Eyebrow, Architectural Drop-Cap, Wide-Numeral Stat, Cream Pull-Quote with Crimson Em-Rule, Cap-Aligned Bilingual Wordpair) running on IBM Plex Sans Arabic Variable + Roboto Flex + Cairo Variable + Instrument Serif + JetBrains Mono Variable. Free fonts, ready today; brand fonts upgrade in place when they return. One Latin letter-spacing canon (0.22em); zero Arabic letter-spacing always. ~22hr migration.
- **Motion** — three durations (140 / 320 / 880ms), five curves (glide / out / spring / snap / counter), four rituals applied across ~80 surfaces. The forbidden middle (320–880ms) is rejected by definition. 27 distinct durations and 6 always-on infinite ornaments retire. ~8hr migration.
- **Material** — four materials in deliberate order (paper → void → ink → foil) bound by grain + seams + an SVG ink-bleed filter. One ink moment per section; one foil candidate per viewport; grain only on void; cream slab = quote slab. Two SVG filters defined once, two pseudo-element grain layers, zero JavaScript. ~6hr migration.

### Top-10 ranked moves (1-line each)

1. **Architectural Arabic drop-cap on long-form leads** (Manifesto / Founder / Method / Content) — the single Arabic-native typographic move; pure CSS. → Part II / Part III #1
2. **Real ⌘K command palette behind the rendered chip** (Header / global) — closes the loop on the kbd that already lies about itself; positions Almobadir alongside Linear/Vercel/Raycast. → Part III #2
3. **Tagline kashida-axis on accent words** (Hero) — the only Arabic-only structurally-impossible-in-Latin move on the entire research set; lands on the most-read element. → Part III #3
4. **Bilingual eyebrow + cap-aligned wordpair system** (typography Sig 1+5, all 8 sections) — one component, every section, one Latin canon and zero Arabic tracking. → Part III #4
5. **Three-duration / five-curve motion discipline** (system-wide) — collapses 27 durations into the brand's restraint signature; unlocks every other move. → Part III #5
6. **Paper material + SVG turbulence + ink-bleed filter** (system-wide) — base materiality on which Manifesto, Founder bio, Newsletter mock, Footer wordmark all lift. → Part III #6
7. **KPI counter roll + sparkline draw cascade** (Hero panel) — Stripe-grade tabular precision; "the network is alive" credibility proof. → Part III #7
8. **Sticky-card deck-stack + Aramco-style oversized issue numeral** (Content) — turns "competent grid" into "publication front page someone screenshots." → Part III #8
9. **Continuous SVG numeral path drawing across all 5 frames** (Method) — the section's signature gesture; "5 cards" → "one film reel." → Part III #9
10. **Lang-toggle masthead pin in destination language** (Header) — Hayy-Jameel positioning statement encoded in the chrome before any content loads. → Part III #10

### How to read this document

- **15-minute scan**: Read this Executive summary + Part III (Top 10) + Part IV (Migration plan total budget table). Decide on the three open questions in Part VII.
- **For engineering scoping**: Part I (signature contracts) + Part IV (per-phase deliverables + line-changed estimates).
- **For per-section work**: Part II cards point to the source `02-wow-*.md` files; the source files have full code, edge cases, RTL/PRM handling.
- **For PR review**: Part V (negative space) is the citation table — block any PR that violates a row.
- **For QA**: Part VI (verification checklist) gates each phase before the next starts.

### Total budget and phasing

**Phase 0 — Tokens & cleanup** (~1 day, ~8hr) — Token foundation for all three signature systems. Retire `--ease-in`, `--d-fast`, `--d-slow`, `--d-glide`. Add `--d-instant`, `--ease-spring`, `--ease-counter`. Load IBM Plex / Roboto Flex / Cairo / Instrument Serif / JetBrains Mono. Introduce `--paper-2/3`, ink-bleed filter, grain pseudo-elements. Global `:focus-visible` + `:active`. **Hard-blocks Phase 1.**

**Phase 1 — Signature systems** (~3 days, ~24hr) — Roll out Typography Sig 1–5 site-wide (eyebrow component, dropcap, stat, pull-quote, wordpair). Roll out Motion Rituals 1–4 (Reveal, Hover, Transition, Counter). Roll out Material composition rule (paper / void / ink / foil + suppressed grain on paper).

**Phase 2 — Per-section moves** (~5 days, ~40hr) — Top-10 from Part III, plus four more per-section "Top 3" anchors. Net: ~14 named moves across 9 sections.

**Phase 3 — Cinematic / optional** (~2 days, ~16hr) — Ambition tier. Hero → Manifesto Z-dolly, Founder magazine-cover treatment (gated on photo session), Method 3-cover stack (gated on real cover assets), debossed footer wordmark on cream sub-region (feature-flagged behind 5-session canary).

**Total core**: Phases 0+1+2 = ~9 working days ≈ 72hr. Add Phase 3 = ~88hr. With one 29LT Idris license decision (~€500) and one Khatt Foundation calligraphic-mark commission (~€200–400), the page ships at the targeted bar.

---

## Part I — The Signature Layer (system-wide)

This is the contract. Anything that ships on almobadir.com must trace back to one of these three layers. If a future change can't, the change is wrong, not the system.

Source: `wow-typography-signature.md`, `wow-motion-signature.md`, `wow-material-signature.md`. Read those for full token tables, CSS pseudocode, and rationale per spec. This Part is the consolidated brief; the source files are the technical contracts.

### 1.1 Typography signature

**The brand is bilingual. Every gesture must serve Arabic-Latin pairing — never one-sidedly. The brand fonts are 404 today; the public chain is IBM Plex Sans Arabic Variable + Roboto Flex + Cairo Variable + Instrument Serif + JetBrains Mono Variable (all free, all Google Fonts). Brand fonts upgrade in place without changing component shape.**

Five recurring gestures, repeated across every section:

1. **The Bilingual Eyebrow** (8 surfaces) — one `الكلمة · WORD · ٠٠١` component. Latin caps tracked at 0.22em (canon, replaces the current 5-tier chaos); Arabic tracked at 0 (always). Caps-baseline aligned via `transform: translateY(-0.06em)` on the Arabic child. Replaces 8 bespoke `.__eyebrow` blocks (~53 LOC).

2. **The Architectural Drop-Cap** (4 surfaces — Manifesto lede, Founder bio, Method body, Content featured) — RTL-correct `::first-letter` with `float: inline-start`, sized 5.5em, line-height 1.0 (Arabic descender safety), color `--crimson`, padded by `inline-end 0.12em`. Pairs with a 220px marginalia gutter (Stripe Press sidenote-as-canon). One selector handles both scripts via `:lang(en)` override.

3. **The Wide-Numeral Stat Frame** (6 surfaces) — Roboto Flex `wdth: 151` for Western digits; Cairo Variable Black for Eastern Arabic. Tabular-numeric, `--crimson` figure, mono caps label below. **Site-wide rule: one numeral system per section, locked.** Drives via `data-script="latn|arab"` attribute.

4. **The Cream Pull-Quote with Crimson Em-Rule** (4 surfaces) — every quote on the site sits inside a `--paper` slab. Latin in Instrument Serif italic (clamp 22→36px); Arabic in Cairo Variable Light (300 = the Arabic italic-equivalent register). 1px crimson rule on `border-inline-start`. Attribution in mono caps, 0.22em canon. Replaces 4 ad-hoc quote blocks.

5. **The Cap-Aligned Bilingual Wordpair** (5 surfaces) — IBM Plex Sans Arabic + Plex Latin (same designer team, metric-paired). Arabic child gets `transform: translateY(-0.08em)` (chip scale) or `-0.05em` (hero scale) so cap-heights align — not x-heights. Separator runs `unicode-bidi: isolate`. Same component handles `الشبكة / Network` (chip) and `المبادر / Almobadir` (hero) at any scale.

**The composition rule (memorize):**
- **Crimson is for typography only.** Status dot, dropcap, stat figure, pull-quote rule. Nothing else.
- **Mono Latin is the metadata register.** Eyebrow, stat-label, attribution. Never body. Never headline.
- **Arabic letter-spacing is always zero.** No exceptions; positive tracking destroys joining (audit C2).
- **Numerals declare their script per section.** Lock once.
- **Cap-alignment is the bilingual default.** Whenever Arabic and Latin appear inline, Arabic translateY-lifts.
- **Cream slab = quote slab.** The only place `--paper` appears on a dark page is inside a pull-quote.

Effort: ~22hr migration. Replaces ~165 LOC v3.css drift with ~240 LOC of one-system CSS. Per-signature: 4hr (eyebrow), 5hr (dropcap+marginalia), 6hr (stat), 3hr (quote), 4hr (wordpair).

Reference contract: `wow-typography-signature.md` (full spec including line-by-line v3.css migration table).

### 1.2 Motion signature

**3 durations, 5 curves, 4 rituals. The forbidden middle (320–880ms) is rejected by definition — anything between `--d-base` and `--d-cinematic` is excluded. This is the discipline that retires 27 distinct durations.**

**Three durations** (Notion REF 14):
- `--d-instant: 140ms` — hover, focus, micro-feedback, menu open
- `--d-base: 320ms` — state changes, card lift, reveal
- `--d-cinematic: 880ms` — entrances, scroll-driven, manifesto

Retires: `--d-fast`, `--d-slow`, `--d-glide`, plus 9 hand-typed values in the forbidden band (.4–.7s) and 18 others.

**Five curves** (each with an exclusive owner):
- `--ease-glide` `(.28,.11,.32,1)` — default, 90% of motion (Apple's signature curve)
- `--ease-out` `(.22,1,.36,1)` — exits only (departures asymmetric to entries)
- `--ease-spring` `(.34,1.56,.64,1)` — five named delight moments only (hero CTA active, network hover, newsletter success, footer back-to-top, manifesto period pop)
- `--ease-snap` `(.4,0,.2,1)` — symmetric state change (header is-scrolled, lang toggle, mega-menu)
- `--ease-counter` `(.215,.61,.355,1)` — numbers only (matches JS easeOutCubic)

Retires: `--ease-in` (declared, used 0 times — DELETE).

**Four rituals** (every animatable element belongs to exactly one):
- **Almobadir Reveal** — single rise-12px-fade-in entrance, 880ms glide, optional 60ms-per-row stagger via `--reveal-i`. Replaces 6 surfaces / ≥51 elements / 8 different rise distances.
- **Almobadir Hover** — single -2px lift, 140ms in (`--ease-glide`), 140ms out (`--ease-out` — asymmetric for ~30% faster perceived recovery), border-color + shadow + transform only (no background swap). `:active { scale(0.98) }` everywhere. Global `:focus-visible` ring (audit gap: 1 of 80). Consolidates 9 surfaces.
- **Almobadir Transition** — single between-section dim+drift on exit (opacity 1→0.6, translateY -24px), scroll-driven where supported. ONE cinematic Z-dolly handoff per page (Hero → Manifesto). Restraint is the brand.
- **Almobadir Counter** — single easeOutCubic ramp, 1400ms ± 200ms desync, once per session per element. Shared `almobadirCount()` JS module. CSS `--ease-counter` token analytically matches JS — single source of truth.

**Always-on infinite ornaments retired** (6 of them): hero pulse-ring, hero panel halo-spin, manifesto orbit, founder verified spin (×2), newsletter marquee. Inactive-frame pause via `:not(.is-active) *, :not(.in-view) * { animation-play-state: paused !important; }`.

Effort: ~8hr migration across 3 sprints. Sprint 1 (4hr): tokens + find-replace pass + global `:focus-visible` + `:active`. Sprint 2 (4hr): four ritual classes + migrate existing reveals to `[data-reveal]` + shared counter module. Sprint 3 (90 min): delete dead motion + pause inactive frames + reduce hero hue-drift to 18s.

Reference contract: `wow-motion-signature.md` (full token rationale, ritual specs, line-by-line migration table, negative-space citation rules).

### 1.3 Material signature

**Four materials in order of dominance: PAPER (35% of vertical real estate, cream `#F8F4EA` archival stock) → VOID (55%, `#0A0F1E` printer's-shop dark) → INK (`--crimson #E6213B` in three depths, applied as drop caps / hairlines / dividers — never decorative) → FOIL (warm-gold metallic gradient, numerals ≥56px only, one per viewport).**

Three system-level finishings:
- **GRAIN** — body-level cream-tinted fractal noise at 0.045 opacity, mix-blend-mode overlay, fixed pseudo-element. Auto-suppresses to 0.012 inside paper sections via `body:has(.surface-paper.is-in-viewport)::after`. Optional `.grain-cinematic` opt-in for Founder + Method theatre.
- **SEAMS** — three hairline treatments where materials meet. (1) Paper-edge: 1px crimson border-image gradient (transparent→crimson→deeper→crimson→transparent) at top/bottom of paper sections. (2) Card-to-card: 1px crimson seam between Network cards on hover-adjacent. (3) Section-shadow: paper-on-void cast shadow `box-shadow: 0 1px 0 var(--ink-crimson-deepest), 0 4px 0 -2px rgba(10,15,30,.4), 0 14px 32px -8px rgba(0,0,0,.45)`.
- **INK-BLEED FILTER** — single SVG `feGaussianBlur stdDeviation="0.6" + feColorMatrix threshold` defined once in `<body>`, referenced via `filter: url(#ink-bleed)`. Invisible at body weight, dramatic at ≥48px. Plus `#ink-bleed-heavy` for hero-scale.

**The composition rule (the single most important rule of the whole system):**

> **Paper carries the editorial. Void carries the storyboard. Ink draws the structure. Foil signs the moment. Grain sits on void only — never on paper.**

Restated as a flow:
1. Decide the surface first (paper or void; never mixed).
2. Place the ink second (every section gets exactly one ink moment — drop cap / hairline / pull-quote border / divider gradient. Not zero. Not three.).
3. Reserve foil for one numeral per viewport. If two compete, the smaller downgrades to flat `--gold`.
4. Apply grain by surface. Body grain runs over void. Paper has its own fibre layer (warmer, lighter).
5. Treat seams as the same ink. Paper edge, card seam, section divider all `--crimson` at 1px.

**Per-section composition**:

| Section    | Surface | Ink moment              | Foil?       | Grain layer        |
|------------|---------|-------------------------|-------------|--------------------|
| Hero       | Void    | Crimson eyebrow rule    | One numeral | Heavy mesh + body  |
| Manifesto  | Paper   | Drop cap + crimson rule | None        | Paper fibre only   |
| Method     | Void    | Step rule + numerals    | Step nums   | Body + cinematic   |
| Network    | Void    | Card hover seam         | None        | Body grain only    |
| Founder    | Paper*  | Drop cap on bio         | None        | Founder cinematic  |
| Newsletter | Paper   | Mock-head crimson rule  | None        | Paper fibre only   |
| Footer     | Void    | Wordmark in crimson     | Year mark   | Body grain only    |

*Founder shifts from current void-with-paper-card to true paper for the bio region; the seam between paper-bio and void-photo carries the press-mark hairline (mirrors a magazine spread).

Effort: ~6hr migration. Two SVG filters (~900 bytes total), two pseudo-element grain layers, zero JS. Performance: zero per-instance runtime cost; SVG filters are GPU-composited on Chromium and Safari 17+.

Reference contract: `wow-material-signature.md` (full per-material specs, SVG filter source, performance accounting).

### How the three layers compose

The three signature layers don't operate independently — they're calibrated to reinforce one another:

- Typography reserves `--crimson` for type only; Material reserves `--crimson` for ink-drawn structure (hairlines, seams, dividers); the overlap is intentional and consolidates the brand's red-mark inventory to one accent at one weight.
- Motion's Almobadir Reveal lifts content 12px while it fades in; Material's grain stays static — the moving element is the type, the still surface is the paper. This separation reads as "the page is being printed in front of you."
- Motion's `--ease-spring` is reserved for five surfaces (one of which is the manifesto period); Typography's pull-quote system (Sig 4) and Material's "one ink moment per section" both anchor the manifesto period as the climactic gesture. All three systems converge on the same beat.

**Conflict resolution**: Wave 2 footer MOVE 1 (debossed cream wordmark sub-region) inverts the footer surface to paper, which would put grain-on-paper and trip the suppression rule. **Resolution**: ship the footer cream sub-region as a `<section>` that activates `body:has(.surface-paper.is-in-viewport)`'s grain-suppression — the cream wordmark band reads as paper, the dark sitemap below it reads as void, the seam between them carries the press-mark. Feature-flag the cream inversion behind a 5-session canary (Part VI).

---

## Part II — Per-Section Moves (ranked)

Each card lists the section's TOP 3 moves from its `02-wow-*.md` source — pulled by ranked priority from each report's own ranking, plus a section-level signature line locking what this section uniquely contributes. Drill into the source for full code, dependencies, and edge-case handling.

### Header (`.hdr3`) — `index.html:40-143` · `v3.css:54-163` · `v3.js:10-70`

**Why this section needs it**: the chassis is 80% there (Apple-blur sticky, gradient ticker, mega panel, kbd chip already rendered) but every promise the markup makes is unkept — the chip doesn't flash, the search doesn't expand, the underline only fades, the ticker rotates without editorial labeling.

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Real ⌘K palette behind the rendered chip — fuzzy-search 16 actions, breadcrumb back-stack, kbd flash on press | 4hr | None | Closes the loop on the chip that already lies; positions Almobadir alongside Linear/Vercel/Raycast. **Source: `02-wow-header.md` MOVE 1.** |
| 2 | Lang toggle becomes the masthead pin (Hayy Jameel pattern) — single pinned word in destination language, View-Transition morph | 1.5hr | None | Editorial-Saudi positioning statement encoded in chrome before any content loads. Ref: arabic REF 19 + REF 9. **Source: MOVE 5.** |
| 3 | Logomark path-draws on first visit — stroke-dasharray glyph-by-glyph over 1.1s, sessionStorage gated, then settles | 1.5hr | None | Mark introduces itself once. Sets "this team thinks about every glyph" tone before the user reaches the hero. **Source: MOVE 3.** |

**Section-level signature**: the keyboard layer that makes the chrome feel like a tool, not a brochure.

### Hero (`.hero3`) — `index.html:148-308` · `v3.css:166-315` · `v3.js:72-112`

**Why this section needs it**: the most-loaded section; required to make 7 channels and 3.3M followers feel inevitable in three seconds. Chassis (mesh + grid + grain + KPI panel + chips + ticker) is strong; the gap is specificity.

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Tagline kashida-axis on accent words `الترفيه` / `التعليم` — variable-font `KSHD` axis stretches connecting strokes on scroll-into-view | 4–6hr | 29LT Idris license (~€500) OR Cairo Variable + letter-spacing fallback (70% of move) | The only Arabic-only move on the entire research set. Latin foundries cannot replicate. Lands on the most-read element. **Source: `02-wow-hero.md` MOVE 2.** |
| 2 | KPI counter roll-up + sparkline draw cascade — counts 0→3.3M in tabular-num lockstep, sparklines `stroke-dasharray` left-to-right at 120ms stagger | 5hr (combined MOVE 3+4) | None | "The network is alive" credibility proof. Stripe-grade tabular precision. **Source: MOVE 3 + MOVE 4 (ship as one).** |
| 3 | Wordmark stroke-draw "calligraphic ink" — SVG path-traced glyphs draw over 1.6s glyph-by-glyph then fill | 5hr | Stroke-trace 7 glyphs of المبادر from Mostaqbali (Glyphs/Illustrator) | The hero's first three seconds. Pure SVG + dasharray, no JS, no font license. **Source: MOVE 1.** |

**Section-level signature**: the moment the brand name *writes itself*, the network proves it's alive, and a single Arabic letter physically stretches across half a column.

### Manifesto (`.manif3`) — `index.html:310-362` · `v3.css:294-324`

**Why this section needs it**: Wave 2 audit called the cream-paper slab between two dark slabs "the strongest compositional move on the entire page." It is right composition with wrong materials — reads as cream on a screen, not paper on a desk.

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Paper-grain texture (SVG turbulence overlay) on `.manif3::before/::after` — activates dead `--manif3-paper-2` token; lays the material every other move on this list lands on | 2hr | None | Without it, drop cap / kashida / wax seal / corner curl all read as web tricks instead of printing. **Source: `02-wow-manifesto.md` MOVE 1.** |
| 2 | Words "ink in" via hand-drawn underline — pen-line draws RTL-correct under each word; word's ink resolves only after the pen passes; 55ms stagger. Simultaneously fixes Wave 2 critical #1 (`inline-block` ligature break), #2 (wrong-axis margin), #10 (unreadable scroll-driven reveal) | 4hr | None | One move, four fixes. The single largest perceptual upgrade on the manifesto. **Source: MOVE 2.** |
| 3 | Architectural Arabic drop cap on "نحن" — 4-line cap, contrasting weight, single crimson serif-tick, RTL `float: inline-start`, body wraps left | 2.5hr | None | Pure CSS. Promotes the brand's first word ("we") to *Qafilah*-spread architectural status. **Source: MOVE 5.** |

**Section-level signature**: the cream slab stops being a hex code and starts being a sheet.

### Network (`.nw3a`) — `index.html:364-477` · `v3.css:327-403` · `v3.js:142-173`

**Why this section needs it**: the strongest crafted module on the homepage already. Magazine grid, dual-language tag, gradient title-em, "best photographic decision on the entire page" (grayscale-on-rest → color-on-hover). Move is amplification *around* the photo treatment, not on it.

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Tabular-roll counters + flag-flip on `@almobadir_media` wide-card stats — `143+ / 5K+ / $1M+` roll Stripe-style; back-face Arabic label flips on hover | 3hr | None | The wide card's static strip is currently "data sheet" voice. Counters earn attention; flip-cards reveal context. Stripe + Browser Company in one motion. **Source: `02-wow-network.md` MOVE 5.** |
| 2 | Diagonal sweep reveal in keyboard order — 7 cards reveal in 60×30ms `--row/--col` ladder from top-leading corner (top-right in RTL) | 45min | None | Cheapest ship on the section. Replaces uniform 80ms-stagger with 2D ladder so cards light up from brand-correct corner. Raycast-grade. **Source: MOVE 4.** |
| 3 | Tighten 3D card tilt to ±4° with spring leave — replaces ±6° vanity with `cubic-bezier(0.34,1.56,0.64,1)` overshoot return | 1.5hr | None | Salvages the gesture by cutting the gimmick. Felt, not seen. **Source: MOVE 1.** |

**Section-level signature**: the editorial network reads as alive because its numbers move and its photos drift, while restraint keeps the magazine grid from becoming a parlor trick.

### Method (`.mtd3`) — `index.html:480-852` · `v3.css:407-597` · `v3.js:175-283`

**Why this section needs it**: the most ambitious part of the page. Commit 2 fixed the icon-numeral collision and HUD occlusion; the section now needs the *editorial cinema* layer that separates "scrollytelling demo" from "publication that takes itself seriously."

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Continuous SVG numeral path drawing across all 5 frames — single 4500px-tall hand-lettered Eastern-Arabic numeral string ٠١→٠٥ that draws as one connected stroke over scroll | 6hr | Illustrator commission for the connected numeral path (~3hr of ~6hr) | Transforms the section's metaphor from "5 stacked frames" to "one continuous editorial gesture." Apple PDPs can't ship this; Arabic-script-native. **Source: `02-wow-method.md` MOVE 1.** |
| 2 | Frame-to-frame color crossfade — `@property --mtd3-c` interpolates through every adjacent palette pair; cuts become dissolves over 240ms | 2hr | None | Cheapest 10/10-bar move on the list. Pinned stage stops feeling like 5 slides; starts feeling like one strip of film. **Source: MOVE 2.** |
| 3 | Sticky "current step" pill above HUD — `٠٢ · تبسيط` slides between rail positions with scroll | 2hr | None | Cinematic wayfinding. Earns the 4500px scroll budget. Pairs symbiotically with MOVE 1 + 2 (color follows the same crossfade triad). **Source: MOVE 3.** |

**Section-level signature**: a single ink stroke that draws itself across the entire scrollable theatre.

### Content (`.cnt3`) — `index.html:855-927` · `v3.css:601-661` · `v3.js:285-336`

**Why this section needs it**: the most editorially competent section on the page (accent-per-sub-brand, skeleton-to-photo reveal, hover accent-bar grow, staggered IO entrance). Three cards on a 1320px shell undersell the "+5,000 منشور across 6 platforms" claim. Move is restoring editorial volume.

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Sticky-card deck-stack — each card pins, next slides up over it, receding card scales 0.95 + dims; opt-in `.cnt3-grid--stack` activates at ≥6 cards | 4hr | Editorial-volume fix from `02-content.md` (6 cards minimum) | Reframes "three articles" into "an editorial deck being dealt." Single highest-impact move on the section. **Source: `02-wow-content.md` MOVE 1.** |
| 2 | Aramco-style "العدد ١٨٤" oversized issue numeral — replaces shy 12px mono `№ 184` with stacked masthead: tiny `العدد` over 64–88px Eastern Arabic numeral in display face | 1.5hr | None | The single typographic gesture that turns "competent grid" into "Arab publication front page." Lowest risk, biggest cultural-credibility return. **Source: MOVE 4.** |
| 3 | Spring-lift + saturated brand-color shadow on hover — Things-3 overshoot curve; coordinated lift + colored shadow (per-card `--accent`) + image zoom; single coordinated animation replacing 3 siblings | 1hr | None | Repairs "27 distinct durations" inside this section. The `--ease-spring` token's showroom. **Source: MOVE 2.** |

**Section-level signature**: the article cards become a portfolio deck dealt out one editorial spread at a time.

### Newsletter (`.nl3-section`) — `index.html:929-1002` · `v3.css:664-760` · `v3.js:338-399`

**Why this section needs it**: post-Wave-2 cuts (fake testimonial removed, Lorem placeholders gone), the section reads honest but not yet like a publication. Job for the next 90 days: **feel real until the first three issues exist.**

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Foil-stamp "نشرة المبادر" treatment on mock masthead — bumps brand name from 11→18px, metallic `background-clip: text` gradient, hairline rule above; HTML restructure flips hierarchy so brand leads, "عدد تجريبي" caps | 1.5hr | None | Single biggest credibility upgrade per hour. Mock reads as publication. Brownbook + Khaleej Times posture. Zero motion = zero PRM concerns. **Source: `02-wow-newsletter.md` MOVE 4.** |
| 2 | Drop cap on the lead deck — Qafilah-style first letter of `.nl3-mock-deck` at 4.4em, `float: inline-end`, `--crimson` ink-pool shadow | 1hr | None | After masthead reads as a publication, body needs to read as editorial. Single `::first-letter` rule. Wow per byte: highest. **Source: MOVE 3.** |
| 3 | Paper tilts on scroll progress — replaces vanity cursor-parallax with CSS `animation-timeline: view()` driving a 3° X-axis tilt tied to user's act of reading | 1.5hr | None | Motion has informational meaning, not ornamental. Paper "responds" to scroll. Ranked third because it depends on static composition feeling correct first. **Source: MOVE 1.** |

**Section-level signature**: the cream paper card becomes a quarterly's spine, stamped and folded as you read.

### Founder (`.fnd3a`) — `index.html:1005-1104` · `v3.css:763-873` · `v3.js:401-445`

**Why this section needs it**: Commit 1 cut vanity (verified-spin, AUTHORITY block); the cinematic budget freed by those deletions has not yet been spent. Three moves ship today with the AI-stylized 150×150 JPEG; one is photo-blocked; rest upgrade gracefully when the real shot arrives.

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Counter roll + name kashida pulse on viewport entry — `0 → 6 / 325 / 5,000` rolls in tabular-num lockstep while name `بدر شاكر` swells kashidas (letter-spacing + variation pulse) for 700ms | 2hr | None | Cinematic moment of arrival the section currently lacks. Replaces three lifeless never-changing stats. **Source: `02-wow-founder.md` MOVE 1.** |
| 2 | Khatt-tradition Arabic quote mark replaces « guillemet — hand-drawn *gulnar* rosette draws via stroke-dasharray over 900ms, ending exactly as quote text becomes legible | 1.5hr | None | Replaces borrowed European « with culturally-native mark. The detail Arabic-literate readers screenshot. Pure SVG; no font license. **Source: MOVE 3.** |
| 3 | Eastern-Arabic numerals in stat counters — swap `Math.round` for `Intl.NumberFormat('ar-SA-u-nu-arab')`; counter rolls keep their motion, digits arrive as ٦ / ٣٢٥ / ٥٫٠٠٠ | 30min | None | 30 minutes for cultural-authenticity-per-minute champion. Signals Arabic-first publication. **Source: MOVE 6.** |

**Section-level signature**: the founder's name pulses Arabic kashidas while his numbers roll and his quote opens with a mark his audience recognizes.

### Footer (`.ftr3`) — `index.html:1109-1202` · `v3.css:876-965` · `v3.js:447-502`

**Why this section needs it**: most operationally complete region on the site, but lacks signature — a single moment where a serious editorial Saudi business brand asserts itself instead of mimicking Stripe.

| # | Move | Effort | Dependency | Why |
|---|------|--------|------------|-----|
| 1 | Debossed wordmark on cream paper sub-region — replaces redundant 60-px CTA banner (audit #4) with a cream sub-region whose `المبادر` is pressed into the paper via multi-shadow inset trick | 5hr | **Feature-flag behind 5-session canary** (cream inversion of dark posture is the bold call) | Highest taste-per-hour ratio. Single most editorial moment on the entire site. Pays back 3 audit issues simultaneously (#4, #5, #8). **Source: `02-wow-footer.md` MOVE 1.** |
| 2 | Hover-construct on the wordmark (path-stroke decompose) — filled letterforms hollow into stroke + construction guides on hover; 700ms transition | 4hr | Wordmark must be path-outlined first (PUNCH-LIST P0 #9 already requires this for font-fidelity) | Cleanest "we know type" gesture. Pays back required path-outlining work. **Source: MOVE 2.** |
| 3 | "mubadir" type-the-phrase easter egg — global keystroke buffer; on match, footer wordmark cycles through 5 brand colors over 1.6s, clock pulse syncs, faint Arabic footnote types out below | 2hr | None | Pure delight, two hours, zero copy. Session-flag gated. Best surface for measuring "is anyone reading deeply?" via GA4 custom event. **Source: MOVE 3.** |

**Section-level signature**: the brand's last impression is a press-shop debossed wordmark on cream paper that rewards keyboard-curious readers.

---

## Part III — The Top 10 (the bag-tag list)

The 10 highest-leverage moves across the entire site, ranked by `(perceived craft lift) ÷ (engineering hours + asset risk + cross-system blast radius)`. Each is the move that, if missed, would leave the largest visible craft gap. Ship in this order.

### Quick-reference summary table

| Rank | Move | Section(s) | Effort | Asset / license dep | Phase |
|------|------|-----------|--------|---------------------|-------|
| 1 | Architectural Arabic drop-cap on long-form leads | Manifesto / Founder / Method / Content / Newsletter | 5hr (system) + per-surface | None | 1 + 2 |
| 2 | Real ⌘K command palette behind the rendered chip | Header (global) | 4hr | None | 2 |
| 3 | Tagline kashida-axis on Hero accent words | Hero | 4–6hr | 29LT Idris (~€500) OR Cairo fallback | 2 |
| 4 | Bilingual eyebrow + cap-aligned wordpair system | All 8 sections | 8hr | None | 1 |
| 5 | Three-duration / five-curve motion discipline | System-wide | 8hr | None | 0 + 1 |
| 6 | Paper material + SVG turbulence + ink-bleed filter | System-wide | 6hr | None | 0 + 1 |
| 7 | KPI counter roll + sparkline draw cascade | Hero panel | 5hr | None | 2 |
| 8 | Sticky-card deck-stack + Aramco-style issue numeral | Content | 5.5hr | 6 cards (editorial volume) | 2 |
| 9 | Continuous SVG numeral path across 5 frames | Method | 6hr | Illustrator commission | 2 |
| 10 | Lang-toggle masthead pin in destination language | Header | 1.5hr | EN site `/en/` exists | 2 |
| **Total** | — | — | **~57hr** | — | — |

The 10 moves total ~57 engineering hours; that's two-thirds of Phase 2 budget. The other ~3 hours of Phase 2 ship the per-section "Top 3" moves not in the global top-10 (e.g. Network MOVE 4 sweep reveal, Founder MOVE 1 counter+kashida pulse, Newsletter MOVE 4 foil-stamp masthead).

### Detailed entries

### #1 — Architectural Arabic drop-cap on long-form leads

**Section**: Manifesto (lede), Founder (bio), Method (per-frame body), Content (featured headline), Newsletter (mock deck) — 5 surfaces unified by one rule.
**Effort**: 2.5hr per surface (CSS) + ~5hr for the unified `editorial::first-letter` + marginalia gutter system from typography Sig 2.
**Why top 10**: This is the single Arabic-native typographic move on the entire research set. Latin sites can't replicate. Pure CSS, no JS, no font license — just `float: inline-start` + 5.5em sizing + crimson ink + a hairline rule under the cap. Replaces nothing; promotes the brand's first words ("نحن", "بدر", each step verb) to architectural status.
**Wave 1 reference**: Arabic REF 10 (Aramco *Qafilah* drop-cap as architecture, https://www.aramcolife.com/en/publications/qafilah-magazine) + Typography REF 1 (Stripe Press chapter-opener, https://press.stripe.com/poor-charlies-almanack) + REF 11 (It's Nice That two-color drop cap, https://www.itsnicethat.com).
**Risk / mitigation**: Arabic letter ن/ب must split cleanly from the joining word; verify with native Arabic renderer per surface. Mitigation: ship as a single `editorial::first-letter` utility class so the 5 surfaces share one verified implementation; spot-check each per Phase 2 sprint day.

### #2 — Real ⌘K command palette behind the rendered chip

**Section**: Header (global) — palette overlay introduced at Hero scroll level for first surfacing.
**Effort**: 4hr (`02-wow-header.md` MOVE 1) — includes 16-action seed list, fuzzy-search, breadcrumb back-stack stub, Arabic-first labels, `--ease-snap` for symmetric open/close.
**Why top 10**: The biggest "screenshot moment" available. The kbd chip is *already* in the DOM lying about itself — closing the loop between visible affordance and actual behavior is the single highest-leverage hour on the chrome. Positions Almobadir alongside Linear / Vercel / Raycast immediately. Battle-tested pattern (`cmdk` lib or vanilla).
**Wave 1 reference**: Interaction REF 1 (cmd-K palette + breadcrumb back-stack, https://linear.app + https://rauno.me/craft).
**Risk / mitigation**: Modal focus-trap and Esc dismissal must be airtight (a11y blocker if not). Mitigation: use Radix Command primitives or `cmdk` lib for tested ARIA; verify reduced-motion path skips backdrop blur and stagger.

### #3 — Tagline kashida-axis on Hero accent words

**Section**: Hero — tagline accent words `الترفيه` / `التعليم`.
**Effort**: 4hr if Idris license already secured; 6hr including license/integration. Cairo Variable + letter-spacing pulse covers 70% at zero font cost.
**Why top 10**: The only Arabic-only move on the entire research set. Latin foundries cannot do this. Lands on the most-read element on the most-loaded section. The variable-font `KSHD` axis physically elongates Arabic kashida strokes — a structurally Arabic gesture, not a Latin trick translated.
**Wave 1 reference**: Typography REF 5 (29LT Idris kashida axis, https://www.29lt.com/product/29lt-idris-sharp/ + https://blog.29lt.com/2024/11/26/29lt-idris/) + Arabic REF 8 (kashida as horizontal elongation, editorial breath).
**Risk / mitigation**: ~€500 license gates whether this ships in v1 or v2. Mitigation: Decision gate (Part VII Q1) — recommendation is ship Idris from day one because this is the only Arabic-structural wow on the list and the hero is where it belongs; Cairo Variable + `letter-spacing` fallback ships v1 if license is deferred.

### #4 — Bilingual eyebrow + cap-aligned wordpair system

**Section**: System-wide — 8 eyebrow surfaces (Header / Hero / Manifesto / Network / Method / Content / Newsletter / Founder / Footer CTA) + 5 wordpair surfaces (Hero, header brand, Network, Founder sub-eyebrow, Newsletter eyebrow).
**Effort**: 4hr (eyebrow component) + 4hr (wordpair component) = 8hr; replaces ~71 LOC of bespoke per-section eyebrow + bilingual rules with 80 LOC of one-system CSS.
**Why top 10**: The single largest typographic-coherence upgrade on the site. Replaces 5 different mono-eyebrow letter-spacings (audit C1) with one canon. Replaces x-height misalignment in every bilingual pair (audit F1) with cap-height alignment via `transform: translateY(-0.08em)` on the Arabic child. One markup, both directions, both scripts.
**Wave 1 reference**: Typography REF 15 (Typotheque + TPTQ Arabic, https://typotheque.com / https://tptq-arabic.com — caps-aligned bilingual specimen) + Arabic REF 9 (Brownbook nameplate, https://brownbook.me/about/ — Arabic ≥ Latin in bilingual masthead).
**Risk / mitigation**: 40+ Latin-inside-Arabic spans need `lang="en" dir="ltr"` wrappers (audit F1). Mitigation: introduce as Phase 1 work alongside the component CSS; one-pass markup sweep (~3hr) catches all sites.

### #5 — Three-duration / five-curve motion discipline

**Section**: System-wide — Sprint 1 of motion signature migration.
**Effort**: 4hr Sprint 1 + 4hr Sprint 2 (rituals) = 8hr core; Sprint 3 (90 min) deletes dead motion.
**Why top 10**: This is the constraint that makes every other move read deliberate. 27 distinct durations collapse into 3. 4 unused easing tokens become 5 with explicit owners. The forbidden middle (320–880ms) is rejected by definition. `:focus-visible` global ring fixes the audit gap of 1 of 80 interactive elements. `:active { scale(0.98) }` everywhere fixes the touch-feedback miss (audit §10). Without this, sections look polished individually but together still feel scattered.
**Wave 1 reference**: Motion REF 14 (Notion three-tier duration, https://rauno.me/craft/interaction-design) + REF 11 (Things 3 spring overshoot) + REF 4 (Linear 60ms-stagger row reveal).
**Risk / mitigation**: Mass find-replace across ~85 lines of v3.css needs visual diff per change. Mitigation: spreadsheet-style decision per line (hover/state → `--d-base`; reveal/entrance → `--d-cinematic`; micro-feedback → `--d-instant`); Sprint 1 verification list in `wow-motion-signature.md` §4 catches regressions.

### #6 — Paper material + SVG turbulence + ink-bleed filter

**Section**: System-wide — Manifesto, Founder bio, Newsletter mock, Footer wordmark sub-region (gated), plus all display-headline elements via `.ink-bleed`.
**Effort**: 6hr (Material signature migration). Includes paper utility class, two SVG filters (`#ink-bleed`, `#ink-bleed-heavy`), three seam treatments, body-grain refinement with paper-suppression via `:has()`.
**Why top 10**: The base material on which Manifesto MOVE 1 (paper-grain), Manifesto MOVE 5 (drop cap), Founder Move 7 (paper-grain shift), and Footer MOVE 1 (debossed wordmark) all land. Without it, those moves read as "web tricks on flat slabs" instead of as printing. Activates the dead `--manif3-paper-2` token (audit #16). The composition rule — paper carries editorial, void carries storyboard, ink draws structure, foil signs the moment — collapses dozens of per-section decisions into one principle.
**Wave 1 reference**: Arabic REF 5 (Diriyah City manuscript-to-wordmark, https://www.atrissi.com/diriyah-city-typeface-saudi-arabia/) + REF 10 (Qafilah magazine paper) + Typography REF 1 (Stripe Press paper).
**Risk / mitigation**: SVG `feTurbulence` renders ±5% differently between Chromium and Gecko. Mitigation: cap opacity at 0.55 max so variance reads as paper grain, not as static; verified pattern in `wow-material-signature.md`.

### #7 — KPI counter roll + sparkline draw cascade (Hero panel)

**Section**: Hero — KPI panel.
**Effort**: 5hr combined (`02-wow-hero.md` MOVE 3 + 4 ship as one).
**Why top 10**: The Hero's credibility argument is the KPI panel; counters that animate to 3.3M while their charts draw is the "this network is alive" proof. Stripe-grade tabular-num precision. Pairs naturally with Hero MOVE 6 (LIVE timestamp) for a triple-credibility hit. Synced animation: counters roll over 1400ms, sparklines draw at 120ms-staggered 700ms each, bars grow with `cubic-bezier(0.34,1.56,0.64,1)` overshoot at 60ms cascade.
**Wave 1 reference**: Interaction REF 3 (Stripe digit roll, https://stripe.com/pricing — `font-variant-numeric: tabular-nums`) + Motion REF 13 (GitHub Copilot stroke-dasharray draw, https://github.com/features/copilot) + REF 4 (Linear 60–120ms cascade).
**Risk / mitigation**: Western digits inside RTL flow need `direction: ltr` isolation; `tabular-nums` already handles it but verify on Safari + Chromium RTL.

### #8 — Sticky-card deck-stack + Aramco-style oversized issue numeral (Content)

**Section**: Content — `.cnt3-grid` (deck) + `.cnt3-head-right` (issue numeral).
**Effort**: 4hr (deck stack) + 1.5hr (issue numeral) = 5.5hr.
**Why top 10**: The single most "editorial publication" gesture available. The deck stack reframes "three articles" as "an editorial deck being dealt out one spread at a time." The Aramco-style `العدد ١٨٤` (clamp 48→88px Eastern Arabic numeral in display face) replaces the shy 12px mono `№ 184` with a quarterly's-spine masthead. Together: the section stops feeling like a Bootstrap card grid and starts feeling like a magazine front page.
**Wave 1 reference**: Motion REF 18 (Framer.com sticky stack, https://framer.com) + Arabic REF 1 (Aramco AR oversized numeral) + REF 16 (Eastern-Arabic numeral as graphic).
**Risk / mitigation**: Deck stack requires the editorial-volume fix from `02-content.md` (6 cards, not 3). Mitigation: opt-in `.cnt3-grid--stack` activates only at ≥6 cards; standard 3-card grid still ships if editorial pipeline lags.

### #9 — Continuous SVG numeral path drawing across all 5 frames (Method)

**Section**: Method — `.mtd3__track`.
**Effort**: 6hr engineering (3hr illustrator commission for the connected numeral path; 2hr engineering; 1hr palette/blend tuning).
**Why top 10**: The single move that transforms the section's metaphor from "5 stacked frames" to "one continuous editorial gesture." The eastern-Arabic ٠١→٠٥ flowing as one ink stroke is unmistakably Arab-editorial. No competitor in the audit set (Stripe Press, Linear, Bloomberg long-form) has script-native numeral choreography. The path is drawn against the actual stage height (5×100vh) via `--mtd3-thread-offset` driven from `update()`.
**Wave 1 reference**: Motion REF 13 (GitHub Copilot stroke-dasharray, https://github.com/features/copilot) + Arabic REF 14 (Iranian vertical kashida-strip marginalia).
**Risk / mitigation**: Path must align to stage height; if `--hdr-h` or HUD margin changes, numerals drift. Mitigation: percentage-based path coordinates, OR one SVG per frame with `viewBox="0 0 100 100"` if commissioning per-frame is preferred.

### #10 — Lang-toggle masthead pin in destination language

**Section**: Header — `.hdr3-lang`.
**Effort**: 1.5hr (`02-wow-header.md` MOVE 5) — includes one-line edit in `/en/index.html` to flip the destination + View-Transition morph.
**Why top 10**: Tiny technical change, large cultural-credibility signal. Most "international" sites bury the Arabic toggle and pay for it in bounce rate. Hayy Jameel and Misk Art Institute treat the toggle as a positioning statement encoded in chrome before any content loads. The label is set in the *destination* language ("EN" on AR pages, "العربية" on EN pages) so users see the language they'd be switching INTO. Pinned at top, not buried in the footer hamburger.
**Wave 1 reference**: Arabic REF 19 (Hayy Jameel / Misk Art Institute, https://hayyjameel.org/ + https://miskartinstitute.org/en) + REF 9 (Brownbook bilingual nameplate dominance).
**Risk / mitigation**: View-Transitions API is gated on Chromium 111+; Safari falls back to native page navigation. Mitigation: graceful upgrade pattern — `if (!document.startViewTransition) return;` lets default navigation handle it.

---

## Part IV — Migration plan

Sequenced in dependency order. Each phase ships independently and is reversible. PUNCH-LIST P0 / P1 fixes (`PUNCH-LIST.md`) ship in parallel with Phase 0 — this WOW blueprint assumes those credibility cuts (fake testimonial, +50K claim, fake archive, hardcoded ticker, brand-font 404 resolution, mega-menu DOM bug) have shipped before WOW work begins. **Do not start Phase 0 until PUNCH-LIST P0 #1, #2, #9, #26, #27, #28 are closed.**

### Phase 0 — Tokens & cleanup (~1 day, ~8hr)

The token foundation that all three signature systems depend on. Hard-blocks Phase 1 — without these, Phase 1 components compile but reference symbols that don't exist or that hold the wrong values.

**Deliverables**:
- `tokens.css`: replace `--ease-in / --d-fast / --d-slow / --d-glide` with `--ease-out / --ease-spring / --ease-snap / --ease-counter / --d-instant / --d-base / --d-cinematic` (motion sig §1). Keep `--ease-glide`. Add `--paper-2 / --paper-3` plus `--ink-bleed-shadow` + `--gold` (already there) for material sig.
- `fonts.css`: load IBM Plex Sans Arabic Variable + IBM Plex Sans Variable + Roboto Flex + Cairo Variable + Instrument Serif + JetBrains Mono Variable from Google Fonts; subset to ar+latn ranges via `unicode-range` (target: ≤380KB total).
- `base.css`: append global `:focus-visible { outline: 2px solid var(--crimson); outline-offset: 4px; border-radius: inherit; }` + global `:where(button, a, [role="button"]):active { transform: translateY(0) scale(0.98); transition: transform 80ms var(--ease-snap); }`. Drop the v3.css selector-specific reduced-motion list (lines 1199–1210); the `*` blanket in base.css covers it.
- Two SVG filters in `<body>`: `#ink-bleed` and `#ink-bleed-heavy` (material sig §INK).
- Find-replace pass on v3.css across ~85 lines mapping legacy durations to the 3-tier system (motion sig §4 Sprint 1 spreadsheet).

**Lines changed**: ~120 LOC across `tokens.css`, `fonts.css`, `base.css`, plus ~85 line revisions in `v3.css`.
**Files touched**: `tokens.css`, `fonts.css`, `base.css`, `v3.css` (mass find-replace), `index.html` (single inline-SVG insertion for filters).
**Rollback**: full git revert of the 5-file commit. No DOM structural changes; no breaking-change risk.

### Phase 1 — Signature systems (~3 days, ~24hr)

Roll out Typography Sig 1–5 + Motion Rituals 1–4 + Material composition rule. After Phase 1, every section visually inherits the brand's typographic, motion, and material posture even before per-section moves land in Phase 2.

**Day 1 — Typography (8hr)**: Add `.eyebrow`, `.editorial::first-letter`, `.stat-fig[data-script]`, `.pull-quote`, `.wordpair` components to v3.css (~245 LOC). Refactor 8 eyebrow markup sites with `lang` + `dir` attributes. Refactor 5 wordpair sites. Mark up the 4 dropcap sites with `class="editorial"`. Mark up 6 stat sites with `data-counter` + `data-script`. Mark up 4 pull-quote sites with `figure class="pull-quote"`.

**Day 2 — Motion (8hr)**: Sprint 2 of motion migration. Add the 4 ritual classes to v3.css. Migrate existing reveals to `[data-reveal]` (delete per-selector translateY blocks; add `--reveal-i` ladder; replace 6 surfaces / ≥51 elements). Move counter logic to shared `almobadirCount()` module. Add `.section-glide` to 6 of 7 sections. Hero panel gets `.almobadir-handoff` (the cinematic Z-dolly).

**Day 3 — Material (6hr) + buffer (2hr)**: `.surface-paper` utility class + `body::after` grain refinement with `:has()` paper-suppression + three seam treatments (paper-edge gradient border, card-to-card hover seam, paper-on-void cast shadow). Apply `.surface-paper` to Manifesto, Founder bio region, Newsletter mock, optional Footer cream sub-region. Apply `.ink-bleed` to display headlines (≥48px). Apply `.foil` to numerals ≥56px (Hero stat band, Method giant numerals, Footer year mark).

**Lines changed**: ~700 LOC net (~245 typography + ~120 motion ritual classes + ~150 material classes + filters + ~185 markup wrappers).
**Files touched**: `v3.css` (heavy), `index.html` (markup wrappers), `v3.js` (counter module migration only).
**Rollback**: per-day commits keep each day reversible. After Day 1, the typography components can be partially adopted (component CSS exists; sites adopt as ready). After Day 2, motion rituals coexist with legacy transitions during migration. After Day 3, paper sections coexist with void.

### Phase 2 — Per-section moves (~5 days, ~40hr)

Top-10 from Part III, plus the remaining ranked moves from each section's "Top 3". Sprint cadence: ship 1–3 moves per day, verify via Part VI gate before next-day work.

**Day 4** (~8hr): Top 10 #1 (drop-cap ship across 5 surfaces) + #10 (lang-toggle masthead pin). The two pure-CSS, lowest-risk Part III moves. Net: 2 moves, 5 sections touched.

**Day 5** (~8hr): Top 10 #2 (⌘K palette) + #4 (eyebrow + wordpair refactor markup pass). The chrome layer. Net: 2 moves, header + 8 eyebrow surfaces + 5 wordpair surfaces.

**Day 6** (~8hr): Top 10 #3 (Hero kashida-axis) + #7 (KPI counter + sparkline cascade). The Hero's two flagship moments. Net: 2 moves, hero only. **Decision gate**: Idris license vs Cairo fallback (Q1 in Part VII).

**Day 7** (~8hr): Top 10 #6 (Material paper + ink-bleed already shipped Phase 1) anchors Manifesto MOVE 1+2+5 (paper-grain + ink-in underline + drop-cap on نحن) + Network MOVE 5 (tabular-roll counters + flag-flip). Net: 4 moves across 2 sections.

**Day 8** (~8hr): Top 10 #8 (Content deck-stack + issue numeral) + #9 (Method continuous numeral path). Net: 2 moves, but #9 includes illustrator commission async — schedule the brief at start of Phase 1.

**Lines changed**: per-section deltas vary. Header: ~120 LOC. Hero: ~180 LOC. Manifesto: ~95 LOC. Network: ~140 LOC. Method: ~60 LOC + SVG asset. Content: ~110 LOC. Total Phase 2: ~705 LOC across `v3.css`, `v3.js`, `index.html`.
**Files touched**: `v3.css` (extensive), `v3.js` (~150 LOC additions), `index.html` (markup), new SVG assets in `assets/illustrations/`.
**Rollback**: each day's moves are wrapped in feature classes (e.g. `.hdr3-cmdk-root`, `.cnt3-grid--stack`); removing the class on a parent disables the move without code revert.

### Phase 3 — Cinematic / optional (~2 days, ~16hr)

The ambition tier. Ship behind feature flags or asset gates — none of these block the Phase 1+2 release.

**Day 9** (~8hr): Hero → Manifesto Z-dolly handoff (motion sig Ritual 3). Founder magazine-cover treatment (Founder MOVE 8 — code shovel-ready, gated by `data-coverart="false"` until 1600×2000 portrait lands). Method 3-cover stack (Method MOVE 8 — gated until 3 real cover JPGs are commissioned).

**Day 10** (~8hr): Footer debossed cream wordmark (Footer MOVE 1 — feature-flagged behind 5-session canary, see Part VI). Footer wormhole back-to-top (MOVE 4). Method per-frame paper-grain shift (Method MOVE 6) + magnetic Calendly card on Founder (MOVE 5).

**Lines changed**: ~250 LOC + asset work (3 cover JPGs at 600×800 WebP, 1 founder portrait 1600×2000, 5 hand-drawn editorial illustrations for Method MOVE 7).
**Rollback**: every Phase 3 move is gated by either a feature flag, a `data-*` attribute, or an asset presence check. Removing the asset/flipping the flag reverts visually with zero code change.

### Cross-phase dependencies

| Phase | Blocks | Blocked by |
|-------|--------|------------|
| Phase 0 (tokens + cleanup) | Phases 1, 2, 3 | PUNCH-LIST P0 #1 (font 404), #9 (wordmark path-outline), #26-28 (credibility cuts) |
| Phase 1 (signature systems) | Phase 2, 3 | Phase 0 |
| Phase 2 (per-section moves) | Phase 3 (some moves) | Phase 0, Phase 1 |
| Phase 3 (cinematic / optional) | None | Phase 0, Phase 1, Phase 2 + asset commissions (founder portrait, 3 covers, 5 illustrations) + Idris license decision |

### Per-phase verification

- After Phase 0: visit hero → confirm hover snappier (Sprint 1 tokens land). Tab through every interactive → confirm crimson focus ring. Toggle Reduce Motion → confirm nothing animates but everything readable.
- After Phase 1: scroll page top to bottom once → every section's content rises+fades in. Hover every `[data-reveal]` after first reveal → does not re-trigger. Eyebrow + wordpair pairs render with caps-aligned bilingual.
- After Phase 2: visual diff vs `design-audit/screenshots/`. Bilingual proof every `.wordpair` rendered chip + hero in both `dir`. Audit-compliance: `--lh-hero` ≥ 1.0 (B1), zero positive letter-spacing on Arabic (C2), one numeral system per section (K1), bidi-isolated separators (F1).
- After Phase 3: 5-session canary on each feature-flagged move. Decision per move: ship to default, hold behind flag, or revert.

### Total budget

| Phase | Hours | Critical-path days | What ships |
|-------|-------|--------------------|------------|
| Phase 0 | 8 | 1 | Tokens, fonts, two SVG filters, global focus + active rings, ease-in deletion, 27→3 duration collapse |
| Phase 1 | 24 | 3 | All five typography signatures, all four motion rituals, material composition rule with paper/grain/seams |
| Phase 2 | 40 | 5 | Top-10 (drop-cap, ⌘K, kashida, eyebrow+wordpair, KPI counters, deck-stack, numeral path, lang-pin) |
| Phase 3 | 16 | 2 | Hero→Manifesto Z-dolly, Founder magazine cover, Method 3-cover stack, Footer cream sub-region (canary) |
| **Total core** | **88** | **11 working days** | — |

Asset / license costs:

| Item | Cost | Phase | Required? |
|------|------|-------|-----------|
| 29LT Idris Variable license | ~€500 one-time | 2 (Day 6) | Recommended; Cairo fallback ships v1 |
| Khatt Foundation calligraphic mark commission | ~€200–400 | 3 (deferred) | Optional; Manifesto MOVE 7 placeholder ships |
| Founder studio portrait (1600×2000) | photo session | 3 (deferred) | Bridges via Founder MOVE 2 duotone thaw |
| Method illustrator (5 plates, unified style) | ~14hr commission + ~€800 fee | 3 (deferred) | Lo-fi 4hr SVG cleanup ships interim |
| Almobadir issue covers (3 × WebP 600×800) | editorial-pipeline | 3 (deferred) | Method MOVE 8 placeholder ships |

**Recommended ship sequence**: PUNCH-LIST P0 (week 0) → Phase 0 (day 1) → Phase 1 (days 2–4) → Phase 2 first half (days 5–6, ships top-10 #1, #2, #4, #5, #6, #10 — the systemic + chrome layer) → Phase 2 second half (days 7–9, ships top-10 #3, #7, #8, #9 — the per-section flagship moments) → Phase 3 (days 10–11, gated cinematic). At end of day 9, the page reads at the targeted bar; Phase 3 is icing.

---

## Part V — Negative space (what we explicitly REJECT)

This list is enforceable. PR comments, design reviews, and editorial briefs can cite a row. Restraint is the brand.

### Motion rejections (from `wow-motion-signature.md`)

| Pattern | Why rejected |
|---------|--------------|
| Bouncy spring on body text or paragraphs | Springs on text caricature editorial gravitas. `--ease-spring` is reserved for 5 named card/CTA surfaces only. |
| Page-driven parallax (multi-layer translateY at differing rates) | Reads as 2018 portfolio. Almobadir is editorial; depth comes from typographic hierarchy, not from layers sliding at 0.3/0.6/0.9. |
| Glitch / VHS / RGB-split filters | "Tech-bro / cyberpunk" register. Off-brand. The one lo-fi-tech moment permitted is the text-scramble (motion REF 15) on hero h1 once per session. |
| Swipe-reveal or curtain-wipe section transitions | Hard cuts disguised as transitions. Reads as PowerPoint. The brand has one section transition: `.section-glide`. |
| Persistent infinite-loop ornaments (orbit spinners, rotating verifieds, halo rings) | Audit §1: 6 always-on ornaments retired. Motion serves a function or it doesn't run. |
| Animation on `width`, `height`, `min-width`, `backdrop-filter` | Layout-thrash. Re-express as `transform: scaleX/scaleY` or accept the property change as instant. |
| Asymmetric durations (different in/out times on the same property) | Curves can be asymmetric; durations cannot. |
| Hover transitions ≥ `--d-base` (320ms) | Hover is high-frequency (Notion REF 14). The forbidden middle (320–880ms) is *especially* forbidden on hover. |
| Custom JS scroll engines (Locomotive, GSAP ScrollSmoother, Pjax) | Lenis (3kb) is the only permitted smooth-scroll. Heavier engines break the load budget. |
| `ease-in` curves (slow start, fast end) | Things ease into the viewport; the brand has no surface that exits in primary attention. `--ease-in` is deleted. |
| Bouncing scroll cues / jiggling chevrons / tilting attention-grabbers (>2°) | The bouncing arrow at `mtd3:421` is the *one* allowed jiggle (semantic navigation affordance). No copies. |
| 3D card tilt on touch devices | Already correctly gated; never translate to gyroscope events. |
| Counters that animate on every viewport re-entry | Once per session per element. `obs.unobserve` after first fire is mandatory. |
| Cursor trails / pixel auras / sparkle particles | The custom-cursor pattern (interaction REF 7) caps at cursor + lerp + label-on-hover. Sparkle emits rejected. |

### Typography rejections (from typography signature + audit findings)

| Pattern | Why rejected |
|---------|--------------|
| Synthetic Arabic italic (`font-style: italic` on Arabic glyphs) | Browsers slant Arabic glyphs that have no real italic. Native readers reject on sight. (PUNCH-LIST #17.) Use weight, color, or size to differentiate. |
| Positive `letter-spacing` on Arabic content | Destroys joining contour. Audit C2: hard rule. Arabic letter-spacing is always 0; no exceptions. |
| All-caps Arabic | Arabic has no uppercase. `text-transform: uppercase` on Arabic is a parse-mismatch and renders as no-op visually but misleads code review. |
| Latin-style word-spacing justification on Arabic paragraphs | Produces gappy "rivers." Use kashida justification (`font-feature-settings: "jstf"`) on fonts that support it (arabic REF 11), or accept ragged-end. |
| Mixing Eastern Arabic and Western digits within the same section | Audit K1, K2. Pick one numeral system per section and lock it. |
| 5+ different mono-eyebrow letter-spacings | Audit C1: today the site has 5. The canon is `0.22em` Latin-only. |
| Bilingual pairs aligned on x-height instead of cap-height | Audit F1. Cap-height alignment is the brand's bilingual default (typography Sig 5: `transform: translateY(-0.08em)` on the Arabic child). |
| Hardcoded font-sizes that bypass the type scale | 237 hardcoded `font-size` literals in v3.css today (audit). The type scale is `--t-2xs..6xl` on a 1.250 ratio; component CSS consumes tokens. |
| Crimson at sub-AA contrast on white CTA | `#E6213B` on `#FBF7EE` = 4.23:1, fails WCAG 1.4.3. CTAs must use `--crimson-2 #C41A30` (5.46:1) or pure white (4.66:1). |

### Material rejections (from `wow-material-signature.md`)

| Pattern | Why rejected |
|---------|--------------|
| Gradient glassmorphism / frosted-glass cards on dark | "B2B SaaS clone" signal. Almobadir surfaces are paper / void / ink / foil — not transparent material. |
| Skeuomorphic shadows (drop-shadow on `<button>` simulating raised plastic) | Plastic-web register. The brand uses `box-shadow` only for paper-on-void cast shadows and on hover-lift (Ritual 2: `--e-2`). |
| `backdrop-filter: blur` on paper sections | Paper has its own fibre layer. Glass blur over paper reads as a screenshot effect, not material. |
| Foil on words, icons, or hover/focus rings | Foil is for numerals ≥56px only, one per viewport. The fallback selector downgrades misuse to flat `--gold`. |
| Crimson on non-typographic surfaces (decorative crimson buttons, crimson backgrounds, crimson lines that aren't structural seams) | Crimson is for type and seams only. Every red mark in the system must be type-related or paper/card-edge. |
| Grain on grain (body grain stacking on section-cinematic grain stacking on photo grain) | Mud. Body grain runs over void; paper has its own fibre; sections opt into cinematic boost only when the surface is void. Never layered on paper. |
| Decorative seams (border-radius rules, dotted borders, double rules) | Hairlines must be the same crimson ink as drop caps. CSS `border` defaults are rejected; use `border-image` linear-gradient or `box-shadow inset` matching the ink palette. |
| `background-attachment: fixed` for grain | Scroll thrash. Use `position: fixed` on the pseudo-element. |

### Editorial rejections (from PUNCH-LIST + brand voice findings)

| Pattern | Why rejected |
|---------|--------------|
| Vanity-engineered claims the audience can verify in 10 seconds | PUNCH-LIST P0 #26-28: fake testimonial, +50K subscriber claim, 184 fake archive chips. Saudi business is small; fabricated numbers kill credibility. Honest small numbers convert. |
| Self-applied verified badges or rotating "VERIFIED" marks | PUNCH-LIST #32. Trust comes from named third-party authority (Favikon rank, publication mentions), never from a self-awarded mark. |
| Half-implemented bilingual toggles that animate but don't swap content | PUNCH-LIST #6, #35. Half-implementation is worse than absence. Wire the toggle to a real i18n table or remove until EN site exists. |
| Editorial voice schizophrenia (meta says X, manifesto says not-X) | PUNCH-LIST #45. Pick one. Site contradicts its own positioning above the fold. |
| 5 different labels for the same CTA | PUNCH-LIST #46. One verb, one phrase site-wide. |

**Citation rule**: future PRs adding any pattern above are blocked with a link to the row. No debate.

---

## Part VI — Verification checklist

### Pre-deploy gate (every phase)

Run before any phase ships to production. All items must pass.

- [ ] **axe-core scan** clean (`npx @axe-core/cli http://localhost:8765/`). Zero violations on home page.
- [ ] **Lighthouse desktop ≥ 95** on Performance, Accessibility, Best Practices, SEO. Run on a throttled "Slow 4G" profile.
- [ ] **Lighthouse mobile ≥ 90** same categories.
- [ ] **WCAG 1.4.3 contrast** — every CTA, body, eyebrow, caption passes 4.5:1 AA on its own surface (paper or void). Spot-check via DevTools color-picker contrast inspector at minimum 5 surfaces per phase.
- [ ] **WCAG 2.4.7 focus indicators** — tab through every interactive element. Crimson outline visible on each.
- [ ] **WCAG 2.5.5 touch targets** — every interactive ≥44×44px on mobile.
- [ ] **`prefers-reduced-motion` toggle** — System Settings → Accessibility → Reduce Motion → reload → confirm: nothing animates, every counter shows final value, every reveal element is at opacity 1, focus rings still visible.
- [ ] **RTL screenshot diff** — `dir="rtl"` is global; capture full-page screenshots at 1440 / 768 / 390 viewports, compare against `design-audit/screenshots/`. Zero unintended layout regressions.
- [ ] **LTR screenshot diff (when `/en/` is built)** — toggle to `/en/` route, repeat capture. Bilingual components (eyebrow, wordpair) render mirror-symmetric without retrofit.
- [ ] **Console errors / warnings** — zero in production build. Font 404s must be resolved (PUNCH-LIST P0 #1).
- [ ] **Performance Tab idle CPU** — record 5 seconds of idle on the homepage. After Phase 0 + Phase 1, idle CPU must drop ≥40% vs pre-migration (validates that the 6 always-on infinite ornaments are gone and inactive Method frames are paused).

### Per-phase gate

Specific things to QA after each phase before advancing.

**After Phase 0**:
- [ ] Hover every chip and CTA on the hero — feels noticeably snappier (140ms vs prior 240ms).
- [ ] Tab through every interactive — every one shows a crimson outline, no missing rings.
- [ ] Press-and-hold any CTA on touch — see the `scale(0.98)` press feedback.
- [ ] Browser DevTools Network → Fonts tab — IBM Plex Sans Arabic Variable, Roboto Flex, Cairo Variable, Instrument Serif, JetBrains Mono Variable all load (200 OK). Brand font 404s remain only if PUNCH-LIST P0 #1 isn't yet resolved.

**After Phase 1**:
- [ ] Scroll page top to bottom once — every section's content rises+fades in. On Chromium with `animation-timeline: view()` support, motion is scroll-progress-bound, not binary.
- [ ] Hover every `[data-reveal]` element after first reveal — entry does not re-trigger.
- [ ] Reload mid-scroll — sections above the viewport are already revealed; sections below reveal on cross.
- [ ] Network section's 7 numbers ramp once when first visible.
- [ ] Bilingual proof: every `.wordpair` rendered at chip + hero scale, in both `dir="rtl"` and (when EN site exists) `dir="ltr"`. Cap-heights align; neither script reads as a translation.
- [ ] Eyebrow audit: zero positive `letter-spacing` on any element with Arabic content (check via DevTools computed-style spot inspect on `.eyebrow [lang="ar"]`).
- [ ] Material composition: paper sections show paper grain, void sections show body grain, paper sections do NOT show body grain (test via `:has()` rule firing).

**After Phase 2**:
- [ ] Top-10 #1 (drop-cap) — visit Manifesto, Founder, Method (per frame), Content featured, Newsletter mock — drop-cap renders RTL-correct, color is `--crimson`, hairline rule beneath.
- [ ] Top-10 #2 (⌘K palette) — Cmd+K opens palette with backdrop blur, fuzzy-search 16 actions, Esc closes, Backspace at empty pops breadcrumb.
- [ ] Top-10 #3 (kashida axis) — scroll the hero into view, Arabic accent words visibly elongate kashidas. Cairo fallback verified if Idris not licensed.
- [ ] Top-10 #4 (eyebrow + wordpair) — every section eyebrow renders the canon `الكلمة · WORD · ٠٠١` pattern with caps-aligned baseline.
- [ ] Method continuous numeral path — scroll method, single ink stroke draws ٠١→٠٥ across the 5 frames. On `prefers-reduced-motion`, path renders fully at 18% opacity.
- [ ] Content deck-stack — at ≥6 cards, scrolling pins each card; receding cards scale 0.95 + dim. With <6 cards, falls back to standard grid.

**After Phase 3**:
- [ ] Hero → Manifesto handoff — scroll past hero; the panel pushes back in Z while manifesto rises forward.
- [ ] Founder magazine-cover treatment — flip `data-coverart="true"` on `.fnd3a` (when photo lands); masthead, cover-lines, name overlay activate.
- [ ] Footer debossed wordmark — feature-flag enabled; 5-session canary observation list (below) collected before defaulting on.

### 5-session canary observation list

For risky moves that invert posture, change reading flow, or depend on subjective taste calls. Watch 5 real-user sessions (founder + 4 hired audience-fit testers) before committing to default-on.

| Move | Watch for | Kill criteria |
|------|-----------|---------------|
| **Footer debossed wordmark on cream sub-region** (Footer MOVE 1) | Does the cream-flash transition between dark page body → cream wordmark band → dark sitemap feel deliberate or jarring? Do users register the deboss as "premium-printed" or as "the page broke"? | If 2+ sessions react with confusion ("did it crash?", "is something missing?") within 3 seconds of the cream stripe entering the viewport, ship the alternative: keep wordmark in the dark brand column with the path-stroke decompose hover (MOVE 2) only. |
| **Hero → Manifesto Z-dolly handoff** (motion sig Ritual 3) | Does the panel pushing back in Z signal "you're entering the heart" or read as a glitch? Mac trackpad vs Windows mouse-wheel react differently — observe both. | If users repeatedly scroll back up to "see what just happened," the dolly is being misread. Fall back to standard `.section-glide` (dim + drift, no Z). |
| **Founder magazine-cover treatment** (Founder MOVE 8) | After photo lands, does the `mix-blend-mode: difference` name overlay read as a tasteful editorial composition or as a clumsy filter? Verify on iOS Safari (older WebKit struggles with `preserve-3d` + `mix-blend-mode`). | If the name overlay is illegible at any breakpoint OR the masthead bar breaks below 480px, drop to a static treatment (caption only) until the photo can be re-shot to support the overlay. |
| **Method 3-cover stack** (Method MOVE 8) | When real cover JPGs land, does the fanning deck on `azure` frame's sub-progress feel like editorial weight or animated chaos? | If iOS Safari 17 jankss on `preserve-3d` + `mix-blend-mode: difference`, isolate via `isolation: isolate` on `.mtd3__art--cover-stack`. If still janky, ship single front-card only (cards 2 and 3 hidden). |
| **Continuous SVG numeral path** (Method Top-10 #9) | Does the connected ٠١→٠٥ path read as one editorial gesture, or as 5 decorative outlines? Asset-dependent: if the illustrator brief comes back too literal, the path looks like 5 SVGs strung together. | If sessions describe the move as "just background numerals," redirect the brief: the path should connect (literally — last stroke of ٠١ flows into the first of ٠٢). Hold ship until commission delivers the connected version. |
| **Tagline kashida-axis** (Top-10 #3) | At the Cairo Variable + letter-spacing fallback, does the elongation register at all to native Arabic readers? Or does the move only land with real Idris KSHD axis? | If 3+ sessions don't notice the elongation in the fallback, the budget for Idris license is paid back in the first move. License Idris and reship. |

### Final smoke-test (after all phases)

- [ ] Visit homepage. Verified spin, manifesto orbit, hero halo+pulse-ring, newsletter marquee — all GONE.
- [ ] Scroll to method, then to founder. Method frame 1 SVG decorations stopped (DevTools → `animation-play-state: paused`).
- [ ] Resize window during a hover-search — no reflow on every keypress.
- [ ] Visit on Windows mouse-wheel laptop — Lenis (motion REF 9, 3kb) ships at this point in a separate PR; motion system is now compatible.
- [ ] Compare full-page screenshot against `design-audit/screenshots/` baseline; record visual diff.

---

## Part VII — Open questions for the founder

Three decisions the audit can't make alone. Each gates a Phase 2 or Phase 3 move; defaults are recommended but the founder owns the call.

### Q1 — License 29LT Idris Variable now, or stay on free Cairo Variable + letter-spacing fallback through Q2?

**Move at stake**: Top-10 #3 — Hero tagline kashida-axis on `الترفيه` / `التعليم` (also Manifesto MOVE 6 — climax phrase `جاذبية أفلام الإثارة`).
**Cost**: ~€500 for the family, brand-tier license. One-time.
**Tradeoff**: Idris ships the only Arabic-only structurally-impossible-in-Latin move on the entire research set. Cairo Variable + `letter-spacing` pulse covers ~70% of the perceptual move at zero font cost; native Arabic readers may register the difference in the fallback as "just tracking" rather than "kashida elongation." Per the 5-session canary list above, if 3+ sessions don't notice the elongation in fallback, the license is paid back on first move.
**Recommendation**: License Idris from day one. The hero is the most-loaded section; this is the move it deserves; €500 is one cup of coffee per week of brand-life.
**Decision deadline**: end of Phase 1 (Day 3) — Phase 2 Day 6 ships hero kashida-axis; license must be procured + integrated by then.

### Q2 — Ship Footer debossed cream wordmark on default, or hold behind feature-flag and observe 5 sessions?

**Move at stake**: Footer MOVE 1 — replaces the redundant 60-px CTA banner with a cream sub-region whose pressed-into-paper wordmark is the most editorial moment on the entire site.
**Risk**: The cream surface inverts the site's dark posture. The rest of `.ftr3` stays dark, so this is a 200-px bright stripe between the dark page body and the dark sitemap. At scroll velocity, the brightness flash may feel jarring.
**Recommendation**: Hold behind feature-flag for first ship. Observe 5 sessions per the canary list. Kill criteria are listed in Part VI. If sessions read the cream as "premium-printed," default it on. If 2+ sessions misread as a crash, ship MOVE 2 (path-stroke decompose hover) alone and keep the dark posture intact.
**Decision deadline**: post-Phase 3 day 10, after canary observations.

### Q3 — Founder portrait re-shoot timeline. Hold MOVE 2 (duotone thaw) as bridge, or block Founder section work until the 1600×2000 commissioned shot lands?

**Move at stake**: Founder section's photographic credibility. PUNCH-LIST #31 already flagged the current 150×150 / 7.7KB AI-stylized JPEG as "the single largest credibility hit in the section." Founder MOVE 2 (duotone → color thaw) was specifically designed to hide AI-stylization artifacts under a cool ink-tone scrim during the bridge period.
**Recommendation**: Ship MOVE 2 as the bridge. The duotone *helps* — at low contrast, AI artifacts blur into the ink-tone overlay; the move "warms up" as the bio reveals, which doubles as a narrative arc. Schedule the photo shoot in parallel; when 1600×2000 lands, MOVE 2 still works on the real photograph (warmer ink tones, real skin in color phase). Don't block Founder section work; bridge with MOVE 2.
**Decision deadline**: Phase 2 Day 7 — Founder section ships with MOVE 1 + 3 + 6 (kashida pulse, gulnar quote mark, eastern numerals). MOVE 2 layers on top whenever the founder is ready to schedule shoot.

### Q4 (deferred — flag for context, not a Phase 0–3 blocker)

**Almobadir Charter / "Fundado 1998" discrepancy** (per user MEMORY: contradicts 9-month store age). If the brand voice on this site needs a foundation date, verify parent-co vs brand before reusing the 1998 claim. Doesn't gate WOW work but should be resolved before paid acquisition copy goes live.

### Q5 (deferred)

**Khatt Foundation calligraphic mark commission** (Manifesto MOVE 7 — replaces the rotating concentric SVG with a Saudi tughra-derived glyph in foiled gold). ~€200–400 license/commission tier. Single-instance asset; doesn't gate Phase 1+2 (the placeholder still-mark from MOVE 7 is shippable). Schedule for Phase 3 if budget supports.

### Q6 (deferred)

**Method illustrator commission** (MOVE 7 — replaces 5 generic SVG icons with hand-drawn editorial illustrations in unified style). ~14hr (1hr brief × 5 + 8hr commission + 1hr swap). Lo-fi shippable interim is the unified-stroke-width SVG cleanup (4hr, 60% as polished). Don't block Phase 2 on commission; ship interim, swap when illustrator delivers.

---

**End of Blueprint.** Source documents in `C:\Users\toner\OneDrive\Desktop\almobadir\design-audit\`. Forensic punch list: `PUNCH-LIST.md`. Three signature contracts: `wow-typography-signature.md`, `wow-motion-signature.md`, `wow-material-signature.md`. Per-section moves: `02-wow-{header,hero,manifesto,network,method,content,newsletter,founder,footer}.md`. Reference recon: `wow-research-{arabic,motion,typography,interaction}.md`.





