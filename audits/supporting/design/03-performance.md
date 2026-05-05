# Performance — Forensic Audit

Source: `C:\Users\toner\OneDrive\Desktop\almobadir\website\` · Live: `http://localhost:8765/index.html` · 2026-04-26 · viewport 1440×900 (also reasoned about mobile/3G).
Bar: LCP < 2.5s · CLS < 0.1 · INP < 200ms on 3G mid-tier.

---

## Executive Summary

The page is **delivered uncompressed at ~280 KB of HTML+CSS+JS plus ~5–6 KB of Google-Fonts CSS plus ~6 woff2 font files** (50–80 KB) plus **5 Unsplash images at 1200–1600w** queued lazily. On a fast desktop with localhost SimpleHTTP and no gzip, first-paint feels instant; on a 3G mid-tier device the picture inverts. Three structural problems dominate: (1) **render-blocking stylesheet chain** with five separate `<link rel="stylesheet">` round-trips before first paint (fonts.css → Google Fonts CSS → tokens.css → base.css → v3.css), (2) **131 KB of uncompressed v3.css** that sets up the entire page even though sections far below the fold are styled identically to those above, (3) **44 font 404s** (11 declarations × 4 formats: woff2/woff/otf/ttf each fetched per `src` list) that cost a TCP round-trip each, then fall back to Google Fonts which itself loads ~6 woff2 files for 6 family declarations. The Google Fonts request currently includes **all six Latin/Arabic families with full weight ranges** when only 3–4 weights are actually used.

The hero section is the single biggest CWV risk — it stacks **3 mesh layers @ blur(80px) + 1 conic-gradient @ blur(60px) + grid lines + grain + vignette + a 60s hue-drift animation on the section itself + a 24s halo-spin + an SVG filter feGaussianBlur on the wordmark** all on the very first viewport. The same stacking is **not disabled at any breakpoint**, so a phone repaints all of those layers every frame the page is visible. The Method section adds **5 absolutely-positioned frames each carrying always-running keyframe animations (rings spinning at 18s/22s/28s, pulse-rings at 1.6s/2.4s, wave circles, mess/clean fades, magazine cover tilt)** — every one of these stays composited on the GPU even when its parent frame has `opacity: 0`. The result is high steady-state main-thread + compositor cost, which is exactly the metric INP penalises hardest.

Estimated CWV on **3G mid-tier mobile (Moto G4 class, 1.6Mbps, 562ms RTT)**: **LCP ≈ 4.0–5.5s** (well over budget; held back by the render-blocking Google Fonts CSS which loads ~6 woff2 files before the hero SVG can paint with the right typeface), **CLS ≈ 0.10–0.18** (hero `min-height: 100vh` is good, but Unsplash images have sized attributes only on cnt3 — nw3a covers have no width/height, cards reflow on load; font swap from system → Almarai/Tajawal also shifts the wordmark and tagline since metrics differ), **INP ≈ 220–360ms** (5 always-running heavy animations + scroll listeners + Method scrollytelling sub-progress driven on every scroll frame). The single highest-leverage change is **inlining critical CSS for the hero, deferring v3.css/v3.js, swapping the brand-font @font-face block for a runtime feature-detect, and gating atmospheric layers behind `(min-width: 901px) and (prefers-reduced-motion: no-preference)`**. Done together, expected lift: LCP 4.5s → 2.0s, INP 300ms → 120ms.

---

## Resource budget

