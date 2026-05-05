# Wave 3 — Manifesto WOW Moves (`.manif3`)

**Compiled:** 2026-04-26
**Section anchor:** `index.html:310-362` · `assets/v3.css:294-324`
**Target tier:** Stripe Press chapter-opener / Pentagram annual-report / Aramco *Qafilah* spread / Khatt Foundation *Harakat* manifesto.
**Constraint:** AMPLIFY, do not redesign. The cream-paper editorial slab parked between two dark slabs is "the strongest compositional move on the entire page" (Wave 2 audit, line 109). Every move below adds material weight to *that* gesture; none of them move the furniture.

---

## Executive summary

The current manifesto is the right composition with the wrong materials. It reads as cream paper on a screen rather than as paper on a desk; the word reveal calls attention to the *act* of writing rather than to *what is written* (Wave 2 finding #28). Eight WOW moves below close that gap by treating every surface as physical: paper grain you can almost feel, ink that bleeds where the underline crosses fibre, a hand-drawn underline that *lengthens* under each word as the eye lands on it (replacing the current scroll-driven opacity-only fade), a crimson period that snaps in and gold-flashes once at the end of the breath, a Saudi calligraphic mark replacing the rotating turbine ornament, an architectural Arabic drop cap on "نحن", a `KSHD` kashida elongation on "جاذبية أفلام الإثارة" (the only phrase we promise to thrill with), an English-shadow paper-fold reveal that earns its translation, and a bottom-edge page-corner curl on scroll exit that returns the slab to the desk it came from.

Every move respects the Wave 2 critical fixes — kashida-aware shaping, logical RTL margins, proper aria, no synthetic Arabic italic, single crimson moment per phrase. Every move ships a `prefers-reduced-motion` fallback that preserves the *editorial* result (paper, ink, calligraphic mark, drop cap stay; only motion is conditional). Total engineering load: ~22-30 hours. Hardest dependency: the `KSHD` axis on 29LT Idris Variable (~€500 license) for MOVE 6; everything else uses fonts the project already loads or free Saudi-licensed glyphs.

Ranked top 3 below. The single highest-leverage move on this list is **MOVE 1 (real paper texture)**: it's the move that lets every subsequent move *land*, because once the surface reads as paper the ink, drop cap, kashida, and corner curl all stop reading as web tropes and start reading as printing.

---

## MOVE 1: Paper-grain texture (SVG turbulence overlay)

**The moment**: Reader's eye lands on the cream slab and registers "this is paper, not pixels" before they consciously read a word. The cream stops being a hex code and starts being a sheet.
**Where**: `.manif3::before` @ `v3.css:296` (extend the existing radial wash with a second SVG-noise layer)
**Implementation**:
```css
.manif3::before {
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: -1;
  background:
    radial-gradient(1200px 600px at 85% 10%, rgba(230,33,59,.04), transparent 60%),
    radial-gradient(900px 500px at 10% 90%, rgba(10,15,30,.05), transparent 60%),
    url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='420' height='420'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='2' stitchTiles='stitch' seed='7'/><feColorMatrix values='0 0 0 0 0.06 0 0 0 0 0.04 0 0 0 0 0.02 0 0 0 0.18 0'/></filter><rect width='100%25' height='100%25' filter='url(%23n)' opacity='0.55'/></svg>");
  background-blend-mode: multiply, multiply, multiply;
  background-size: auto, auto, 420px 420px;
  mix-blend-mode: multiply;
  opacity: 0.85;
}
.manif3::after {
  /* paper warp — top vs bottom tonal lift, uses the dead --manif3-paper-2 var (Wave 2 #16) */
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: -1;
  background: linear-gradient(180deg, var(--manif3-paper-2) 0%, transparent 38%, transparent 62%, color-mix(in oklab, var(--manif3-paper) 88%, var(--void)) 100%);
  opacity: 0.6;
}
```
**Reference**: Wave 1 typography REF 13 (Are.na restraint as material discipline) + Aramco Qafilah Magazine (REF 10, manuscript-surface treatment) — https://www.aramcolife.com/en/publications/qafilah-magazine. Technique sourced from SVG `feTurbulence` paper-grain pattern documented at https://yoksel.github.io/svg-filters/.
**Effort**: ~2hr (texture file inline as data-URI, blend-mode tuning across F8F4EA at desktop / mobile)
**RTL safe**: yes — decorative, no directional axis
**prefers-reduced-motion**: no fallback needed (texture is static)
**Why amplifies (not redesigns)**: The cream-paper colour, radial wash, and section composition stay identical; the texture only adds the materiality that the current flat fill is missing. It activates the dead `--manif3-paper-2` token that Wave 2 #16 flagged as wasted.

---

## MOVE 2: Words "ink in" via hand-drawn underline lengthening

**The moment**: Each word doesn't fade — it's *underlined into being*. A pen-line draws under the word from start to end (RTL-correct, right→left), and the word's ink only resolves to full opacity once the pen passes beneath it. The reader watches the page being annotated by an editor in real time.
**Where**: Replaces the current scroll-driven opacity-only fade on `.manif3__word` @ `v3.css:307-308` and the linear `update()` remap @ `v3.js:124-133` (Wave 2 finding #10)
**Implementation**:
```css
.manif3__word {
  display: inline; /* fixes Wave 2 #1 — kashida-safe shaping */
  position: relative;
  margin-inline-end: .12em; /* fixes Wave 2 #2 — logical axis */
  opacity: .22;
  transition: opacity .25s var(--ease-glide);
  background-image: linear-gradient(to left, currentColor 0 100%, transparent 100%);
  background-repeat: no-repeat;
  background-position: right 0% bottom .04em;
  background-size: 0% .06em;
  transition: opacity .25s var(--ease-glide), background-size .55s cubic-bezier(.2,.7,.1,1);
  color: var(--void);
}
.manif3__word.is-lit {
  opacity: 1;
  background-size: 100% .06em; /* underline lengthens RTL */
}
.manif3__word.is-fully-lit {
  /* 0.4s after lit, the underline fades — leaving only the word, like an editor's pencil mark erased after the read */
  background-size: 100% 0;
  transition: opacity .25s var(--ease-glide), background-size .8s var(--ease-glide) .4s;
}
```
```js
// v3.js — replace the linear scroll remap with a single intersection-fired sequence
const manifIO = new IntersectionObserver(([entry]) => {
  if (!entry.isIntersecting) return;
  const words = entry.target.querySelectorAll('.manif3__word');
  words.forEach((w, i) => {
    setTimeout(() => {
      w.classList.add('is-lit');
      setTimeout(() => w.classList.add('is-fully-lit'), 380);
    }, i * 55); // 55ms stagger — fast enough to feel responsive, slow enough to read
  });
  manifIO.unobserve(entry.target);
}, { threshold: 0.35 });
manifIO.observe(document.querySelector('.manif3__lede'));
```
**Reference**: Wave 1 motion REF 4 (Linear 60ms-stagger row reveal) — https://linear.app/features. Wave 1 typography REF 11 (It's Nice That hand-drawn editorial gesture). Aramco *Qafilah* (REF 10) precedent for "annotated by hand" editorial register.
**Effort**: ~4hr (CSS + JS rewrite, kashida-shape regression test in Safari/Chrome with Almarai + future Mostaqbali)
**RTL safe**: yes — `background-position: right 0%` and `to left` gradient direction make the pen draw RTL-correctly. Verify by inspecting first word "نحن" (rightmost in visual order) underlines first.
**prefers-reduced-motion**: drop the underline animation entirely — words start at `opacity: 1`, no stagger, no underline. The full sentence is read on intersection. (Already handled by the existing `@media (prefers-reduced-motion: reduce)` block at `v3.css:1140` once the new selectors are added.)
**Why amplifies (not redesigns)**: Replaces the Wave 2-flagged "act of writing" gimmick (#28) with the *act of reading* — a copy-editor's pen marking the line. Same word-by-word punctuation; new metaphor. Solves Wave 2 #1 (`inline-block` ligature break) and #2 (wrong-axis margin) along the way.

---

## MOVE 3: Crimson period — snap + single gold flash

**The moment**: After the last word of the sentence has inked, the crimson period doesn't just appear — it *snaps* in with a single 60ms scale overshoot, holds for 80ms, then a thin gold ring flashes outward once and fades. One crimson, one gold, one breath. The whole sentence arrives at this period like a typesetter's full stop.
**Where**: `.manif3__period` @ `v3.css:310-311` and `index.html:349`
**Implementation**:
```html
<!-- v3 markup unchanged structurally -->
<span class="manif3__period">.<span class="manif3__period-flash" aria-hidden="true"></span></span>
```
```css
.manif3__period {
  display: inline-block;
  position: relative;
  color: var(--crimson);
  margin-inline-end: .04em;
  opacity: 0;
  transform: scale(.4);
  transition: opacity .35s var(--ease-glide) .25s, transform .35s cubic-bezier(.34, 1.7, .3, 1) .25s;
}
.manif3.is-fully-lit .manif3__period {
  opacity: 1;
  transform: scale(1);
  animation: manif3-period-settle .12s ease-out .55s 1 backwards;
}
@keyframes manif3-period-settle {
  0% { transform: scale(1.18); }
  100% { transform: scale(1); }
}
.manif3__period-flash {
  position: absolute;
  inset: -.45em;
  border-radius: 50%;
  border: 1.5px solid #C9A24C; /* foiled gold, not LED yellow */
  opacity: 0;
  transform: scale(.5);
  pointer-events: none;
}
.manif3.is-fully-lit .manif3__period-flash {
  animation: manif3-period-gold .9s cubic-bezier(.2,.7,.1,1) .7s 1 forwards;
}
@keyframes manif3-period-gold {
  0%   { opacity: 0;   transform: scale(.5); }
  35%  { opacity: .85; transform: scale(1); }
  100% { opacity: 0;   transform: scale(2.2); }
}
```
**Reference**: Wave 1 interaction REF 3 (Stripe tabular roll precision feel) — https://stripe.com/pricing. Wave 1 motion REF 11 (Things-3 spring overshoot tokens) — https://culturedcode.com/things/. Gold-foil register from Aramco Annual Report (Wave 1 arabic REF 1).
**Effort**: ~1.5hr
**RTL safe**: yes — period sits at logical sentence-end (visual left in RTL), flash is centered radial.
**prefers-reduced-motion**: drop the overshoot and the gold flash entirely; period fades in opacity-only over .35s, stays. The crimson alone is the punctuation.
**Why amplifies (not redesigns)**: The crimson period already exists. Wave 2 #13 said "pick one crimson moment." This move *makes* the period the one — and earns the choice with a single gold flash that signals "this is the seal" without reading as decorative. The underline (Wave 2 #13's other crimson) is removed in MOVE 5; the period becomes the sentence's only red mark.

---

## MOVE 4: English shadow translation reveals via paper-fold

**The moment**: As the reader finishes the Arabic and the period seals, the cream paper appears to *fold up* from the bottom edge to reveal the English translation tucked beneath — like the underside of a folded page being lifted. The fold has perspective; the underside has a hairline darker tone where the crease catches light.
**Where**: `.manif3__shadow` @ `v3.css:316-317` and `index.html:351-353`
**Implementation**:
```html
<p class="manif3__shadow" lang="en">
  <span class="visually-hidden">English translation:</span>
  <em>We are not a content platform. We are a team that believes serious knowledge has no obligation to be boring — and that the most intricate ideas in business and finance can be told with the pull of a thriller film.</em>
</p>
<!-- aria-hidden REMOVED (Wave 2 #3 fix) -->
```
```css
.manif3__shadow {
  direction: ltr;
  text-align: start;
  font-family: var(--font-serif);
  font-style: italic;
  font-weight: 400;
  font-size: clamp(18px, 1.7vw, 22px); /* Wave 2 #19 — capped 22 not 26 */
  line-height: 1.5;
  color: var(--manif3-on-paper-soft);
  max-width: 60ch;
  margin: 0;
  padding-inline-start: 0; /* Wave 2 #19 — drop crimson rule */
  border: 0;                /* Wave 2 #19 — drop border-left */
  text-indent: -.4em;       /* hanging em-dash gesture */
  /* fold transform */
  transform-origin: 50% 0;
  transform: perspective(1200px) rotateX(-90deg);
  opacity: 0;
  transition: transform 1.05s cubic-bezier(.22, .9, .25, 1) .55s, opacity .35s var(--ease-glide) .55s;
  position: relative;
}
.manif3__shadow::before {
  /* underside-of-page crease — only visible during the fold */
  content: '';
  position: absolute;
  top: -1px;
  inset-inline: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(10,15,30,.18), transparent);
  opacity: 0;
  transition: opacity .8s var(--ease-glide);
}
.manif3.is-fully-lit .manif3__shadow {
  transform: perspective(1200px) rotateX(0);
  opacity: 1;
}
.manif3.is-fully-lit .manif3__shadow::before {
  opacity: 1;
  transition-delay: .55s;
  animation: manif3-crease-fade 1.4s var(--ease-glide) 1.4s forwards;
}
@keyframes manif3-crease-fade { to { opacity: 0; } }
```
```css
/* shared visually-hidden if not already in tokens */
.visually-hidden { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0,0,0,0); white-space: nowrap; border: 0; }
```
**Reference**: Wave 1 motion REF 17 (Lusion lite scroll-tied perspective dolly, CSS-only) — https://lusion.co. Wave 1 typography REF 1 (Stripe Press chapter-opener marginalia treatment for translation) — https://press.stripe.com/poor-charlies-almanack.
**Effort**: ~3hr (transform tuning per breakpoint, crease hairline must not appear at desktop pixel-snap)
**RTL safe**: yes — the fold axis is horizontal (top edge), no left/right axis involved. The underlying English `<p>` keeps `direction: ltr` for legibility.
**prefers-reduced-motion**: skip the fold; shadow simply opacity-fades from 0 to 1 over .8s with no transform. The translation arrives on time, just without the page-fold metaphor. Crimson rule and indent drop remain (Wave 2 #19 fix is unconditional).
**Why amplifies (not redesigns)**: Removes Wave 2 critical #3 (`aria-hidden` on the shadow) and Wave 2 #19 (the crimson left rule that read as a UI annotation). Replaces them with the fold gesture, which is editorially honest: the translation *is* underneath the original.

---

## MOVE 5: Architectural Arabic drop cap on "نحن"

**The moment**: The first word "نحن" doesn't lead the line — the **ن** of it sits as a 4-line architectural drop cap to the right, set in a contrasting weight, in the same paper-coloured field but with a single crimson serif-tick at the bottom. The body wraps left around it. This is *Qafilah*-spread typography on the web.
**Where**: New rule on `.manif3__lede`'s `::first-letter` (or via a `<span class="manif3__dropcap">ن</span>` if `::first-letter` doesn't fire reliably across the existing word-span structure — markup option below)
**Implementation**:
```html
<!-- index.html option A — pull the initial letter out of the first word span -->
<p class="manif3__lede" id="manif3-title">
  <span class="manif3__dropcap" aria-hidden="true">ن</span>
  <span class="manif3__word manif3__word--noinitial">حن</span>
  <span class="manif3__word">لسنا</span>
  <!-- … -->
</p>
<!-- visually-hidden full sentence for SR (Wave 2 #11 fix) goes elsewhere in the blockquote -->
```
```css
.manif3__dropcap {
  float: inline-start; /* RTL = float right, the receiving side */
  font-family: var(--font-ar-display);
  font-weight: 800;
  font-size: 5.4em;
  line-height: 0.86;
  margin-inline-start: 0;
  margin-inline-end: .12em;
  margin-block-start: .04em;
  color: var(--void);
  position: relative;
  font-feature-settings: 'calt' 1, 'kern' 1; /* Wave 2 #5 fix — replace the bogus ss01 */
}
.manif3__dropcap::after {
  /* a single crimson serif-tick, oxidised-ink register */
  content: '';
  position: absolute;
  inset-block-end: .12em;
  inset-inline-end: -.06em;
  width: .12em;
  height: .04em;
  background: var(--crimson);
  opacity: .85;
  transform: skewX(-12deg);
}
.manif3__word--noinitial {
  /* prevent margin gap before the kashida-joined remainder of "نحن" */
  margin-inline-end: .18em;
}
```
**Reference**: Wave 1 arabic REF 10 (Qafilah Magazine drop cap as architecture) — https://www.aramcolife.com/en/publications/qafilah-magazine. Wave 1 typography REF 11 (It's Nice That two-color drop cap) — https://www.itsnicethat.com. Wave 1 typography REF 1 (Stripe Press 5-line drop cap) — https://press.stripe.com/poor-charlies-almanack.
**Effort**: ~2.5hr (Arabic letter ن splits cleanly from "نحن" because it's the initial-form joining letter — verify with native Arabic renderer that the remaining "حن" still shapes correctly with the leading char severed)
**RTL safe**: yes — `float: inline-start` resolves to `float: right` under `dir="rtl"`. Critical to use `inline-start` not `right` for token correctness.
**prefers-reduced-motion**: not motion-related; drop cap is structural. No fallback needed. (Note: `prefers-reduced-data` could disable the crimson tick on slow connections but the cost is one element so not worth gating.)
**Why amplifies (not redesigns)**: The lede stays the lede. The drop cap promotes "نحن" — the brand's *we* — to architectural status, which is the single most editorially honest typographic move available in Arabic. Replaces nothing; adds gravitas. Pairs with MOVE 1 (paper texture) so the cap reads as printed, not webby.

---

## MOVE 6: 29LT Idris kashida-axis on "جاذبية أفلام الإثارة"

**The moment**: As the underline pen passes beneath "جاذبية أفلام الإثارة" (the *one* phrase the manifesto promises will *thrill*), the kashidas in those three words physically *elongate* — a 700ms taffy stretch on the connecting strokes — and stay stretched for the rest of the read. Latin can't do this. No competitor will.
**Where**: `.manif3__phrase` @ `v3.css:312` (currently has `nowrap` and the SVG underline)
**Implementation**:
```css
@property --kshd { syntax: '<number>'; inherits: true; initial-value: 0; }
.manif3__phrase {
  position: relative;
  display: inline;
  color: var(--crimson);
  white-space: normal; /* Wave 2 #14 fix — let it wrap on small viewports */
  font-family: '29LT Idris Variable', var(--font-ar-display);
  font-variation-settings: 'wght' 600, 'KSHD' var(--kshd);
  transition: --kshd .7s cubic-bezier(.2,.7,.1,1) .55s;
}
.manif3.is-fully-lit .manif3__phrase {
  --kshd: 72;
}
@media (max-width: 480px) {
  .manif3__phrase { white-space: normal; }
  .manif3.is-fully-lit .manif3__phrase { --kshd: 36; } /* less stretch on mobile */
}
@font-face {
  font-family: '29LT Idris Variable';
  src: url('/assets/fonts/29LT-Idris-Variable.woff2') format('woff2-variations');
  font-weight: 200 800;
  font-display: swap;
}
```
**Fallback if license is gated:**
```css
/* If 29LT Idris Variable hasn't shipped yet, fake it with letter-spacing pulse on Cairo Variable */
.manif3__phrase {
  font-family: 'Cairo Variable', var(--font-ar-display);
  letter-spacing: 0;
  transition: letter-spacing .7s cubic-bezier(.2,.7,.1,1) .55s;
}
.manif3.is-fully-lit .manif3__phrase { letter-spacing: .04em; }
```
**Reference**: Wave 1 typography REF 5 (29LT Idris kashida axis) — https://www.29lt.com/product/29lt-idris-sharp/ + https://blog.29lt.com/2024/11/26/29lt-idris/. Wave 1 arabic REF 8 (kashida as horizontal elongation as editorial breath) — https://blog.29lt.com.
**Effort**: ~3hr (license + font load + variation-settings verification on Safari, where `@property` registration on a font axis can be fussy). License: ~€500 for the family — recommend Star Toner / Almobadir share the budget if either project will use Arabic display variable elsewhere.
**RTL safe**: yes — kashida is an Arabic-native concept; the axis only affects Arabic glyphs. No directional flip.
**prefers-reduced-motion**: skip the transition, set `--kshd: 50` statically (mid-stretch) so the typographic register is preserved. The user still sees the kashida'd letterforms, just without the morph.
**Why amplifies (not redesigns)**: The crimson phrase was already the visual climax of the sentence (Wave 2 noted "four 'look here' gestures within one phrase" in #6). Removing the SVG underline (MOVE 2 absorbs it into the per-word ink) and adding the KSHD stretch concentrates the climax in *one* typographic event — kashida elongation — which is the only event of its kind not borrowed from Latin design. Drops Wave 2 #14 nowrap risk on small screens. Drops Wave 2 #15 broken `pathLength` underline.

---

## MOVE 7: Saudi calligraphic mark replaces rotating ornament

**The moment**: The figcaption's spinning concentric-circle SVG turbine is gone. In its place sits a 22px **wax-seal-style calligraphic mark** — a single Khatt-Foundation-licensed Saudi tughra-derived glyph (or a custom letter-mark in the manifesto's display weight) — set in foiled gold, sitting still. No rotation. The reader's eye reads it as a stamp, not a propeller.
**Where**: `.manif3__ornament svg` @ `index.html:356` and `v3.css:319-321` (Wave 2 finding #9)
**Implementation**:
```html
<!-- replace the generic concentric-circle SVG with a curated Saudi calligraphic mark -->
<span class="manif3__ornament" aria-hidden="true">
  <svg viewBox="0 0 32 32" width="22" height="22" role="img">
    <!-- placeholder: a stylised "م" of المبادر set as a wax-seal mark -->
    <!-- replace path data with Khatt Foundation / Bahia Shehab licensed glyph or commissioned mark -->
    <circle cx="16" cy="16" r="14" fill="none" stroke="currentColor" stroke-width=".75" opacity=".55"/>
    <path d="M11 22 C 11 18, 13 15, 16 15 C 19 15, 21 18, 21 22 M 16 11 L 16 13 M 13 13 L 19 13" stroke="currentColor" stroke-width="1.6" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
  </svg>
</span>
```
```css
.manif3__ornament {
  color: #C9A24C; /* foiled gold, not crimson */
  display: inline-flex;
}
.manif3__ornament svg {
  /* animation: REMOVED (Wave 2 #9 + #22 fix) — wax seal is still */
  filter: drop-shadow(0 .5px 0 rgba(10,15,30,.18)); /* impressed-into-paper shadow */
  transition: filter .35s var(--ease-glide);
}
.manif3.is-fully-lit .manif3__ornament svg {
  /* one-shot: a single 220ms gold-flash to time with the period flash from MOVE 3 */
  filter: drop-shadow(0 .5px 0 rgba(10,15,30,.18)) drop-shadow(0 0 6px rgba(201,162,76,.55));
  transition: filter .35s var(--ease-glide) .9s;
}
```
**Reference**: Wave 1 arabic REF 2 (Bahia Shehab — A Thousand Times No, lam-alif lexicon) — https://www.bahiashehab.com/featured-group-shows/a-thousand-times-no | https://www.khtt.net/en/page/15813/a-thousand-times-no. Wave 1 arabic REF 7 (Islamic Arts Biennale — letterform-as-mark) — https://www.atrissi.com/islamic-arts-biennale-branding/. Wave 1 arabic REF 10 (Qafilah / Kameel Hawa wax-seal register).
**Effort**: ~2hr engineering + curation: commission or license a real Saudi calligraphic mark from Khatt Foundation (https://www.khtt.net) or Tarek Atrissi Studio. Budget: ~€200-400 depending on license tier. Engineering itself: drop-in SVG swap.
**RTL safe**: yes — decorative, no directional axis. The mark sits in a flexbox row that already respects logical properties.
**prefers-reduced-motion**: drop the gold-flash filter transition; the mark is static at all times. (No regression — the previous spin was also being killed by reduced-motion via the global `@media` block, so this is a strict improvement.)
**Why amplifies (not redesigns)**: Wave 2 #9 said "stop the rotation. The ornament should be a wax seal, not a turbine." This move *makes it the wax seal* — and elevates it from a generic dingbat to a culturally-specific Saudi mark, which the project's "Arabic editorial seriousness" thesis (Wave 1 arabic summary) demands. The figcaption position and DOM structure are unchanged.

---

## MOVE 8: Page-corner curl on scroll exit

**The moment**: As the reader scrolls past the manifesto and the next dark slab (Network) starts to crowd in, the bottom-leading corner of the cream paper *curls up* — a 28°-deep dog-ear with a darker triangular underside — clip-pathed off the slab. The paper is being lifted off the desk. The dark slab arrives as the desk underneath.
**Where**: `.manif3` itself @ `v3.css:295` (extend existing surface) — uses CSS scroll-driven animation tied to the section's view-timeline
**Implementation**:
```css
@property --curl { syntax: '<number>'; inherits: true; initial-value: 0; }

.manif3 {
  /* existing rules unchanged */
  --curl: 0;
  /* the curl is a pseudo-element that paints ON TOP of the bottom-inline-start corner */
}
.manif3::after {
  /* COEXISTS with MOVE 1's paper-warp ::after — change MOVE 1 to use a child <div class="manif3__paper"> if you implement both, OR merge into a single combined ::after rule. Sketch shown as separate for clarity */
  content: '';
  position: absolute;
  inset-block-end: 0;
  inset-inline-start: 0;
  /* dog-ear is a triangle revealed by clip-path; size grows with --curl */
  width: calc(64px + var(--curl) * 28px);
  height: calc(64px + var(--curl) * 28px);
  background:
    linear-gradient(135deg, color-mix(in oklab, var(--manif3-paper) 76%, var(--void)) 0%, color-mix(in oklab, var(--manif3-paper) 92%, var(--void)) 100%);
  clip-path: polygon(0 100%, 100% 100%, 0 0);
  transform-origin: 0 100%;
  transform: rotate(calc(-2deg * var(--curl))) translate(calc(-1px * var(--curl)), calc(1px * var(--curl)));
  filter: drop-shadow(2px -2px 4px rgba(10,15,30,.18));
  pointer-events: none;
  z-index: 2;
  opacity: var(--curl);
}
@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    .manif3 {
      animation: manif3-curl linear both;
      animation-timeline: view(block);
      animation-range: exit 0% exit 90%;
    }
    @keyframes manif3-curl {
      from { --curl: 0; }
      to   { --curl: 1; }
    }
  }
}
```
**Reference**: Wave 1 motion REF 3 (Cyd Stumpel — CSS `animation-timeline: view()`) — https://cydstumpel.nl/start-using-scroll-driven-animations-today/. Wave 1 motion REF 17 (Lusion lite CSS-only perspective). Print-skeumorphic dog-ear convention from physical editorial design (Stripe Press chapter ends).
**Effort**: ~3hr (clip-path + view-timeline tuning, Safari fallback gracefully degrades to no curl since `animation-timeline: view()` is gated)
**RTL safe**: yes — `inset-inline-start: 0` resolves to the visual right corner under `dir="rtl"`, which is the receiving (logical end) edge of the slab. Curl appears at the bottom-right in Arabic flow, bottom-left in LTR — both are the trailing corner. Sign-flip handled by logical property.
**prefers-reduced-motion**: the entire `@supports + @media (prefers-reduced-motion: no-preference)` block is skipped → no curl. The slab edge stays straight. No regression.
**Why amplifies (not redesigns)**: The composition (cream slab between dark slabs) is *the* compositional move on the page. The curl preserves it and adds a single physical exit gesture — paper lifting off the desk — that resolves the slab back into the dark Network section beneath it. No layout change; no content change; one corner.

---

## Ranked top 3

### #1 — MOVE 1: Paper-grain texture (SVG turbulence)

The base material. Without it, MOVES 5 (drop cap), 6 (kashida), 7 (wax seal), and 8 (corner curl) all read as "web tricks on a flat slab" instead of as printing. Activates the dead `--manif3-paper-2` token from Wave 2 #16. ~2hr; zero RTL/A11y risk; zero motion footprint. Ship first.

### #2 — MOVE 2: Words "ink in" via underline lengthening + simultaneous Wave 2 critical fixes

This is the single largest perceptual upgrade on the manifesto: it replaces the Wave 2-flagged "act of writing" gimmick (#28) with editorial ink, AND simultaneously fixes Wave 2 critical #1 (`inline-block` ligature break), critical #2 (wrong-axis `margin-left`), and high #10 (scroll-driven word reveal that's unreadable at fast scroll). One move, four fixes. ~4hr. Most engineering load on the list, most return.

### #3 — MOVE 5: Architectural Arabic drop cap on "نحن"

Pure CSS. ~2.5hr. Promotes the brand's first word — "we" — to *Qafilah*-spread architectural status. The single most editorially honest typographic move available in Arabic that we're not yet making. Pairs with MOVE 1 to read as printed, not webby. No motion, no animation, no a11y exposure; just typography.

---

## 200-word summary

Eight WOW moves amplify the manifesto without redesigning it. MOVE 1 lays a real paper-grain texture (SVG turbulence + dead-token-activated tonal warp) so the cream slab reads as a sheet, not a fill. MOVE 2 replaces the scroll-driven word reveal with a hand-drawn pen-line that *underlines each word into being*, RTL-correct, and simultaneously fixes the Wave 2 critical issues of broken Arabic shaping (`inline-block`) and wrong-axis margin. MOVE 3 gives the crimson period a single overshoot snap and one gold flash, making it — not the underline — the only crimson seal. MOVE 4 reveals the English shadow translation via paper-fold (perspective rotateX), removing the Wave 2-flagged crimson UI rule. MOVE 5 sets "نحن" as a 4-line architectural Arabic drop cap with a crimson serif-tick, *Qafilah*-style. MOVE 6 adds 29LT Idris's kashida axis to the climax phrase "جاذبية أفلام الإثارة" — a structurally Arabic gesture no Latin foundry can replicate. MOVE 7 swaps the spinning concentric SVG for a still Saudi calligraphic mark in foiled gold (Khatt Foundation register). MOVE 8 curls the bottom-trailing corner off the slab on scroll exit, returning the paper to its desk. Total ~22-30hr; one font license decision; every move ships a `prefers-reduced-motion` fallback that preserves the editorial result.
