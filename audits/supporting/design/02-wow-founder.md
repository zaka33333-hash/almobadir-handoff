# Wave 2 — FOUNDER (`.fnd3a`) Wow Moves
## Badr Shaker editorial profile

**Authored:** 2026-04-26
**Section:** `<section class="fnd3a" id="founder">` — index.html lines 1005–1104
**CSS:** `assets/v3.css` lines 763–873
**JS:** `assets/v3.js` lines 401–445 (IO + counter + photo parallax)
**Live URL:** http://localhost:8765/index.html#founder
**Bar:** NYT Magazine contributor pages, The Atlantic staff bios, Stripe Press author cards, Brownbook profile pages.
**Wave 1 inputs:** wow-research-{arabic, motion, typography, interaction}.md

---

## Executive Summary

Commit 1 cut vanity (verified-spin badge, AUTHORITY block) — chassis is now editorial-clean. Resting state is photo + masthead + name + tagline + bio + quote + 3-stat dl + 4-card follow grid. The cinematic budget freed by those deletions has not yet been spent. This pass spends it where the eye actually lands: **the moment of arrival** (counters roll, name kashida-pulses, photo duotone-thaws into color), **the dwell** (real Khatt-tradition Arabic mark in the quote, paper-grain shift, magnetic Calendly card), and **the upgrade lane** (a magazine-cover treatment that activates only when the commissioned 1600×2000 portrait lands, so today's 150×150 JPEG is not magnified into a worse mistake).

Each move below has a `Asset dependency` tag — three work today with the AI-stylized JPEG, four upgrade gracefully when the real shot arrives, one is photo-blocked. None require new dependencies. None ship sound. Every move respects `prefers-reduced-motion`. Every Arabic-relevant move uses Cairo Variable + Tatweel (U+0640) — no proprietary 29LT Idris license required for v1.

The single highest-leverage move is **MOVE 1 — counter roll + kashida pulse on the name on viewport entry**. It gives the section a "first scroll-in moment" without animation-on-rest, costs ~25 lines of JS+CSS, lands in 2 hours, and reads as cinematic-editorial rather than influencer-dashboard.

**Budget for top 3:** ~6 hours engineering, zero new assets required.

---

## MOVE 1: Counter roll + kashida pulse on entry
**The moment**: When the section crosses 18% into viewport, the name `بدر شاكر` swells its kashidas (letter-spacing + variation pulse) for 700ms while the three stats counters roll from `0 → 6 / 325 / 5,000` in tabular-num lockstep, then the page settles to stillness.
**Where**: `.fnd3a__name` + `.fnd3a__count` @ index.html lines 1032, 1046, 1051, 1056 · CSS lines 796–797 · JS lines 405–428 (extend existing IO + `runCounters`).
**Implementation**:
```css
/* v3.css — append near .fnd3a__name (line 796) */
@property --fnd3a-kshd { syntax: "<number>"; inherits: false; initial-value: 0; }
.fnd3a__name {
  --fnd3a-kshd: 0;
  letter-spacing: calc(-0.025em + var(--fnd3a-kshd) * 0.03em);
  font-variation-settings: "wght" calc(800 + var(--fnd3a-kshd) * 80);
  transition: --fnd3a-kshd 720ms cubic-bezier(.2,.7,.1,1);
}
.fnd3a.is-inview .fnd3a__name { --fnd3a-kshd: 1; }
.fnd3a.is-inview.is-settled .fnd3a__name { --fnd3a-kshd: 0; transition-duration: 1100ms; }
.fnd3a__stat-val { font-variant-numeric: tabular-nums; }
@media (prefers-reduced-motion: reduce) {
  .fnd3a__name { transition: none; --fnd3a-kshd: 0; }
}
```
```js
/* v3.js — modify the existing IO callback at line 405 */
const io = new IntersectionObserver((entries) => {
  entries.forEach((e) => {
    if (e.isIntersecting) {
      root.classList.add('is-inview');
      io.unobserve(root);
      runCounters();
      // settle the kashida pulse after the counters finish
      setTimeout(() => root.classList.add('is-settled'), 1500);
    }
  });
}, { threshold: 0.18 });
```
**Reference**: Wave 1 motion REF 3 (CSS `animation-timeline: view()` ladders) + REF 14 (3-tier duration discipline) · Wave 1 typography REF 5 (29LT Idris kashida axis, Cairo Variable fallback) — https://blog.29lt.com/2024/11/26/29lt-idris/ · Wave 1 interaction REF 3 (Stripe-style number roll) — https://stripe.com/pricing
**Effort**: ~2hr
**RTL safe**: yes — kashida widening reads LTR/RTL identical because it operates on letter-spacing, no sign flip.
**prefers-reduced-motion**: counters jump to final via existing `if (reduceMotion)` guard at v3.js:415; kashida pulse skipped via media query → name stays at `--fnd3a-kshd: 0`.
**Asset dependency**: works with current photo, works with both.

---

## MOVE 2: Duotone → color thaw on photo, scrubbed by section scroll
**The moment**: As the founder section enters and crosses the viewport, the portrait *thaws* from a cool ink-tone duotone into a full-color image — synchronized to the bio paragraph revealing line-by-line. The reader sees the founder "warm up" exactly while reading what he does.
**Where**: `.fnd3a__photo` + `.fnd3a__bio` @ index.html lines 1023, 1035 · CSS line 779.
**Implementation**:
```css
/* v3.css — replace .fnd3a__photo block at line 779 */
@property --fnd3a-thaw { syntax: "<number>"; inherits: false; initial-value: 0; }
.fnd3a__photo {
  width: 100%; height: 100%; object-fit: cover;
  /* duotone scrim: 0 = full duotone (ink), 1 = full color */
  filter:
    contrast(calc(1.04 + var(--fnd3a-thaw) * 0.0))
    saturate(calc(0.0 + var(--fnd3a-thaw) * 0.92))
    brightness(calc(0.82 + var(--fnd3a-thaw) * 0.12))
    sepia(calc((1 - var(--fnd3a-thaw)) * 0.18))
    hue-rotate(calc((1 - var(--fnd3a-thaw)) * 200deg));
  transform: scale(1.02);
  transition: transform 1.6s var(--ease-glide), filter 900ms var(--ease-glide);
}
@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    .fnd3a__photo {
      animation: fnd3aThaw linear both;
      animation-timeline: view();
      animation-range: cover 5% cover 55%;
    }
    @keyframes fnd3aThaw { from { --fnd3a-thaw: 0; } to { --fnd3a-thaw: 1; } }
  }
}
/* fallback for Safari / non-supporting: tie to is-inview only */
@supports not (animation-timeline: view()) {
  .fnd3a__photo { --fnd3a-thaw: 0; }
  .fnd3a.is-inview .fnd3a__photo { --fnd3a-thaw: 1; }
}
@media (prefers-reduced-motion: reduce) {
  .fnd3a__photo { animation: none; --fnd3a-thaw: 1; }
}
```
**Reference**: Wave 1 motion REF 2 (Apple Vision Pro sticky-video scrub, applied to filter axis instead of currentTime) — https://www.apple.com/apple-vision-pro/ · Wave 1 motion REF 3 (CSS scroll-driven animations) — https://cydstumpel.nl/start-using-scroll-driven-animations-today/
**Effort**: ~1.5hr
**RTL safe**: yes — filter operates per-pixel, direction-agnostic.
**prefers-reduced-motion**: photo lands at `--fnd3a-thaw: 1` (full color) immediately, no scrub.
**Asset dependency**: works with current photo (the duotone *helps* hide AI-stylization artifacts at low contrast), works better with commissioned shot (warmer ink tones, real skin in color phase).

---

## MOVE 3: Real Khatt-tradition Arabic mark replaces the « guillemet
**The moment**: The pull quote opens with a hand-drawn Arabic *gulnar* (rosette/quotation mark) calligraphic stroke instead of the European « character — the kind of mark Bahia Shehab's manuscripts and Khatt Foundation specimens use. On viewport entry the mark draws itself with `stroke-dasharray` over 900ms, ending exactly as the quote text becomes legible.
**Where**: `.fnd3a__quote-mark` @ index.html line 1039 · CSS line 806.
**Implementation**:
```html
<!-- index.html line 1039 — replace the « span with an inline SVG -->
<svg class="fnd3a__quote-mark" viewBox="0 0 64 64" aria-hidden="true" focusable="false">
  <!-- gulnar/rosette mark inspired by Khatt Naskh tradition: 3 petals + centre dot -->
  <g fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round">
    <path d="M32 12 C 22 18, 22 30, 32 32 C 42 30, 42 18, 32 12 Z" />
    <path d="M14 32 C 20 22, 32 22, 32 32 C 32 42, 20 42, 14 32 Z" />
    <path d="M50 32 C 44 22, 32 22, 32 32 C 32 42, 44 42, 50 32 Z" />
    <circle cx="32" cy="32" r="2" fill="currentColor" stroke="none"/>
  </g>
</svg>
```
```css
/* v3.css — replace .fnd3a__quote-mark block at line 806 */
.fnd3a__quote-mark {
  position: absolute; top: 14px; inset-inline-start: 18px;
  width: 56px; height: 56px;
  color: var(--crimson); opacity: 0.42;
  pointer-events: none;
}
.fnd3a__quote-mark path, .fnd3a__quote-mark circle {
  stroke-dasharray: 1; stroke-dashoffset: 1; pathLength: 1;
}
.fnd3a.is-inview .fnd3a__quote-mark path,
.fnd3a.is-inview .fnd3a__quote-mark circle {
  animation: fnd3aQuoteDraw 900ms cubic-bezier(.2,.7,.1,1) forwards;
  animation-delay: 460ms; /* lands with the quote reveal at v3.css:861 */
}
.fnd3a.is-inview .fnd3a__quote-mark path:nth-child(2) { animation-delay: 540ms; }
.fnd3a.is-inview .fnd3a__quote-mark path:nth-child(3) { animation-delay: 620ms; }
@keyframes fnd3aQuoteDraw { to { stroke-dashoffset: 0; } }
@media (prefers-reduced-motion: reduce) {
  .fnd3a__quote-mark path, .fnd3a__quote-mark circle { stroke-dashoffset: 0; animation: none; }
}
```
**Reference**: Wave 1 arabic REF 2 (Bahia Shehab "A Thousand Times No" lam-alif lexicon) — https://www.bahiashehab.com/featured-group-shows/a-thousand-times-no · Wave 1 arabic REF 11 (Khatt Foundation Harakat manifesto Naskh aesthetic) — https://www.khtt.net/en/page/3650/the-harakat-manifesto · Wave 1 motion REF 13 (GitHub Copilot path-draw stagger) — https://github.com/features/copilot
**Effort**: ~1.5hr (40 min for the SVG; rest is CSS + RTL test)
**RTL safe**: yes — symmetric mark, decorative; sits on `inset-inline-start` already.
**prefers-reduced-motion**: SVG paths render at full `stroke-dashoffset: 0` instantly — mark appears without draw.
**Asset dependency**: works with current photo, works with both.

---

## MOVE 4: Platform-aware hover preview on the follow grid cards
**The moment**: Hover on the Instagram card and a tiny 3×3 mini-grid of Badr's actual top posts fades in inside the card on its right edge. Threads card → mini text-card excerpt. Calendly card → date-strip preview ("Wed · Thu · Fri"). Favikon card → mini #6 badge with the rank graphic. The card grows from "social link" into "preview of the destination" without leaving the page.
**Where**: `.fnd3a__follow-card` @ index.html lines 1069–1096 · CSS line 837.
**Implementation**:
```html
<!-- index.html — add a preview node to each card; example for Instagram (line 1069) -->
<a href="https://instagram.com/badrshaqer" target="_blank" rel="noopener" class="fnd3a__follow-card" data-platform="ig">
  <span class="fnd3a__follow-num">01</span>
  <span class="fnd3a__follow-icon" aria-hidden="true">…existing svg…</span>
  <div class="fnd3a__follow-body">
    <span class="fnd3a__follow-platform">Instagram</span>
    <span class="fnd3a__follow-handle">@badrshaqer</span>
  </div>
  <span class="fnd3a__follow-stat">317K</span>
  <span class="fnd3a__follow-arrow" aria-hidden="true">↗</span>
  <span class="fnd3a__follow-preview" aria-hidden="true">
    <!-- 9 tiny tiles (curated from his feed; jpgs at 60×60 each) -->
    <span></span><span></span><span></span>
    <span></span><span></span><span></span>
    <span></span><span></span><span></span>
  </span>
</a>
```
```css
/* v3.css — append after .fnd3a__follow-card hover (line 840) */
.fnd3a__follow-preview {
  position: absolute; inset-block: 8px; inset-inline-end: 8px;
  width: 0; aspect-ratio: 1; opacity: 0;
  display: grid; grid-template-columns: repeat(3, 1fr); gap: 1px;
  background: var(--hairline);
  border-radius: 8px; overflow: hidden;
  transition: width 380ms var(--ease-glide), opacity 240ms ease;
  pointer-events: none;
}
.fnd3a__follow-preview span {
  background-color: var(--void-3);
  background-size: cover; background-position: center;
}
/* Curated thumbnails per platform — drop these into assets/ig/01.jpg etc. */
[data-platform="ig"] .fnd3a__follow-preview span:nth-child(1) { background-image: url(assets/ig/01.jpg); }
[data-platform="ig"] .fnd3a__follow-preview span:nth-child(2) { background-image: url(assets/ig/02.jpg); }
[data-platform="ig"] .fnd3a__follow-preview span:nth-child(3) { background-image: url(assets/ig/03.jpg); }
/* …repeat 4–9; 9 tiles total */
[data-platform="threads"] .fnd3a__follow-preview {
  grid-template-columns: 1fr; padding: 8px; background: var(--void-2);
  font-family: var(--font-ar-body); font-size: 9px; line-height: 1.3; color: var(--ink-mute);
}
[data-platform="calendly"] .fnd3a__follow-preview {
  grid-template-columns: repeat(3, 1fr); align-items: center;
  font-family: var(--font-mono); font-size: 9px; color: var(--ink);
}
.fnd3a__follow-card:hover .fnd3a__follow-preview { width: 76px; opacity: 1; }
@media (hover: none), (prefers-reduced-motion: reduce), (max-width: 720px) {
  .fnd3a__follow-preview { display: none; }
}
```
**Reference**: Wave 1 interaction REF 13 (hover-earned reveal — Linear/Notion 1px payoff) — https://linear.app · Wave 1 motion REF 8 (Arc Browser viewport-aware video previews) — https://arc.net
**Effort**: ~3hr (1.5hr CSS+HTML, 1.5hr curating 9 IG thumbs at 60×60 + 3 Threads excerpts)
**RTL safe**: yes — preview lives on `inset-inline-end`, mirrors automatically; the grid is symmetric. Test that the IG preview anchors correctly in RTL.
**prefers-reduced-motion**: preview hidden entirely (display:none) — card hover still works for `transform: translateY(-3px)` from existing CSS.
**Asset dependency**: needs 9 IG thumbnails curated from real feed (~30 min editorial); works with current founder photo (preview is platform-content, not founder-photo).

---

## MOVE 5: Magnetic Calendly card (within the follow grid)
**The moment**: As the cursor approaches the Calendly card from any direction within ~80px, the card translates 4–6px toward the cursor and the card's inner label translates 8px further — the conversion CTA literally *reaches* for the visitor. Card 1, 2, 4 stay still. Only #03 is magnetic.
**Where**: `.fnd3a__follow-card--cta` @ index.html line 1083 · CSS line 851 · JS new block in fnd3a IIFE at v3.js:402.
**Implementation**:
```js
/* v3.js — append inside the fnd3a IIFE before its closing })(); at line 445 */
const ctaCard = root.querySelector('.fnd3a__follow-card--cta');
if (ctaCard && !reduceMotion && matchMedia('(hover: hover)').matches) {
  const ctaLabel = ctaCard.querySelector('.fnd3a__follow-body');
  const zone = ctaCard.parentElement; // the <li> wrapper
  let raf = null, tx = 0, ty = 0, lx = 0, ly = 0;
  zone.addEventListener('pointermove', (e) => {
    const r = ctaCard.getBoundingClientRect();
    tx = Math.max(-6, Math.min(6, (e.clientX - (r.left + r.width / 2)) * 0.12));
    ty = Math.max(-6, Math.min(6, (e.clientY - (r.top + r.height / 2)) * 0.12));
    if (!raf) raf = requestAnimationFrame(loop);
  });
  zone.addEventListener('pointerleave', () => { tx = 0; ty = 0; });
  function loop() {
    lx += (tx - lx) * 0.18; ly += (ty - ly) * 0.18;
    ctaCard.style.transform = `translate(${lx}px, ${ly}px)`;
    if (ctaLabel) ctaLabel.style.transform = `translate(${lx * 0.4}px, ${ly * 0.4}px)`;
    if (Math.abs(tx - lx) > 0.05 || Math.abs(ty - ly) > 0.05) raf = requestAnimationFrame(loop);
    else { raf = null; ctaCard.style.transform = ''; if (ctaLabel) ctaLabel.style.transform = ''; }
  }
}
```
```css
/* v3.css — append .fnd3a__follow-card--cta tweak after line 855 */
.fnd3a__follow-card--cta {
  transition: transform 0.42s cubic-bezier(.34,1.56,.64,1), border-color 0.4s ease, background-color 0.4s ease;
}
.fnd3a__follow-card--cta .fnd3a__follow-body {
  transition: transform 0.42s cubic-bezier(.34,1.56,.64,1);
}
```
**Reference**: Wave 1 interaction REF 15 (magnetic CTA — Vercel/Linear) — https://vercel.com · Wave 1 motion REF 11 (Things 3 spring overshoot easing) — https://culturedcode.com/things/
**Effort**: ~1hr
**RTL safe**: yes — pointer math is absolute pixels, no sign-flip.
**prefers-reduced-motion**: skipped via the `!reduceMotion` guard. Hover still gets `transform: translateY(-3px)` from existing CSS.
**Asset dependency**: works with current photo, works with both.

---

## MOVE 6: Eastern-Arabic numeral set in the stats counters
**The moment**: The three stat values display in Eastern-Arabic numerals (٦ / ٣٢٥ / ٥٫٠٠٠) instead of Western (6 / 325 / 5,000) — culturally native to a Riyadh founder, signals "Arabic-first publication." The counter still rolls; it just rolls in Arabic numerals.
**Where**: `.fnd3a__count` @ index.html lines 1046, 1051, 1056 · JS lines 410–433 (extend `formatNum`).
**Implementation**:
```js
/* v3.js — replace formatNum at line 429 */
function formatNum(v, format, target) {
  const n = (format === 'comma') ? Math.round(v) : Math.round(v);
  // Eastern-Arabic numeral mapping (٠١٢٣٤٥٦٧٨٩)
  const easternDigits = ['٠','١','٢','٣','٤','٥','٦','٧','٨','٩'];
  // Use locale 'ar-SA' with an Arab-Indic numbering system — Intl handles grouping correctly
  const fmt = new Intl.NumberFormat('ar-SA-u-nu-arab', {
    useGrouping: format === 'comma',
  });
  return fmt.format(n);
}
```
```html
<!-- index.html — update the dt labels to match (lines 1045, 1050, 1055) so script is consistent -->
<!-- already in Arabic body / Latin mono — keep as-is. Only the numerals shift. -->
```
**Reference**: Wave 1 arabic REF 16 (Eastern-Arabic numeral as graphic — Aramco Vision 2030) — https://www.aramco.com · Wave 1 arabic REF 1 (single Arabic numeral as full-bleed page).
**Effort**: ~30min
**RTL safe**: yes — Intl handles bidi for grouped numerals; CSS `unicode-bidi: plaintext` already applies via parent `dir="rtl"`.
**prefers-reduced-motion**: counter still skips animation via existing reduceMotion guard; final value just renders in Arabic numerals.
**Asset dependency**: works with current photo, works with both.

---

## MOVE 7: Section-scoped paper-grain shift on scroll
**The moment**: The existing `.fnd3a__grain` SVG-noise overlay (currently at fixed 0.06 opacity) drifts — slowly translates and shifts opacity from 0.04 → 0.08 → 0.04 over the user's scroll through the section. Paper feels alive without animation-on-rest. Imperceptible per-frame; obvious if you scroll back.
**Where**: `.fnd3a__grain` @ index.html line 1006 · CSS line 765.
**Implementation**:
```css
/* v3.css — replace .fnd3a__grain block at line 765 */
.fnd3a__grain {
  position: absolute; inset: 0; pointer-events: none; z-index: 0;
  opacity: 0.06; mix-blend-mode: overlay;
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='240' height='240'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='2' stitchTiles='stitch'/><feColorMatrix values='0 0 0 0 1 0 0 0 0 1 0 0 0 0 1 0 0 0 0.6 0'/></filter><rect width='100%25' height='100%25' filter='url(%23n)'/></svg>");
  background-size: 240px 240px;
}
@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    .fnd3a__grain {
      animation: fnd3aGrainDrift linear both;
      animation-timeline: view();
      animation-range: cover 0% cover 100%;
    }
    @keyframes fnd3aGrainDrift {
      0%   { opacity: 0.04; background-position: 0 0; }
      50%  { opacity: 0.08; background-position: 120px 60px; }
      100% { opacity: 0.04; background-position: 240px 120px; }
    }
  }
}
```
**Reference**: Wave 1 motion REF 12 (Figma Config parallax with `animation-timeline: scroll()`) — https://config.figma.com · Wave 1 motion REF 3 (CSS scroll-driven primitives).
**Effort**: ~30min
**RTL safe**: decorative, isolated to background-position; no directional content.
**prefers-reduced-motion**: animation skipped via `@media (prefers-reduced-motion: no-preference)` wrap; grain stays at 0.06 opacity.
**Asset dependency**: works with current photo, works with both.

---

## MOVE 8: Magazine-cover treatment (activates only when commissioned shot lands)
**The moment**: When a real 1600×2000 4:5 portrait replaces the AI-stylized JPEG, the section's left column promotes itself: a fine-printed masthead bar runs across the top of the photo (`المبادر · العدد ٠٠١ · 2026`), three "cover lines" hang on the right margin of the photo (`الفلسفة · الفريق · الشبكة` — section anchors), and the founder's name overlays the bottom-third of the portrait at `mix-blend-mode: difference` in Cairo Variable 1000. The section transforms from "bio block" into "issue 001 cover."
**Where**: `.fnd3a__portrait-frame` @ index.html lines 1022–1026 · CSS lines 778–791. Activation gated by `data-coverart="true"` on the section.
**Implementation**:
```html
<!-- index.html line 1005 — add data-coverart toggle (default off; flip on after photo upload) -->
<section class="fnd3a" id="founder" aria-labelledby="fnd3a-name" dir="rtl" data-coverart="false">
  …
  <figure class="fnd3a__portrait">
    <div class="fnd3a__portrait-frame">
      <!-- existing img -->
      <img src="assets/Badrshaqer.jpg" alt="…" class="fnd3a__photo" …>
      <div class="fnd3a__portrait-veil" aria-hidden="true"></div>

      <!-- cover-mode chrome — visible only when data-coverart="true" -->
      <div class="fnd3a__cover-masthead" aria-hidden="true">
        <span>المبادر</span>
        <span class="fnd3a__cover-rule"></span>
        <span class="fnd3a__cover-issue">العدد ٠٠١ · 2026</span>
      </div>
      <ul class="fnd3a__cover-lines" aria-hidden="true">
        <li>الفلسفة</li>
        <li>الفريق</li>
        <li>الشبكة</li>
      </ul>
      <span class="fnd3a__cover-name" aria-hidden="true">بدر شاكر</span>

      <figcaption class="fnd3a__cap">…</figcaption>
    </div>
  </figure>
```
```css
/* v3.css — append after the existing .fnd3a__cap block (~line 791) */
.fnd3a__cover-masthead, .fnd3a__cover-lines, .fnd3a__cover-name { display: none; }

[data-coverart="true"] .fnd3a__cover-masthead {
  position: absolute; top: 12px; inset-inline: 14px;
  display: flex; align-items: center; gap: 10px;
  font-family: var(--font-mono); font-size: 10px; letter-spacing: 0.22em;
  color: var(--ink); text-transform: uppercase; z-index: 2;
  text-shadow: 0 1px 4px rgba(0,0,0,.5);
}
[data-coverart="true"] .fnd3a__cover-masthead > span:first-child {
  font-family: var(--font-ar-display); font-weight: 800; letter-spacing: 0;
}
[data-coverart="true"] .fnd3a__cover-rule {
  flex: 1; height: 1px; background: rgba(245,234,210,0.6);
}
[data-coverart="true"] .fnd3a__cover-lines {
  position: absolute; top: 50%; transform: translateY(-50%);
  inset-inline-end: 14px;
  list-style: none; margin: 0; padding: 0;
  display: flex; flex-direction: column; gap: 14px;
  font-family: var(--font-ar-body); font-size: 13px; font-weight: 600;
  color: var(--ink); text-align: end; z-index: 2;
  text-shadow: 0 1px 4px rgba(0,0,0,.5);
}
[data-coverart="true"] .fnd3a__cover-lines li::before {
  content: "—"; margin-inline-end: 6px; color: var(--crimson);
}
[data-coverart="true"] .fnd3a__cover-name {
  position: absolute; bottom: 10%; inset-inline-start: 14px;
  font-family: var(--font-ar-display); font-weight: 1000;
  font-size: clamp(40px, 6vw, 80px); line-height: 0.85;
  color: #fff; mix-blend-mode: difference;
  pointer-events: none; z-index: 2;
}
[data-coverart="true"] .fnd3a__cap { display: none; } /* cover-name replaces caption */
[data-coverart="true"] .fnd3a__photo { filter: contrast(1.06) saturate(1.0) brightness(1.0); }
```
**Reference**: Wave 1 typography REF 10 (NYT Magazine text-as-image cover treatments) — https://www.nytimes.com/section/magazine · Wave 1 arabic REF 9 (Brownbook bilingual masthead) — https://brownbook.me/about/ · Wave 1 typography REF 14 (Pentagram numeral-as-protagonist).
**Effort**: ~3hr (most of which is *not shipped* until photo arrives — flip the data attribute and styles activate)
**RTL safe**: yes — uses `inset-inline-*` exclusively; `mix-blend-mode: difference` is direction-agnostic.
**prefers-reduced-motion**: no animation in this move; static composition.
**Asset dependency**: **needs commissioned photo** (1600×2000 4:5 with clear bottom-third for the name overlay). Today shipping the toggle code is fine; the data attribute stays `"false"` until photo upload.

---

# Top 3 Ranked

Ranked by `(perceived craft lift) ÷ (engineering hours + asset risk)` for almobadir's editorial-Saudi audience, with Commit-1 vanity cuts already in:

### 1. MOVE 1 — Counter roll + name kashida pulse on entry
The cinematic moment of arrival the section currently lacks. Counters going `0 → 6 / 325 / 5,000` synchronized with a 700ms kashida swell on the Arabic name reads as deliberate, editorial, native to Cairo Variable. Lands in 2hr, ships today, works with current photo, RTL-clean. Replaces three lifeless never-changing stats with a "the page just opened for you" beat. Ship first.

### 2. MOVE 3 — Khatt-tradition Arabic quote mark with stroke-draw
Replaces the European « guillemet — borrowed from Latin typesetting — with a hand-drawn Arabic *gulnar* rosette inspired by Khatt Foundation Naskh tradition. Single-touch SVG (no font license needed), draws in 900ms on entry, ends a tap before the quote text becomes legible. Costs 1.5hr. The detail Arabic-literate readers will screenshot. Pure cultural fit; zero asset dependency.

### 3. MOVE 6 — Eastern-Arabic numerals in the stats counters
30 minutes of work to swap `Math.round` for `Intl.NumberFormat('ar-SA-u-nu-arab')`. Counter rolls keep their motion; the digits arrive as ٦ / ٣٢٥ / ٥٫٠٠٠ instead of 6 / 325 / 5,000. Signals "Arabic-first publication" the way Eastern-Arabic numerals on Aramco Vision 2030 covers do. Highest ratio of cultural authenticity per minute on the entire list. Combine with MOVE 1 for 2.5hr total ship.

**Top 3 budget:** ~4 hours engineering. No new dependencies. No new assets. RTL-clean. Reduced-motion safe. Ship as a single Commit 2.

---

# Asset-Dependency Roadmap

| Move | Today (150×150 AI JPEG) | After commissioned 1600×2000 |
|------|--------------------------|------------------------------|
| 1 — counter roll + kashida pulse | ✓ ships today | ✓ unchanged |
| 2 — duotone → color thaw | ✓ ships today (duotone hides AI artifacts) | ✓ better — real skin in color phase |
| 3 — Arabic quote mark draw | ✓ ships today | ✓ unchanged |
| 4 — follow-grid hover previews | ✓ ships today (preview is platform content) | ✓ unchanged |
| 5 — magnetic Calendly card | ✓ ships today | ✓ unchanged |
| 6 — Eastern-Arabic numerals | ✓ ships today | ✓ unchanged |
| 7 — paper-grain drift on scroll | ✓ ships today | ✓ unchanged |
| 8 — magazine-cover treatment | ✗ ship code, gate via data-coverart="false" | ✓ flip attribute on |

Everything except MOVE 8 ships in Commit 2 today. MOVE 8 is shovel-ready, attribute-gated, waiting on the photo session.

---

# 200-Word Summary

The FOUNDER section's chassis is editorial-clean after Commit 1 (verified-spin badge + AUTHORITY block removed). The cinematic budget those deletions freed has not yet been spent. Eight WOW moves are proposed. Three (MOVES 1, 3, 6) ship today in ~4 hours, work with the current AI-stylized 150×150 JPEG, and lift the section from "competent template" to "considered editorial." MOVE 1 (counters roll 0 → 6/325/5,000 in tabular-num lockstep while the name kashida-pulses) gives the section the moment-of-arrival it currently lacks. MOVE 3 replaces the borrowed European « with a hand-drawn Arabic *gulnar* mark in the Khatt Foundation Naskh tradition — the detail Arabic-literate readers screenshot. MOVE 6 swaps the stat counters to Eastern-Arabic numerals (٦/٣٢٥/٥٫٠٠٠) in 30 minutes, signaling Arabic-first publication. MOVES 2, 4, 5, 7 ship in week 2 (~6 more hours). MOVE 8 is shovel-ready code gated by a `data-coverart="false"` attribute, waiting on the commissioned 1600×2000 photo session — flip the attribute when the file lands and the section transforms into an issue-001 magazine cover. Zero new dependencies, zero sound, every move has `prefers-reduced-motion` fallback and is RTL-clean.