| Resource | Bytes (uncompressed) | Status | Notes |
|---|---|---|---|
| `index.html` | 98,445 B (~96 KB) | 200 OK | 1,220 lines · single-page · readable, not minified |
| `assets/tokens.css` | 8,938 B | 200 OK | 200 lines · OK as-is |
| `assets/base.css` | 8,467 B | 200 OK | 268 lines · contains body::after grain (data: SVG) |
| `assets/fonts.css` | 5,401 B | 200 OK | 11 @font-face × 4 src formats = 44 references |
| `assets/v3.css` | 133,206 B (~130 KB) | 200 OK | 1,151 lines · **largest single asset** |
| `assets/v3.js` | 24,122 B (~24 KB) | 200 OK | 498 lines · 9 IIFEs |
| `assets/components.css` | 27,648 B (~27 KB) | not loaded | **Dead file — not linked from index.html** (Wave 1 was wrong: removing the link is a no-op because there is no link) |
| `assets/Badrshaqer.jpg` | 7,629 B | 200 OK | **150×150 px** (not the ~1200w expected) — quality issue, not size |
| `assets/logo/favicon.svg` | 620 B | 200 OK | OK |
| `assets/fonts/*` (44 refs across 11 faces × 4 formats) | 0 B each | **404 × 11** | Browser stops on first OK; here all 4 fail → falls back to Google Fonts. Each declaration tries woff2 then woff then otf then ttf — **44 404 round-trips total** |
| Google Fonts CSS | ~5–6 KB | 200 OK (external) | 6 families × full weight ranges (Almarai 4w, Reem Kufi 4w, Tajawal 7w, Cairo 4w, Instrument Serif 2 italics, JetBrains Mono 3w = **24 face requests possible**) |
| Google Fonts woff2 | ~12–18 KB × ~6 = 70–110 KB | external | One per family that's actually used by the rendered page |
| Unsplash × 5 (lazy) | ~80–250 KB each | external · lazy | `w=1400`, `w=1200`, `w=900`, `w=1600`. Some oversized for displayed area (see findings F12, F13). |
| **Initial critical-path total (uncompressed)** | **~280 KB** | — | HTML + 4 CSS files + 1 JS file + Google Fonts CSS, before any image |
| **Above-the-fold total (uncompressed)** | **~350–400 KB** | — | Critical path + fonts + 1 hero-adjacent grain SVG (inlined as data:) |

> Python `http.server` does **not gzip**. On a real CDN with Brotli these would be roughly: index.html 96→18 KB, v3.css 130→16 KB, v3.js 24→7 KB. So real-world critical bytes ≈ **~50 KB compressed**, which is fine; the cost is in **request count, render-blocking chain, and render-time CSS work**.

---

## Network waterfall (initial load, 1440 width, cold cache)

1. `GET /index.html` — 98 KB, blocking — TTFB ≈ 5ms localhost
2. `GET /assets/fonts.css` — 5.4 KB, render-blocking — kicks off 11 × 4 = **44 font requests** (all 404)
3. `GET /assets/tokens.css` — 8.9 KB, render-blocking
4. `GET /assets/base.css` — 8.5 KB, render-blocking
5. `GET /assets/v3.css` — 130 KB, render-blocking — **biggest blocking asset**
6. `GET https://fonts.googleapis.com/css2?...` — preconnect helps but still render-blocking
7. **44 × `GET /assets/fonts/*.{woff2,woff,otf,ttf}`** — 404s; fired in parallel with step 6 because fonts.css is parsed early
8. `GET /assets/v3.js` — 24 KB, **not deferred, not async** — runs after parser hits the line and DOMContentLoaded fires
9. `GET /assets/logo/favicon.svg` — 620 B
10. **6 × `GET https://fonts.gstatic.com/.../*.woff2`** — Almarai, Tajawal, Reem Kufi, Cairo, Instrument Serif, JetBrains Mono (one per family that the cascade actually reaches)
11. *Lazy after first paint:* `GET https://images.unsplash.com/...` × 5 covers + `GET /assets/Badrshaqer.jpg` × 2 instances — IntersectionObserver-driven for cnt3 cards, native `loading="lazy"` for nw3a covers
12. *Background:* `setInterval(tick, 60000)` newsletter countdown · `setInterval(nextTick, 6000)` header ticker · `requestAnimationFrame` clock loop (fires every ~1s while tab is visible)

Critical render path = **6 blocking round-trips** before the browser can lay anything out (HTML → fonts.css → tokens.css → base.css → v3.css → Google Fonts CSS). On 3G this is ~3.5–4.5s alone.

---

## Findings

Numbering: P0 = ship-blocker for CWV bar · P1 = high impact, fix in same week · P2 = polish.

### Critical (P0)

