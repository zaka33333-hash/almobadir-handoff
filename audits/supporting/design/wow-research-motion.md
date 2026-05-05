# Wow-Effect Recon — Motion & Scroll Choreography

**Compiled:** 2026-04-26
**For:** Almobadir wow-effect audit (build phase)
**Scope:** 20 specific motion moves from 2024-2026 cinematic-grade sites, scored for portability into Almobadir's existing IntersectionObserver + scroll chassis.
**Method:** WebFetch + WebSearch on documented techniques (CSS-Tricks, Codrops, Awwwards case studies, MDN, vendor docs), live Playwright screenshots where the site rendered (Lenis homepage, rauno.me/craft), domain-knowledge synthesis where SPAs blocked content extraction.
**Bias:** Each move is a *specific moment* with a named primitive, not "Site X feels nice." Heavy WebGL / asset-dependent moves are flagged but not recommended unless the analog works without that dependency.

---

## REF 1: Apple iPhone / AirPods Pro — canvas-pinned image-sequence scroll
- **Source URL:** https://www.apple.com/iphone-16-pro/ (and the long-running airpods-pro precursor)
- **Screenshot:** see CSS-Tricks article reference; Apple page is JS-hydrated and blocks WebFetch
- **The technique:** A `position: sticky` (or `position: fixed`) `<canvas>` stays viewport-anchored; a tall scroll-spacer parent maps `scrollTop / maxScroll` to a frame index across a numbered image sequence (`0001.jpg`...`0148.jpg`). `requestAnimationFrame` paints `drawImage(frames[i])` on every tick. Apple uses canvas (not `<video>`) because `<video>` cannot be advanced inside `rAF` and produces visible jitter on scroll.
- **Implementation cost:** **heavy** — needs 80-200 numbered renders; ~50MB asset weight even compressed. WITHOUT the assets it is medium (you'd substitute a sprite-sheet or 12-frame icon flip).
- **Code sketch:**
  ```js
  const frames = Array.from({length: 148}, (_,i) => loadImg(`f${String(i+1).padStart(4,'0')}.jpg`));
  const sticky = document.querySelector('.pin');
  window.addEventListener('scroll', () => {
    const r = sticky.getBoundingClientRect();
    const p = clamp(-r.top / (sticky.offsetHeight - innerHeight), 0, 1);
    requestAnimationFrame(() => ctx.drawImage(frames[Math.floor(p*147)], 0, 0));
  });
  ```
- **Almobadir analog:** Method section already pins. Replace the 5 static frames with a 24-frame *icon-only* sequence (e.g., the central pictogram morphs through 24 SVG states the brand designer can ship as a single sprite). RTL-safe — the morph is in-place. Cost without renders: medium.

---

## REF 2: Apple Vision Pro — sticky video scrub via `currentTime`
- **Source URL:** https://www.apple.com/apple-vision-pro/
- **The technique:** A muted `<video preload="auto">` sits inside a `position: sticky; top: 50vh` container. A scroll handler sets `video.currentTime = (scrollProgress * video.duration)`. The video itself never plays; scroll IS the playhead. Pure CSS variants now exist using `animation-timeline: scroll()` to drive a CSS variable that maps to the video element via JS shim.
- **Implementation cost:** **medium** if you have an asset; **heavy** if you have to commission/render one.
- **Code sketch:**
  ```js
  const v = document.querySelector('video.scrub');
  v.addEventListener('loadedmetadata', () => {
    addEventListener('scroll', () => {
      const p = clamp((-v.parentElement.getBoundingClientRect().top) / (v.parentElement.offsetHeight - innerHeight), 0, 1);
      requestAnimationFrame(() => v.currentTime = p * v.duration);
    });
  });
  ```
- **Almobadir analog:** Founder section. A 6-second `<video>` of the founder turning toward camera, scrubbed across the section's scroll height — feels like the page "introduces" him as you arrive. Skip if no founder video exists. RTL-safe.

---

## REF 3: CSS-only `animation-timeline: view()` reveals (Cyd Stumpel pattern)
- **Source URL:** https://cydstumpel.nl/start-using-scroll-driven-animations-today/ (referenced on Awwwards SOTD)
- **The technique:** Native CSS scroll-driven animation. No JS. `animation-timeline: view()` ties an animation's progress to the element's traversal of the viewport, scoped via `animation-range: cover 10% contain 90%`. Wrap in `@media (prefers-reduced-motion: no-preference)` and you get free a11y. Chrome/Edge ship it; Firefox flagged; Safari needs polyfill.
- **Implementation cost:** **trivial** (CSS-only, ~6 lines per element).
- **Code sketch:**
  ```css
  @media (prefers-reduced-motion: no-preference) {
    .reveal {
      animation: rise linear both;
      animation-timeline: view();
      animation-range: cover 0% cover 40%;
    }
    @keyframes rise { from { opacity:0; transform: translateY(24px) } to { opacity:1; transform: none } }
  }
  ```
- **Almobadir analog:** Replace the JS IntersectionObserver in `v3.js` for `.reveal-on-scroll` patterns (founder, network, content) with this CSS primitive on Chromium; keep IO as fallback for Safari. Result: smoother fades that *progress* with scroll instead of binary toggling. RTL-safe (timeline is vertical).

---

## REF 4: Linear feature pages — staggered row reveal at 30% threshold
- **Source URL:** https://linear.app/features (and per-feature pages, e.g., `/insights`)
- **The technique:** IntersectionObserver fires when `intersectionRatio >= 0.3`. The container gets `[data-in-view]`; child rows are styled `transition: transform .55s var(--ease-glide), opacity .55s; transition-delay: calc(var(--i) * 60ms)` and translate from `translateY(12px)` opacity 0 → 0. The 60ms stagger across 6-9 rows is the signature beat — fast enough to feel responsive, slow enough that the eye perceives sequence.
- **Implementation cost:** **trivial** — Almobadir's IO already exists; just add `--i` index var on each child.
- **Code sketch:**
  ```html
  <ul data-reveal>
    <li style="--i:0">…</li><li style="--i:1">…</li><li style="--i:2">…</li>
  </ul>
  <style>
    [data-reveal] li { opacity:0; transform: translateY(10px); transition: .55s var(--ease-glide); transition-delay: calc(var(--i) * 60ms); }
    [data-reveal][data-in-view] li { opacity:1; transform: none; }
  </style>
  ```
- **Almobadir analog:** Network section (logo grid) and Content section (article cards). Replace current uniform fade with 60ms-stagger reveal per row. RTL needs no changes — vertical motion only.

---

## REF 5: Vercel — animated mesh-gradient hero (Stripe minigl pattern)
- **Source URL:** https://vercel.com (and the canonical https://stripe.com from which the technique propagated)
- **The technique:** ~10kb WebGL "minigl" shader on a single `<canvas>` painting 4-color mesh gradient driven by Fractal Brownian Motion noise; multiple Simplex-noise octaves at decreasing amplitude produce wispy color drift. CPU-cheap (GPU does it), no scroll dependency, just an idle ambient layer.
- **Implementation cost:** **medium** — adds ~12kb gz and a canvas + shader file. Simpler CSS-only fallback exists (`background: conic-gradient(...); animation: hue 40s linear infinite`) but reads as obviously CSS, not ambient.
- **Code sketch:**
  ```js
  import { Gradient } from './minigl-gradient.js';
  const g = new Gradient(); g.initGradient('#hero-canvas');
  // Fragment shader does FBM; JS just kicks off rAF loop.
  ```
- **Almobadir analog:** Hero `.hero3` background. Currently hue-drifts via CSS keyframes (60s loop, audit flagged as too slow). Swap for minigl. Brand-color compatible (passes a 4-color array). RTL-safe (no directional motion).

---

## REF 6: rauno.me — horizontal scroll feed with cursor crosshair
- **Source URL:** https://rauno.me (verified live: see screenshot at `screenshots/motion-refs/rauno-craft.jpeg`)
- **The technique:** Page-level `overflow-x: scroll` with `scroll-snap-type: x mandatory` and large image cards as snap targets. A custom cursor (`mix-blend-mode: difference` crosshair) follows pointer via JS `pointermove → translate(x,y)`. Wheel events on horizontal sections re-routed (`e.preventDefault(); el.scrollLeft += e.deltaY`) so vertical wheels move horizontally. Lenis can also do this.
- **Implementation cost:** **medium** — wheel reroute + cursor element are 30-50 LOC.
- **Code sketch:**
  ```js
  document.addEventListener('pointermove', e => {
    cursor.style.transform = `translate(${e.clientX}px, ${e.clientY}px)`;
  });
  rail.addEventListener('wheel', e => {
    if (Math.abs(e.deltaY) > Math.abs(e.deltaX)) { e.preventDefault(); rail.scrollLeft += e.deltaY; }
  }, { passive: false });
  ```
- **Almobadir analog:** Network section (current logo grid). Convert to horizontal mandatory-snap rail of partner-portrait cards. **RTL critical:** Arabic readers expect rightward primary motion to map to "next" — set `dir="rtl"` on the rail and `scroll-snap-align: start` works correctly because snap is logical-property aware. Test on a real RTL browser.

---

## REF 7: Igloo Inc — WebGL UI with text glitch shaders (procedural enclosures)
- **Source URL:** https://igloo.inc (case study: https://www.awwwards.com/igloo-inc-case-study.html, threejs forum thread)
- **The technique:** Entire UI rendered in WebGL (not DOM). Text glitches use SDF (signed-distance field) texture offsets — letters appear to scramble/dissolve without forcing browser re-layout. Procedural ice-crystal generation grows geometry inside container shapes. Volume-data exporter (custom VDB-to-browser) compresses particle fields below typical image weight.
- **Implementation cost:** **heavy** — requires Three.js (~150kb), shader expertise, asset pipeline. Almost certainly overkill for Almobadir.
- **Code sketch:** intentionally not portable.
- **Almobadir analog:** Don't ship this. The portable lesson is *one* idea: text scramble on a single hero word (no WebGL needed — see REF 13).

---

## REF 8: Arc Browser — `<video>` UI demos that auto-play in viewport
- **Source URL:** https://arc.net (and dia.com, the Browser Company successor)
- **The technique:** Each feature block hosts a muted `<video autoplay loop muted playsinline>` that an IntersectionObserver pauses when offscreen and `currentTime = 0; play()` when entering. The videos are tight 4-8s loops of UI gestures, edge-rounded with `border-radius: 18px` matching macOS app aesthetic.
- **Implementation cost:** **medium** — JS observer + video assets. Save ~70% bandwidth vs. naive autoplay.
- **Code sketch:**
  ```js
  const io = new IntersectionObserver(entries => {
    entries.forEach(e => {
      const v = e.target;
      if (e.isIntersecting) { v.currentTime = 0; v.play(); } else v.pause();
    });
  }, { threshold: 0.5 });
  document.querySelectorAll('video.demo').forEach(v => io.observe(v));
  ```
- **Almobadir analog:** Method section frames 2-4 each gain a 5-second loop video showing the *real* studio process (camera tracking shot, founder editing). Replaces some of the SVG decorative animations the audit flagged for removal. RTL-safe.

---

## REF 9: Lenis — smooth-scroll engine that doesn't break native scroll
- **Source URL:** https://lenis.darkroom.engineering (verified: see `screenshots/motion-refs/lenis-hero.jpeg`)
- **The technique:** 3kb library that intercepts wheel/touch events, applies a damped lerp to scroll position via `scrollTo` (NOT transform on `<body>` — the older Locomotive approach broke `position: sticky` and `100vh`). Plays nicely with `scroll-snap`, anchor links, GSAP ScrollTrigger. Single config knob: `lerp` (0.05-0.1 sweet spot).
- **Implementation cost:** **trivial** — 5 lines to install + init.
- **Code sketch:**
  ```js
  import Lenis from 'lenis';
  const lenis = new Lenis({ lerp: 0.08, autoRaf: true });
  // That's it. Existing scroll handlers continue to work.
  ```
- **Almobadir analog:** Site-wide. Currently feels Apple-esque on Mac trackpads and *jagged* on Windows mouse wheels. Lenis levels both inputs. Respect `prefers-reduced-motion` — disable Lenis when set. RTL-safe (vertical scroll only).

---

## REF 10: Supabase Launch Week — timeline scroll with "today" indicator
- **Source URL:** https://supabase.com/launch-week (and `/launch-week/15`, `/x`)
- **The technique:** Vertical timeline of day cards (Day 01 → Day 05). A `position: sticky` "TODAY" pill on the left rail uses `scroll-driven animation` to slide down by `Y%` matching scroll progress through the timeline. Each card uses IntersectionObserver to flip from "preview" (blurred filter) → "revealed" (filter:none) with a 600ms ease-out transition once its server timestamp ≤ now.
- **Implementation cost:** **medium** — sticky pill + filter transition + IO; no library required.
- **Code sketch:**
  ```css
  .today-pill { position: sticky; top: 50vh; animation: slide linear; animation-timeline: view(--rail); }
  @keyframes slide { to { transform: translateY(calc(100% - 1lh)); } }
  .day-card { filter: blur(8px); transition: filter .6s ease-out; }
  .day-card[data-revealed] { filter: none; }
  ```
- **Almobadir analog:** Method section progress rail. Currently it's a static numbered indicator. Convert to a sticky "current step" pill that *slides* between frames as the user scrolls. RTL: pill goes on right rail, `transform: translateY` unchanged.

---

## REF 11: Things.app — every state transition is a smooth interpolation
- **Source URL:** https://culturedcode.com/things/
- **The technique:** No "scroll moves" per se — but a *philosophy* worth porting: every UI state change uses spring physics, never instant snap. Marketing site demos this with hover states that *spring* to a new layout (`translateY`, blur, scale change together) using a custom-tuned cubic-bezier resembling `cubic-bezier(0.34, 1.56, 0.64, 1)` (overshoot spring).
- **Implementation cost:** **trivial** — add one easing token.
- **Code sketch:**
  ```css
  :root { --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1); }
  .card:hover { transform: translateY(-4px) scale(1.02); transition: transform .42s var(--ease-spring); }
  ```
- **Almobadir analog:** Network and Content card hovers. The audit notes 5 different durations and zero overshoot — adding a single `--ease-spring` token and using it on cards gives that "tactile" feel. RTL-safe.

---

## REF 12: Figma Config '25 microsite — scroll-transform parallax + marquee
- **Source URL:** https://config.figma.com (the technique is now a built-in primitive: "Scroll Transform" in Figma Sites)
- **The technique:** Three layers per section, each transforming at different rates (`translateY(scrollPx * speed)` where speed = 0.2 / 0.5 / 0.9). The depth illusion is sold by making the slowest layer also lightly blurred and the fastest layer crisp. Marquee runs underneath via `@keyframes` translateX with `animation-play-state: paused` toggled by IO when offscreen.
- **Implementation cost:** **medium** if hand-rolled, **trivial** if you adopt CSS scroll-driven (`animation-timeline: scroll()`).
- **Code sketch:**
  ```css
  .layer-back  { animation: parallax linear; animation-timeline: scroll(); --rate: -100px; }
  .layer-front { animation: parallax linear; animation-timeline: scroll(); --rate: -300px; }
  @keyframes parallax { to { transform: translateY(var(--rate)); } }
  ```
- **Almobadir analog:** Manifesto section background. Currently flat. Add 3 parallax layers (texture, type ornament, gradient) — gives weight without changing content. RTL: pure vertical, sign-flip not needed.

---

## REF 13: GitHub Copilot — perspective-grid hero converging to product
- **Source URL:** https://github.com/features/copilot
- **The technique:** SVG perspective grid in hero, lines drawn with `stroke-dasharray: 1; stroke-dashoffset: 1` then animated to 0 on load (`animation: draw 1.4s var(--ease-out) forwards`). Inputs (icons) at the grid's outer edges fade in staggered by 80ms, all converging on a center "product" output card that scales from 0.92 → 1 with a subtle shadow lift. Tells a metaphor (chaos → order) in 3 seconds without copy.
- **Implementation cost:** **medium** — SVG paths + a stagger timeline; no library.
- **Code sketch:**
  ```css
  .grid-line { stroke-dasharray: 1; stroke-dashoffset: 1; animation: draw 1.4s var(--ease-out) forwards; animation-delay: calc(var(--i) * 60ms); }
  @keyframes draw { to { stroke-dashoffset: 0; } }
  .input-icon { opacity: 0; animation: fade-in .5s var(--ease-out) calc(800ms + var(--i)*80ms) forwards; }
  ```
- **Almobadir analog:** Hero. Replace static brand mark with a 1.5s convergence animation: client thumbnails / brief icons converge into a center video frame. Tells the studio's "many inputs, one cinematic output" thesis. RTL: mirror the convergence (top-right → top-left etc.) by reading `dir`.

---

## REF 14: Notion / Notion Calendar — instant interactions, *no* animation on common actions
- **Source URL:** https://www.notion.com/calendar
- **The technique:** Counter-intuitive but documented by Rauno: high-frequency interactions (opening a menu, typing) get **0ms transitions** because animation feels slow when repeated 100x/day. Reserve animation for low-frequency, "moment of delight" actions: opening the app for the first time today, completing a task, navigating to a new view.
- **Implementation cost:** **trivial** — it's a deletion / restraint principle.
- **Code sketch:**
  ```css
  .menu, .toast, .tooltip { transition: opacity .12s linear; } /* almost-instant */
  .hero-reveal, .completion-celebration { transition: all .8s var(--ease-glide); } /* cinematic */
  ```
- **Almobadir analog:** Audit found 27 distinct durations. Group them: instant (≤150ms) for hover/focus, base (320ms) for cards/forms, cinematic (700-1000ms) for entrance reveals only. Three durations, not 27.

---

## REF 15: Awwwards SOTD pattern — text scramble on hero word (SDF-cheap version)
- **Source URL:** https://www.awwwards.com/websites/scrolling/ (recurring pattern across 2025 SOTD: Heimdall Power, Efimov Audio, Stefan Vitasović portfolio)
- **The technique:** The hero word's letters cycle through random unicode (often glitch-block U+2580-258F) for 80ms each, settling into the real letter with 40ms stagger. JS-only. Looks expensive, costs ~1kb. Fires on page load AND on hover for re-trigger.
- **Implementation cost:** **trivial** — ~30 LOC vanilla JS.
- **Code sketch:**
  ```js
  function scramble(el, text, dur=900) {
    const glyphs = '▒▓█◆◇★';
    const start = performance.now();
    (function tick(t){
      const p = Math.min((t-start)/dur, 1);
      const out = [...text].map((c,i) => p*text.length > i ? c : glyphs[Math.random()*glyphs.length|0]).join('');
      el.textContent = out;
      if (p < 1) requestAnimationFrame(tick);
    })(start);
  }
  ```
- **Almobadir analog:** Hero h1 ("لكل قصة بحاجة لرواية" / "Every story needs a teller") on first scroll past 20%. RTL: the technique works on Arabic glyphs identically — just feed `text` containing Arabic characters; `[...text]` correctly handles them in modern Chromium. Test that the random glyph palette includes Arabic visual filler, not just Latin blocks.

---

## REF 16: Raycast — keyboard-key cascade reveal
- **Source URL:** https://www.raycast.com
- **The technique:** Each key in the keyboard mockup gets `opacity: 0; transform: translateY(8px) rotateX(-15deg); transform-origin: bottom`. On IO trigger they animate to identity, but the stagger is 2D — keys reveal in *physical* keyboard order (top-left → bottom-right diagonal sweep) not DOM order. Achieved by computing `--delay: calc((var(--row) * 60ms) + (var(--col) * 30ms))`.
- **Implementation cost:** **trivial** — CSS custom-prop math, no JS once the attrs are set.
- **Code sketch:**
  ```html
  <kbd style="--row:0;--col:0">Q</kbd> <kbd style="--row:0;--col:1">W</kbd> ...
  <style>
    kbd { opacity: 0; transform: translateY(8px) rotateX(-15deg); transition: 480ms var(--ease-out); transition-delay: calc(var(--row)*60ms + var(--col)*30ms); }
    [data-in-view] kbd { opacity: 1; transform: none; }
  </style>
  ```
- **Almobadir analog:** Network grid (logo wall) — sweep reveal from top-leading corner. RTL: flip the diagonal — `calc((rows - row) * 60ms + (cols - col) * 30ms)` when `dir=rtl` so the sweep enters from top-right.

---

## REF 17: Lusion — scroll-tied 3D camera dolly (lite version)
- **Source URL:** https://lusion.co
- **The technique (full):** Three.js scene, `camera.position.z = THREE.MathUtils.lerp(start, end, scrollProgress)`. The "lite" version that's portable: a **CSS-only** version where `transform: perspective(1200px) translateZ(calc(var(--p) * -300px))` is driven by a `--p` variable updated from a single `scroll` listener (or via `animation-timeline: scroll()`). Gives the viewer a sense of moving INTO the page.
- **Implementation cost:** **medium** — zero-asset CSS version is trivial; full Three.js version is heavy.
- **Code sketch:**
  ```css
  .stage { perspective: 1200px; }
  .stage-content {
    animation: dolly linear; animation-timeline: scroll();
    transform-style: preserve-3d;
  }
  @keyframes dolly { to { transform: translateZ(-300px) rotateX(8deg); } }
  ```
- **Almobadir analog:** Hero → Manifesto handoff. Currently a static section break. Add a 100vh "dolly" pin where the hero card pushes back in Z while manifesto content rises forward. RTL: `rotateX` is z-axis-tilt only, no horizontal sign-flip needed.

---

## REF 18: Framer.com — sticky cards that "stack" as you scroll
- **Source URL:** https://framer.com
- **The technique:** Each card is `position: sticky; top: 80px`. As you scroll, the next card slides up *over* the previous. Combined with `transform: scale(calc(1 - 0.05 * var(--depth)))` (driven by IntersectionObserver depth count) cards behind shrink slightly, creating a "deck" stack. No library required.
- **Implementation cost:** **trivial** — 8 lines of CSS + 12 lines of JS for depth tracking (or pure CSS with `animation-timeline: view()`).
- **Code sketch:**
  ```css
  .stack-card { position: sticky; top: 80px; transition: transform .6s var(--ease-glide); }
  .stack-card[data-depth="1"] { transform: scale(.96); }
  .stack-card[data-depth="2"] { transform: scale(.92); filter: blur(2px); }
  ```
- **Almobadir analog:** Content section (article cards). Currently each card scrolls past independently. Stacking them creates a "portfolio deck" reveal that emphasizes depth of work. RTL-safe.

---

## REF 19: GSAP Flip — element morphs across DOM positions
- **Source URL:** https://tympanus.net/codrops case studies (recurring in 2024-2026)
- **The technique:** `Flip.getState(el)` snapshots geometry. Move the element in the DOM (different parent, different position). `Flip.from(state, { duration: 1, ease: 'power3.inOut' })` interpolates between captured rect → new rect using FLIP technique (First/Last/Invert/Play). The illusion: a thumbnail "expands" into a hero image when clicked, using its actual self, not a clone.
- **Implementation cost:** **medium** — needs GSAP Flip plugin (~12kb gz). Modern browsers also have native View Transitions API doing similar — see REF 20.
- **Code sketch:**
  ```js
  const state = Flip.getState('.thumb-clicked');
  document.querySelector('.hero-slot').appendChild(thumbEl);
  Flip.from(state, { duration: .9, ease: 'power3.inOut', absolute: true });
  ```
- **Almobadir analog:** Founder section → bio modal. Founder portrait clicks open a fullscreen bio; the portrait *itself* expands to fill — feels native, not popup-y. Native API alternative below.

---

## REF 20: View Transitions API — native page-section morph (Chrome 111+)
- **Source URL:** MDN reference + 2024-2025 "view transitions in the wild" articles (Stripe Press, Astro showcase)
- **The technique:** Browser-native morph between DOM states. Wrap the state change in `document.startViewTransition(() => updateDOM())`. Style the morph via pseudo-elements: `::view-transition-old(root)` and `::view-transition-new(root)` with `view-transition-name: hero-image` on matched elements. No library.
- **Implementation cost:** **trivial** — 5 lines. Falls back gracefully (no animation on Safari < 18.2).
- **Code sketch:**
  ```js
  function navigate(toUrl) {
    if (!document.startViewTransition) return location.href = toUrl;
    document.startViewTransition(() => { /* swap DOM */ });
  }
  ```
  ```css
  ::view-transition-old(hero), ::view-transition-new(hero) { animation-duration: .6s; }
  .founder-photo { view-transition-name: founder; }
  ```
- **Almobadir analog:** Single-page anchor jumps (Hero → Method, etc.). Each section's heading gets `view-transition-name`; jumps between sections morph the headings instead of jump-cutting. RTL-safe (browser handles flow direction).

---

## Top 8 portable for Almobadir

Ranked by ratio of (visual punch) ÷ (implementation effort + risk to existing chassis), with Almobadir's reduced-motion-respecting and RTL constraints already weighted in:

1. **REF 3 — CSS `animation-timeline: view()` reveals.** Replaces JS IO logic with native CSS. Smoother, smaller, free a11y. Lowest risk, highest leverage. Ship first.
2. **REF 4 — 60ms-stagger row reveals (Linear).** Already 90% of Almobadir's IO chassis; just add `--i` and `transition-delay`. Network + Content sections light up immediately.
3. **REF 11 — `--ease-spring` overshoot token (Things).** Single token addition. Audit's existing 5 different card-hover durations collapse to one curve and feel 10x more crafted.
4. **REF 14 — Three-tier duration discipline (Notion).** This is *the* fix to the audit's "27 distinct durations" finding. Not a new move, but a constraint that makes everything else read as deliberate.
5. **REF 9 — Lenis smooth scroll.** 3kb. Levels Mac/Windows scroll feel. Biggest "people screenshot this" perceptual win for users on Windows mouse wheel — which is most of the audience.
6. **REF 15 — Text scramble on hero h1.** ~30 LOC. Works on Arabic. One showstopper moment on first scroll. Almost free.
7. **REF 10 — Sticky "current step" pill on Method timeline.** Builds on existing pinned section. The audit already wants a clearer scroll cue; this delivers it cinematically.
8. **REF 18 — Sticky-card stack on Content.** CSS-only. Turns the article grid into a portfolio deck without changing content or copy.

Honorable mentions (not in top 8 because of asset cost or scope risk): REF 1 (canvas image-sequence — too heavy without renders), REF 5 (mesh gradient — would replace existing hero, not augment), REF 13 (Copilot grid — needs custom illustration work), REF 20 (View Transitions — Almobadir is single-page, lower payoff than on multi-page sites).

---

## 200-word summary

Twenty 2024-2026 motion moves audited, scored, and grounded in named primitives. The headline finding for Almobadir: **the chassis is already there**. IntersectionObserver, sticky pins, a glide easing token, reduced-motion guard — Almobadir has the bones top sites use. What's missing is *discipline* and one or two cinematic moments per page.

The cheapest wins are constraint-shaped: collapse 27 durations to three (REF 14), introduce a single overshoot easing token for tactility (REF 11), and migrate IO-driven reveals to native CSS `animation-timeline: view()` where Chromium permits (REF 3). These three together cost a day of work and lift the entire site's motion polish to bench-grade.

The cinematic moments are surgical: a 60ms-stagger row reveal on Network/Content cards (REF 4), Lenis for cross-platform scroll feel (REF 9), a text scramble on the hero h1 that works in Arabic (REF 15), and a sticky "current step" pill on Method that slides with scroll progress (REF 10).

Skip the heavy moves. No WebGL fluid simulations, no Three.js camera dollies, no canvas image-sequences without renders the studio doesn't yet own. Ship the eight portable refs above and Almobadir reads as 2026 craft, not 2022 portfolio.

---

## Sources

- [Apple-style scroll animation technique — CSS-Tricks](https://css-tricks.com/lets-make-one-of-those-fancy-scrolling-animations-used-on-apple-product-pages/)
- [Recreating Apple's Vision Pro Animation in CSS — CSS-Tricks](https://css-tricks.com/recreating-apples-vision-pro-animation-in-css/)
- [Scroll-revealed WebGL Gallery with GSAP, Three.js, Astro — Codrops](https://tympanus.net/codrops/2026/02/02/building-a-scroll-revealed-webgl-gallery-with-gsap-three-js-astro-and-barba-js/)
- [Stefan Vitasović Portfolio 2025 case study — Codrops](https://tympanus.net/codrops/2025/03/05/case-study-stefan-vitasovic-portfolio-2025/)
- [Invisible Details of Interaction Design — Rauno Freiberg](https://rauno.me/craft/interaction-design)
- [Start using Scroll-driven animations today — Cyd Stumpel](https://cydstumpel.nl/start-using-scroll-driven-animations-today/)
- [Lenis Smooth Scroll — darkroom.engineering](https://github.com/darkroomengineering/lenis)
- [How to create the Stripe Website Gradient Effect — bram.us](https://www.bram.us/2021/10/13/how-to-create-the-stripe-website-gradient-effect/)
- [CSS scroll-driven animations — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations)
- [Igloo Inc case study — Awwwards](https://www.awwwards.com/igloo-inc-case-study.html)
- [Igloo Inc procedural crystals — WebGPU.com](https://www.webgpu.com/showcase/igloo-inc-procedural-crystals/)
- [Awwwards Best Scroll Websites](https://www.awwwards.com/websites/scrolling/)
- [Active Theory case studies on Medium](https://medium.com/active-theory)
- [Things by Cultured Code — features](https://culturedcode.com/things/features/)
- [Figma Sites announcement — Figma blog](https://www.figma.com/blog/introducing-figma-sites/)
- [Building Smooth Scroll in 2025 with Lenis — Edoardo Lunardi](https://www.edoardolunardi.dev/blog/building-smooth-scroll-in-2025-with-lenis)