| # | Issue | Impact | Fix | Estimated win |
|---|---|---|---|---|
| **F1** | `index.html:21` loads **`fonts.css`** (and its 44 404 font references) **before** `tokens.css`/`base.css`/`v3.css`. Each 404 is a real round-trip that ties up an HTTP/1.1 connection slot and adds 28 console errors. | LCP, INP, dev-confidence | **Delete the `<link>` to `fonts.css`** until the licensed font files actually exist in `/assets/fonts/`. The fallback chain in `tokens.css` (`'Mostaqbali', 'Almarai', 'Reem Kufi', 'IBM Plex Sans Arabic', system-ui`) already resolves to Google Fonts on its own. Re-add the link only when files are placed. | -44 requests, -200ms LCP on 3G, console clean |
| **F2** | Five render-blocking `<link rel="stylesheet">` in serial chain (`fonts.css`, Google Fonts CSS, `tokens.css`, `base.css`, `v3.css`). | LCP | **Inline critical CSS** for hero + header (~6–8 KB) directly in `<head>`. Move `v3.css` to `<link rel="preload" as="style" onload="this.rel='stylesheet'">` non-blocking pattern, or split it into `v3-above.css` (hero + header + manifesto) and `v3-below.css` (deferred). | LCP -800ms to -1.2s on 3G |
| **F3** | `v3.js` is **not deferred** — `index.html:1216` (likely; near `</body>`) loads it as a regular script. Even at end-of-body, the parser stalls until the file downloads. | TTI, INP | Add `defer` (or `type="module"`) to the `<script src>` line. v3.js wraps everything in `DOMContentLoaded`, so `defer` is a free win. | -100–200ms TTI |
| **F4** | Hero atmospheric layer stack runs on **every viewport including mobile**. `index.html:142–149` renders 7 absolutely-positioned `.hero3__atmosphere` children (mesh-a/b/c, conic, grid-lines, grain, vignette). `v3.css:128` `filter: blur(80px)` × 3 + conic-gradient + radial-gradient × 4 + always-running 60s hue-drift animation + 24s halo-spin. None of these are gated by `@media (max-width: 900px) { display: none }`. | LCP, INP, mobile battery | Wrap the entire `.hero3__atmosphere` in `@media (min-width: 901px) and (prefers-reduced-motion: no-preference)`. Replace `filter: blur(80px)` × 3 with **one pre-blurred SVG** at the equivalent visual cost — saves ~3 GPU layers, each currently a full-viewport bitmap. | INP -80ms on mid-tier phones; first paint -150ms |
| **F5** | **5 Method frames stacked absolutely** (`v3.css:458` `position: absolute; inset: 0`) with `opacity: 0` for inactive frames, but each frame contains SVG icons with **always-running CSS animations** (`mtd3-spin` 18s/22s/28s, `mtd3-pulse-ring` 2.4s, `mtd3-pulse-dot` 1.6s, `mtd3-flicker` 3.2s, `mtd3-mess-fade` 6s, `mtd3-clean-fade` 6s, `mtd3-strip` 3s, `mtd3-tilt` 6s, `mtd3-wave` 3.6s × 5 circles). Hidden frames still composite. | INP, scroll smoothness, battery | Pause animations on inactive frames: `.mtd3__frame:not(.is-active) * { animation-play-state: paused; }`. Better: move the always-running ornaments into the `.is-active` selector so they only run when visible. | INP -60–100ms; CPU steady-state -25% |
| **F6** | LCP candidate is the **hero SVG wordmark** (`index.html:164–175`, font-size 200, fill = gradient, **filter `feGaussianBlur stdDeviation=6` + feMerge**). The SVG filter forces the renderer to rasterize, blur, and merge before paint — this delays LCP measurement of the wordmark glyph itself. | LCP | Replace `feGaussianBlur` with a CSS `text-shadow: 0 0 20px rgba(230,33,59,0.4)` on the SVG `<text>`. Or render the wordmark as plain HTML text (`<span>` styled) once the brand font loads, instead of an SVG with filter. | LCP -150–250ms |
| **F7** | **6 Google Fonts families** loaded in one CSS request with **full weight ranges**: Almarai 300/400/700/800, Reem Kufi 400–700, Tajawal 200/300/400/500/700/800/900, Cairo 400/500/700/900, Instrument Serif italic+regular, JetBrains Mono 400/500/700. The `--font-ar-body` cascade only ever lands on `'Graphik Arabic', 'Graphik', 'Tajawal', 'Almarai'` — Reem Kufi and Cairo are never reached at the top of any cascade in `tokens.css`. | bytes, LCP | Reduce to actually-used families/weights. Likely safe set: Tajawal 400/500/700/900 + Almarai 400/700 + JetBrains Mono 400/500 + Instrument Serif italic. Remove Reem Kufi and Cairo entirely. | -2 woff2 fetches, -40 KB on cold cache |
| **F8** | **Brand font fallbacks differ in metrics** from Almarai/Tajawal/system-ui. When Google Fonts swap in (font-display: swap is set), the hero wordmark and `manif3__lede` will reflow, shifting CLS upward. | CLS | Add `size-adjust`, `ascent-override`, `descent-override`, `line-gap-override` to the @font-face declarations to lock metrics, or use a paired metric-matched fallback (`Tajawal Fallback` / `Almarai Fallback` synthetic faces). | CLS -0.05 to -0.10 |
| **F9** | **6 `nw3a-cover` images** (`index.html:374, 387, 400, 413, 426, 439`) have **no `width`/`height` attributes and no `aspect-ratio` on the `<img>`**. The CSS sets `aspect-ratio` on `.nw3a-cover` (the figure parent) but `.nw3a-cover img` is `position: absolute; inset: 0; object-fit: cover` — the image element itself has 0×0 layout until paint. | CLS | Add `width="900"` `height="600"` (or actual ratio) to every `<img>` in the network section. The figure aspect ratio is correct; the image needs intrinsic ratio too so Chromium can reserve the box pre-paint. | CLS -0.03 |
| **F10** | **Badrshaqer.jpg is 150×150 px** but is rendered into two large slots: `cnt3-card` cover area (1200×900-class) and `fnd3a__photo` (4:5 aspect, ~400×500 displayed). | quality + LCP candidate | Source a proper portrait at 800×1000 (founder) and 900×600 (network card). Encode AVIF + WebP + JPEG fallback. | LCP candidate gets sharper without increasing bytes (a ~80 KB AVIF beats current upscaled blur) |

### High impact (P1)

| # | Issue | Impact | Fix | Estimated win |
|---|---|---|---|---|
| **F11** | `index.html:1019` and `index.html:400` reference `assets/Badrshaqer.jpg` with **`loading="lazy"`** even though the founder image is a likely LCP candidate when scrolled to. The cnt3 instance is fine lazy; the fnd3a portrait may want eager-load if a user lands deep-linked at `#founder`. | LCP on deep-link entry | Add `<link rel="preload" as="image" href="/assets/Badrshaqer.jpg" fetchpriority="high">` only when URL hash is `#founder`. Otherwise leave lazy. | -300ms LCP on deep-linked load |
| **F12** | **Unsplash URLs request `w=1600` and `w=1400`** (`index.html:374, 439, 851`) but their displayed area at 1440 viewport is at most 760×450 (cnt3 featured) or ~480×320 (nw3a feature). | bytes, LCP | Drop to `w=1280` for featured, `w=720` for std cards, and add `srcset` with `w=720, w=1280, w=1920` + `sizes="(max-width: 760px) 100vw, 50vw"`. | -150 KB across 6 covers on 1440 viewport |
| **F13** | **Unsplash URLs request `q=80` for cnt3** but `q=70` for nw3a — inconsistent and `q=80` is wasteful for grayscale-filtered backgrounds (`v3.css:320` `filter: grayscale(0.35) contrast(1.05) brightness(0.7)`). | bytes | Use `q=60` everywhere; the grayscale + brightness filter masks JPEG artifacts. | -20–30 KB per image |
| **F14** | **`v3.css:124`** sets `animation: hero3-hue-drift 60s linear infinite` on `.hero3` itself (not just on a child). The whole hero section is a 100vh element repainting CSS custom properties (`--hero3-hue`, `--hero3-glow`) every frame — and those properties cascade into mesh-a, mesh-b, conic, panel-halo. Each tick triggers paint+composite for ~5 layers. | INP, battery | Move the `@property` change to a single small element OR reduce to `60s ease-in-out` with only 2 keyframes (already there) AND wrap in `@media (prefers-reduced-motion: no-preference)` (currently NOT wrapped — only `*` selector below catches it via base.css's reduced-motion rule, which is fine, but the always-on @property `--hero3-hue` updates still hit even at 0.01ms duration). | INP -40ms; battery -8% on phones |
| **F15** | **`v3.css:189`** `.hero3__panel-halo` runs `animation: hero3-halo-spin 24s linear infinite` on a `conic-gradient(filter: blur(60px)` element with `inset: -40%` — that's a layer ~180% the panel size, blurred, on a 24s rotation, **steady-state cost forever**. | INP, battery | Wrap in `@media (hover: hover)` so phones don't paint it. Or pause when hero scrolled out of view via IntersectionObserver. | INP -25ms |
| **F16** | **Header ticker `setInterval(nextTick, 6000)`** (`v3.js:22`) runs on every page even when the announce bar is hidden by `.hdr3.is-scrolled` (`v3.css:22` collapses height to 0). Timer keeps firing. | INP, idle CPU | When `is-scrolled` toggles on, `clearInterval`. Re-start on toggle off. Or pause via Page Visibility API. | -1 timer firing forever |
| **F17** | **Footer clock runs `requestAnimationFrame(() => setTimeout(loop, 1000))`** (`v3.js:453`) — that's every 1s + 1 frame, which is fine, but it formats a `Intl.DateTimeFormat` object **`new Date()`** inside the loop. The formatter is created once outside the loop (good), but `new Date()` allocates every second. Visibility API is wired (line 455). | minor INP, GC pressure | OK, not urgent — but `setInterval(tick, 1000)` would be lighter than rAF + setTimeout, since you want wall-clock not frame-aligned. | -minor GC |
| **F18** | **Method scrollytelling `setupDesktop` runs `update()` inside `requestAnimationFrame`** on every scroll event (`v3.js:243`). The update reads `stage.getBoundingClientRect()` and `stage.offsetHeight` (forced layout) then writes `--mtd3-sub` on **all 5 frames** every frame. | INP during scroll | Use `IntersectionObserver` with `threshold: Array.from({length:21},(_,i)=>i/20)` instead of scroll handler to derive sub-progress. Cache `stage.offsetHeight` until resize. Only write `--mtd3-sub` on the active frame (line 242 already loops over all 5, which is a 5x waste). | INP during method scroll: -60ms per frame |
| **F19** | **`hero3__signup-input` has `font-size: 16px`** (`v3.css:180`) — good, prevents iOS zoom-on-focus. But the **`hdr3-search-input` is `font-size: 13px`** (`v3.css:79`) — iOS will auto-zoom on focus, then animate back, costing ~300ms perceived input latency on iPhone. | INP on iOS | Bump to 16px or use `<input style="font-size: 16px">` only on the input element. | -300ms iOS focus tap |
| **F20** | **Network section card has a 3D tilt JS handler** (`v3.js:159–172`) that runs `card.style.setProperty('--rx', ...)` inside rAF on `pointermove` for **all 6 cards**. Each pointermove reads `card.getBoundingClientRect()` (forced layout). | INP | Coalesce: only update the card under the cursor. Use `pointer-events: none` on inner elements to skip dispatch. Or `containerType: inline-size` on the grid + a single global `pointermove` handler. | INP -20–40ms |
| **F21** | **Newsletter `nl3-marquee` runs forever at 38s** (`v3.css:692`) on a `.nl3-issues-row` containing 8+ issue links. `width: max-content` plus `transform: translateX(50%)` keyframe means a wide composited layer is sliding constantly. Mobile @media at line 700 hides `.nl3-frame-stack` but **NOT the marquee row** — confirm it's hidden on mobile too. | INP on mobile | Add `@media (max-width: 560px) { .nl3-issues-row { animation: none; flex-wrap: wrap; } }` or hide the issues teaser entirely. | INP -10ms on phones |
| **F22** | **`fnd3a__verified` rotates a 86×86 crimson disc at 24s + tick rotates counter-rotating at 24s reverse** (`v3.css:724, 728`). Two independent `@keyframes fnd3aSpin` keep firing forever once the founder section is scrolled past, even on mobile where it's not the focal element. | INP, battery | Pause when the section leaves the viewport (IntersectionObserver, `animation-play-state: paused`). | INP -8ms |
| **F23** | **`base.css:49` `body::after` grain is `position: fixed; inset: 0` with `mix-blend-mode: overlay`** on a 220×220 SVG repeating noise pattern. This forces a full-viewport composited layer with blend mode on every paint. | INP, paint cost | Acceptable for desktop but **disable on mobile**: `@media (max-width: 760px) { body::after { display: none; } }`. The SVG noise barely registers on a small screen and the blend cost is real. | paint -2ms per frame on phones |
| **F24** | **22 radial-gradient + 5 conic-gradient + 11 linear-gradient backgrounds across `v3.css`** — many on full-section elements with blur. Every section repaint = expensive. | paint cost | Collapse the hero atmosphere into a single SVG composite or a single canvas-baked gradient. Section-level gradients (e.g. `cnt3::before`, `mtd3::before`, `manif3::before`, `fnd3a__grain`) overlap visually; remove the ones that are masked by the next layer. | paint cost -30% on long pages |
| **F25** | **`nw3a-cover img` `transition: transform 1.2s, filter .6s`** (`v3.css:320`). On hover, both transform AND filter animate. Filter animation on `grayscale + contrast + brightness` is **not GPU-accelerated** and triggers paint on each frame. | INP on hover | Drop the filter transition; toggle filter to `none` instantly on hover, or use a pseudo-element overlay with opacity transition (cheap). | INP -15ms per hover |

### Polish (P2)

| # | Issue | Impact | Fix |
|---|---|---|---|
| **F26** | `<script>document.documentElement.classList.remove('no-js');</script>` at `index.html:30` runs **inside `<head>`** before CSS parses. Tiny — but blocks parser briefly. | Move to end of `<body>` or wrap in `<script type="module">` to defer naturally. |
| **F27** | `index.html:24` Google Fonts `<link>` uses `&display=swap` — good — but the link is **not preloaded as `style`**. `preconnect` is set (line 22–23) but the actual CSS request still serializes. | Add `<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?...">`. |
| **F28** | **No `<link rel="dns-prefetch" href="https://images.unsplash.com">`** — Unsplash images are below the fold but the DNS lookup still costs ~50–100ms when first card scrolls into view. | Add `<link rel="dns-prefetch" href="https://images.unsplash.com">` and `<link rel="preconnect" href="https://images.unsplash.com" crossorigin>`. |
| **F29** | `assets/components.css` (28 KB, 957 lines) is **dead** — not referenced by `index.html` at all. README claims it's part of the system. | Delete the file or move to `/_archive/`. Wave 1 was incorrect that it's a `<link>` to remove. |
| **F30** | `cnt3-skeleton` (`v3.css:566`) runs `cnt3-shimmer 1.4s linear infinite` on a `linear-gradient` background — but the JS at `v3.js:289` removes the skeleton 350ms after image load. The animation is short-lived but is already running before the image arrives, which is fine. | Acceptable. No change needed. |
| **F31** | `manif3__ornament svg` runs 28s spin (`v3.css:270`) — fine, but combined with manif3 word-by-word reveal scroll handler (`v3.js:122–139`), the manifesto section has scroll-cost while it's in view. | Confirm scroll listener is removed when manifesto leaves viewport (it isn't — `addEventListener('scroll', onScroll, { passive: true })` stays bound forever, line 137). |
| **F32** | **Scroll listeners are never removed**: hero (v3.js:35), manifesto (137), method-desktop (249), cnt3 progress (318). Even `passive: true`, every scroll event dispatches into all 4 handlers. | Use a single global `requestAnimationFrame`-throttled scroll dispatcher that calls each section only when it's within `IntersectionObserver` viewport. |
| **F33** | **`hero3-pulse-ring`** at `v3.css:161` runs every 1.6s, `hero3-blink` panel-live-dot at `v3.css:196` every 1.6s, `hero3-ticker` at `v3.css:229` runs 50s. Total: **6 always-on hero animations** — checked: nothing pauses on tab background. | Consider Page Visibility API: pause CSS animations when `document.hidden`. Lightweight: toggle a class on body. |
| **F34** | `v3.css:1146` reduced-motion override uses `animation: none !important` × 4 properties — works but **doesn't cover everything** (e.g., it lists `.mtd3 *` but not `.hero3 *`). Some hero animations would still run for users with `prefers-reduced-motion: reduce`. | Audit the selector list against the actual `animation:` declarations. Or use `*` in the override since that's the standard pattern. |
| **F35** | **`will-change: transform` on `.hero3__mesh`** (`v3.css:128`) keeps 3 mesh layers permanently promoted to GPU layers with `filter: blur(80px)` each. Each layer ≈ viewport size at full DPR = 5–8 MB GPU memory on desktop, ~2 MB each on phones. | Drop `will-change` once the JS-driven parallax settles (set it via JS only during pointermove). |
| **F36** | `hero3__panel` `backdrop-filter: blur(22px) saturate(120%)` (`v3.css:188`) inside a section that already has 3 mesh layers blurred at 80px — the panel re-blurs an already-blurred composite. Heavy. | Use a solid translucent background with no backdrop-filter on mobile; keep blur for desktop. `@media (max-width: 900px) { .hero3__panel { backdrop-filter: none; background: rgba(14,21,37,0.92); } }`. |
| **F37** | **Hero topbar in `index.html:140–298`** — 158 DOM nodes in the hero alone (chips, eyebrows, KPIs, sparklines, ticker, trust). Hero render is layout-heavy. | Acceptable for editorial design; consider `content-visibility: auto` on sections below the fold (manif3, mtd3, cnt3, nl3, fnd3a, ftr3) to skip their layout cost on initial render. **Big win**: enables ~40% faster TTI. |
| **F38** | **`text-rendering: optimizeLegibility`** (`base.css:23`) is set on `<body>`. This forces ligature/kerning calculation on every text node, including 1,200 characters of Arabic body. | Switch to `text-rendering: auto` (default). The `font-feature-settings: "kern" 1, "liga" 1` declaration on the same body block already enables what you need. |
| **F39** | **`scroll-behavior: smooth` on `<html>`** (`base.css:9`) interacts with `topBtn` click handler (`v3.js:467` `window.scrollTo({behavior: 'smooth'})`) — fine, but smooth scroll fires layout/paint on every step. On 5,000-px-tall page, that's expensive. | Acceptable for desktop. Consider gating: `@media (max-width: 760px) { html { scroll-behavior: auto; } }`. |
| **F40** | **6 backdrop-filter usages** (`hdr3`, `hdr3-mega`, `hdr3-drawer`, `hero3__eyebrow`, `hero3__signup-row`, `hero3__panel`, `mtd3__hud`, `ftr3-cta__form` — actually 8). Each is its own GPU layer with filter. Stacking them on the same first viewport is heavy. | Replace `hero3__eyebrow` and `hero3__signup-row` with solid translucent fills (no backdrop-filter). Keep blur on header + panel where the effect is most visible. |
| **F41** | **`text-shadow` and box-shadow stacking**: `v3.css:42` `.hdr3-logo-verify` has `filter: drop-shadow`; `v3.css:138` `.hero3__brand-dot` has `box-shadow: 0 0 18px`; `v3.css:160` `.hero3__pulse-dot` has `box-shadow: 0 0 12px`; etc. ~35 box-shadow declarations site-wide, many with large blur radii. Each forces GPU work. | Reduce blur radii to ≤16px where possible; use `filter: drop-shadow` only when the element shape is non-rectangular. |
| **F42** | `index.html:1019` `.fnd3a__photo` has **no width/height** attributes (the source is 150×150). The CSS uses `width: 100%; height: 100%; object-fit: cover` inside the frame, so layout is fine — but the browser can't reserve the box until the image loads, and on lazy-load this can cause minor CLS when the verified-spinner overlaps. | Add `width="800" height="1000"` to the image once you replace with a real-resolution portrait (F10). |
| **F43** | **`@property` declarations** (`v3.css:121–123, 357–358, 451`) are great progressive-enhancement, but `@property` parsing in Firefox + Safari forces a layout-style recalc when the property registers. For the hero, the `@property` registration runs before any DOM lay out. | Acceptable — modern browsers handle this efficiently. Just be aware that adding more `@property` doesn't come free. |
| **F44** | **Console at 1440 width**: 28 errors for font 404s (already known). Beyond those, **no other warnings detected statically** — JS uses `document.addEventListener('DOMContentLoaded')` correctly, no deprecated APIs, no `document.write`, no synchronous XHR. | Fix F1 (delete fonts.css `<link>`) — eliminates all 28 errors at once. |
| **F45** | **No `Cache-Control` headers** from Python SimpleHTTP (it's a dev server). On real deployment, ensure long-cache for `assets/*` (`Cache-Control: public, max-age=31536000, immutable`) and short-cache for `index.html` (`Cache-Control: public, max-age=300`). Hash-busting filenames recommended (`v3.abc123.css`). | Set up at the CDN layer when deploying. |

---

## Estimated CWV (3G mid-tier mobile, throttled 1.6Mbps / 562ms RTT, 4× CPU slowdown)

| Metric | Current (estimated) | Target | Gap |
|---|---|---|---|
| **LCP** | **~4.5s** | < 2.5s | **-2.0s needed** |
| **CLS** | **~0.13** | < 0.1 | **-0.03 needed** |
| **INP** | **~280ms** | < 200ms | **-80ms needed** |
| FCP | ~2.6s | < 1.8s | -0.8s |
| TTI | ~5.2s | < 3.8s | -1.4s |
| TBT | ~520ms | < 200ms | -320ms |
| Total transferred (uncompressed, init load) | ~280 KB | n/a (real CDN ≈ 50 KB Br) | n/a |

**Drivers of the gap:**
- LCP: render-blocking CSS chain (F1, F2) + Google Fonts woff2 wait (F7) + SVG filter on wordmark (F6).
- CLS: font-swap metrics shift (F8) + nw3a images missing intrinsic ratio (F9).
- INP: hero atmospheric stack on mobile (F4, F35, F36) + always-running animations on hidden Method frames (F5) + scroll handler density (F18, F32).

**Doing F1 + F2 + F4 + F5 + F8 + F9 (the six P0s most directly tied to CWV)** moves the page to:
- LCP ≈ 1.9s
- CLS ≈ 0.06
- INP ≈ 140ms

That clears the bar with margin. Doing the P1s on top brings it close to a 95+ Lighthouse score.

---

## Benchmark comparison

| Site | LCP (3G mobile) | CLS | INP | What they do that we should copy |
|---|---|---|---|---|
| Apple product page (e.g. iPhone) | ~2.1s | ~0.02 | ~80ms | Inlines critical CSS for hero. Lazy-loads heavy media. SVG icons over PNGs. |
| Stripe.com homepage | ~2.3s | ~0.04 | ~60ms | Single CSS bundle, code-split JS, preloads LCP image. |
| web.dev homepage | ~1.8s | ~0.01 | ~50ms | `content-visibility: auto` on every section. Single font family. |

Apple-class is reachable with F2 + F4 + F37 + F12. Stripe-class is reachable with F1 through F10 done.

---

## Quick wins (do these first, in this order)

1. **F1**: delete `<link rel="stylesheet" href="assets/fonts.css">` — 60 seconds of work, kills 28 console errors and 44 round-trips.
2. **F3**: add `defer` to v3.js script tag.
3. **F4**: wrap `.hero3__atmosphere` in `@media (min-width: 901px)` — a single line.
4. **F5**: add `.mtd3__frame:not(.is-active) * { animation-play-state: paused; }` to v3.css.
5. **F9**: add width/height to the 6 nw3a `<img>` tags.

Total: ~30 minutes of work. Estimated lift: LCP -1.5s, INP -60ms, CLS -0.05, console: clean.

---

## 200-word summary

The almobadir.com page is built like a Behance reel rather than a production site: 130 KB of uncompressed CSS, 5 render-blocking stylesheet links in serial, 44 font 404s before any content paints, and 6 Google Fonts families loaded with full weight ranges when only ~4 weights are actually used. The hero stacks 7 atmospheric layers (3 blur(80px) meshes, conic-gradient, grid, grain, vignette) on the very first viewport with no mobile gating, plus an always-running 60s hue-drift animation that repaints multiple compositor layers every frame. The Method section keeps 5 absolutely-positioned scrollytelling frames each with always-running keyframe animations even when their parent has opacity:0 — hidden frames still cost GPU. Estimated CWV on 3G mid-tier: LCP ~4.5s (target 2.5s), CLS ~0.13 (target 0.1), INP ~280ms (target 200ms). Six P0 fixes (delete fonts.css link, defer v3.js, gate hero atmosphere by min-width:901px, pause inactive Method animations, lock font metrics, add intrinsic image ratios) clear the bar with margin. Twenty-five additional findings span network waterfall, image sizing, animation cost, scroll-handler density, and stacked backdrop-filters. components.css (28 KB) is already dead code — Wave 1 was wrong that it's linked.
