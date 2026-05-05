# Wave 1 Discovery — almobadir.com

**Audited:** 2026-04-26
**Live URL:** http://localhost:8765/index.html (local server)
**Source root:** `C:\Users\toner\OneDrive\Desktop\almobadir\website\`
**Page title:** المبادر — عندما يلتقي الترفيه والتعليم
**Locale:** `lang="ar"`, `dir="rtl"`
**HTML doc:** 1,220 lines / single page / 9 top-level sections inside `<body>`
**Total scrollable height:** 14,809 px @ 1440w · 16,887 px @ 768w · 16,983 px @ 390w

---

## Site Map

The page is a single `index.html` with 9 sections in DOM order. Below is the canonical map with element counts captured live in the browser at 1440×900.

| # | Section root | DOM tag | Heights @1440 / @768 / @390 (px) | Counts (button / a / input / svg / img / heading) |
|---|---|---|---|---|
| 1 | `.hdr3` `#hdr3-root` | `<header>` (sticky) | 105 / 62 / 62 | 6 / 28 / 1 / 7 / 0 / 1×h3 |
| 2 | `.hero3` | `<section>` | 1232 / 1490 / 1343 | 1 / 7 / 1 / 6 / 0 / 1×h1 + 1×h2 |
| 3 | `.manif3` `#manifesto` | `<section>` paper-slab | 879 / 651 / 530 | 0 / 0 / 0 / 2 / 0 / 0 (uses `.manif3__lede` p) |
| 4 | `.nw3a` `#network` | `<section>` | 2283 / 2485 / 3208 | 0 / 6 / 0 / 0 / 6 / 1×h2 + 6×h3 |
| 5 | `.mtd3` `#method` | `<section>` scroll-pinned | 5108 / 5064 / 3772 | 0 / 1 / 0 / 6 / 0 / 1×h2 + 5×h3 |
| 6 | `.cnt3` `#cnt3-latest` | `<section>` | 1143 / 1509 / 1855 | 0 / 4 / 0 / 1 / 3 / 1×h2 + 3×h3 |
| 7 | `.nl3-section` `#newsletter` | `<section>` | 1065 / 1654 / 1657 | 1 / 8 / 1 / 1 / 0 / 1×h2 + 1×h3 + 2×h4 |
| 8 | `.fnd3a` `#founder` | `<section>` | 1663 / 2241 / 2328 | 0 / 4 / 0 / 8 / 1 / 1×h2 + 1×h3 |
| 9 | `.ftr3` | `<footer>` | 1169 / 1635 / 2177 | 4 / 22 / 1 / 9 / 0 / 1×h2 + 1×h3 + 4×h4 |

**Composition note:** four CSS files load in this order: `assets/fonts.css` → `assets/tokens.css` → `assets/base.css` → `assets/v3.css`. `components.css` is **not loaded by index.html** but exists in `/assets/` — it is dead-code legacy / earlier draft, present in the repo but unreferenced (confirmed by grep on index.html). One JS: `assets/v3.js` (defer). No build step. Google-Fonts `<link>` preloads Almarai, Reem Kufi, Tajawal, Cairo, Instrument Serif, JetBrains Mono.

### Screenshot index (35 files, all under `design-audit/screenshots/`)

**Full-page (8):** `full-1920.png` `full-1440.png` `full-1280.png` `full-1024.png` `full-768.png` `full-414.png` `full-390.png` `full-375.png`

**Section captures @1440 (9):** `hdr3-1440.png` `hero3-1440.png` `manif3-1440.png` `nw3a-1440.png` `mtd3-1440.png` `cnt3-1440.png` `nl3-section-1440.png` `fnd3a-1440.png` `ftr3-1440.png`

**Section captures @768 (9):** `hdr3-768.png` `hero3-768.png` `manif3-768.png` `nw3a-768.png` `mtd3-768.png` `cnt3-768.png` `nl3-section-768.png` `fnd3a-768.png` `ftr3-768.png`

**Section captures @390 (9):** `hdr3-390.png` `hero3-390.png` `manif3-390.png` `nw3a-390.png` `mtd3-390.png` `cnt3-390.png` `nl3-section-390.png` `fnd3a-390.png` `ftr3-390.png`

---

## Inventory: Interactive Elements

Per-section breakdown of every clickable / focusable element. Format: `selector | source location | label | href`.

### 1. `.hdr3` — header (28 anchors / 6 buttons / 1 input — 35 interactives total)
**Announce strip:**
- `button.hdr3-announce-prev` | index.html:38 | "السابق" (prev) | —
- `button.hdr3-announce-next` | index.html:44 | "التالي" (next) | —
- 3 `span.hdr3-tick` containing inline links: `a.hdr3-tick-link` to `#newsletter` (line 40), `#network` (41), `#network` (42)

**Top nav (lines 65-74):**
- `button.hdr3-nav-link[aria-haspopup]` data-mega="network" | line 68 | "الشبكة" (network) — opens mega-menu
- `a.hdr3-nav-link[href="#method"]` | 70 | "المنهج"
- `a.hdr3-nav-link[href="#content"]` | 71 | "المحتوى"
- `a.hdr3-nav-link[href="#founder"]` | 72 | "المؤسس"
- `a.hdr3-nav-link.hdr3-nav-ext[href="https://almobadir.media"]` | 73 | "الوكالة" (external, target=_blank)

**Mega menu (lines 75-94):** 6 brand cards `a.hdr3-card` — each Instagram URL, target=_blank:
- @almobadir_mindset (verde, 478K) | line 85
- @almobadir (crimson, 363K) | 86
- @badrshaqer (crimson, 317K) | 87
- @almobadir_flousak (azure, 202K) | 88
- @almobadir_business (crimson, 124K) | 89
- @almobadir_media (teal, 57.7K) | 90
- `a.hdr3-mega-all[href="#network"]` | 82 | "عرض الكل"

**Utility bar (lines 97-111):**
- `input[type=search].hdr3-search-input` | line 99 | placeholder "ابحث في المبادر…"
- `kbd.hdr3-kbd` "⌘ K" (visual only)
- `button.hdr3-lang-btn[data-lang="ar"]` | 103 | "AR"
- `button.hdr3-lang-btn[data-lang="en"]` | 104 | "EN"
- `a.hdr3-cta[href="#newsletter"]` | 106 | "اشترك في النشرة"
- `button.hdr3-burger#hdr3-burger` | 111 | "القائمة" (menu toggle)

**Drawer (lines 115-135) — duplicates the nav for mobile:**
- 5 nav links + 6 brand-card links + 1 CTA = 12 anchors
- Anchors: #network, #method, #content, #founder, https://almobadir.media (lines 118-122)
- 6 IG handle links (lines 126-131)
- `a.hdr3-drawer-cta[href="#newsletter"]` | 133 | "اشترك في النشرة"

### 2. `.hero3` — hero (7 anchors / 1 button / 1 input)
- `form.hero3__signup` (line 188) — novalidate, JS-handled submit (v3.js:99-111)
- `input#hero3-email[type=email]` | 193 | placeholder "عنوان بريدك الإلكتروني"
- `button[type=submit].hero3__signup-cta` | 194 | "اشترك" (subscribe)
- 6 `a.hero3__chip` to Instagram (lines 260-265): mindset / flagship / badrshaqer / flousak / business / media — all target=_blank
- 1 `a.hero3__panel-link[href="#network"]` | 251 | "استعرض الشبكة كاملة"

### 3. `.manif3` — manifesto (no interactive elements)
- Static editorial slab. 0 buttons / 0 anchors. SVG underline + ornament only.

### 4. `.nw3a` — network grid (6 anchors)
- 6 `a.nw3a-link` per card — all to instagram.com, target=_blank rel=noopener (lines 372, 385, 398, 411, 424, 437)

### 5. `.mtd3` — method (1 anchor)
- `a.mtd3__cta[href="#content"]` | line 828 | "شاهد المنهج تطبيقاً" (outro CTA)
- 5 `<article class="mtd3__frame">` panels (data-step 1-5) with role="listitem" inside `<div role="list">`. No clickable controls inside frames; navigation is scroll-based.

### 6. `.cnt3` — content cards (4 anchors)
- 3 `a.cnt3-link` (lines 848, 865, 881) — featured + 2 standard cards, all to Instagram
- 1 `a.cnt3-cta[href="https://instagram.com/almobadir"]` | 898 | "شاهد كل المحتوى على إنستغرام" (target=_blank)

### 7. `.nl3-section` — newsletter (8 anchors / 1 button / 1 input)
- `form#nl3-form` | 927 | novalidate
- `input#nl3-email[type=email]` | 930 | placeholder "name@example.com" dir="ltr"
- `button[type=submit].nl3-submit` | 933 | "اشترك مجاناً" (free signup)
- 8 `a.nl3-issue` previous-issue chips in the marquee (lines 985-992) — 4 real + 4 aria-hidden duplicates for infinite scroll. All `href="#"` (placeholders).

### 8. `.fnd3a` — founder (4 anchors)
- `a.fnd3a__follow-card` × 4 (lines 1087, 1094, 1101, 1108): Instagram, Threads, Calendly (CTA variant), Favikon — all target=_blank rel=noopener

### 9. `.ftr3` — footer (22 anchors / 4 buttons / 1 input)
- `form.ftr3-cta__form` | 1134 | novalidate
- `input#ftr3-email[type=email]` | 1136
- `button[type=submit].ftr3-cta__btn` | 1137 | "اشتراك"
- `a.ftr3-cta__alt` to instagram.com/almobadir | 1139
- 6 IG `a` in `.ftr3-list--net` (1159-1164)
- 6 platform anchors in `.ftr3-list` (1170-1175): #network, #method, #content, #newsletter, #founder, https://almobadir.media
- 5 anchors in `.ftr3-list--plat` (1181-1185): Threads, YouTube ×2, TikTok, Favikon
- 2 contact anchors (1191, 1192): mailto:hello@almobadir.com, almobadir.media
- `a.ftr3-mailbtn` (1197) | "راسلنا مباشرة" → mailto
- `button.ftr3-lang__btn[data-lang="ar"]` | 1207 | "AR"
- `button.ftr3-lang__btn[data-lang="en"]` | 1209 | "EN"
- `button.ftr3-top#ftr3-top` | 1211 | "للأعلى" (scroll-to-top)
- 1 brand mark anchor `a.ftr3-mark[href="#"]` | 1145

**Site-wide grand totals (live DOM):** 80 anchors, 16 buttons, 5 inputs (3 email + 1 search), 3 `<form>`s.

---

## Inventory: Animations

### @keyframes (35 rules — all named, no duplicates by intent, but identical "spin to 360deg" appears under 5 different names)

| # | name | location | duration · iter | applied to |
|---|---|---|---|---|
| 1 | `pulse` | base.css:261 | 2.4s infinite | `.pulse` (live-dot pattern, also @ 266 reduced) |
| 2 | `meshSpin` | components.css:162 | 32s linear infinite | `.hero-mesh` (legacy `.hero` not used in DOM — components.css unloaded) |
| 3 | `rise` | components.css:248 | (consumed by .hero-wordmark, .hero-tagline etc; legacy) | unused — components.css not linked |
| 4 | `hero3-hue-drift` | v3.css:125 | 60s linear infinite | `.hero3` (drives `--hero3-hue` via @property) |
| 5 | `hero3-scroll-shift` | v3.css:126 (inside @supports) | linear (scroll-timeline) | `.hero3` when `animation-timeline: scroll()` supported |
| 6 | `hero3-rise` | v3.css:157 | 1000ms (`--d-glide`) ease-glide forwards | `[data-stagger]` 0..7 with delays 0/180/360/540/720/540/900/1080ms |
| 7 | `hero3-pulse` | v3.css:162 | 1.6s ease-glide infinite | `.hero3__pulse-ring` |
| 8 | `hero3-halo-spin` | v3.css:190 | 24s linear infinite | `.hero3__panel-halo` |
| 9 | `hero3-blink` | v3.css:197 | 1.6s ease-glide infinite | `.hero3__panel-live-dot` |
| 10 | `hero3-ticker` | v3.css:234 | 50s linear infinite | `.hero3__ticker-track` |
| 11 | `hero3-scroll-cue` | v3.css:239 | 2.4s ease-glide infinite | `.hero3__scroll-line::after` |
| 12 | `manif3-spin` | v3.css:271 | 28s linear infinite | `.manif3__ornament svg` |
| 13 | `nw3a-pulse` | v3.css:284 | 2.4s ease-in-out infinite | `.nw3a-eyebrow-dot` |
| 14 | `nw3a-arrow` | v3.css:350 | 0.7s ease-glide (one-shot on hover) | `.nw3a-card:hover .nw3a-cta span` |
| 15 | `mtd3-pulse` | v3.css:365 | 2.4s ease-glide infinite | `.mtd3__eyebrow-dot` |
| 16 | `mtd3-bounce` | v3.css:371 | 2s ease-glide infinite | `.mtd3__hint svg` |
| 17 | `mtd3-spin` | v3.css:423 | 28s/22s/18s linear infinite | `.mtd3-ring--1`, `.mtd3-ring--2` (reverse), `.mtd3-reel` |
| 18 | `mtd3-pulse-ring` | v3.css:424 | 2.4s ease-glide infinite | `.mtd3-ring--3` |
| 19 | `mtd3-pulse-dot` | v3.css:425 | 1.6s ease-glide infinite | `.mtd3-target` |
| 20 | `mtd3-flicker` | v3.css:426 | 3.2s steps(2,end) infinite | `.mtd3-ticks` |
| 21 | `mtd3-mess-fade` | v3.css:429 | 6s ease-glide alternate | `.mtd3-mess` (frame 02 chaos lines) |
| 22 | `mtd3-clean-fade` | v3.css:430 | 6s ease-glide alternate | `.mtd3-clean` (frame 02 clean overlay) |
| 23 | `mtd3-strip` | v3.css:433 | 3s linear infinite | `.mtd3-strip` (frame 03 film strip) |
| 24 | `mtd3-wave` | v3.css:439 | 3.6s ease-glide infinite, +0.4s/0.8s/1.2s/1.6s stagger on `:nth-child(2..5)` | `.mtd3-waves circle` |
| 25 | `mtd3-tilt` | v3.css:441 | 6s ease-glide alternate | `.mtd3-cover` (frame 04 magazine tilt) |
| 26 | `cnt3-pulse` | v3.css:550 | 2.4s ease-glide infinite | `.cnt3-dot` |
| 27 | `cnt3-shimmer` | v3.css:567 | 1.4s linear infinite | `.cnt3-skeleton` (image-load placeholder) |
| 28 | `nl3-pulse` | v3.css:616 | 2.4s ease-glide infinite | `.nl3-pulse` |
| 29 | `nl3-spin` | v3.css:643 | 0.6s linear infinite | `.nl3-submit.is-loading .nl3-submit-arrow` |
| 30 | `nl3-pulse-green` | v3.css:656 | 2s ease-glide infinite | `.nl3-live-dot` |
| 31 | `nl3-marquee` | v3.css:694 | 38s linear infinite (paused on hover) | `.nl3-issues-row` |
| 32 | `fnd3aRail` | v3.css:718 | 28s linear infinite | `.fnd3a__rail-text` (vertical scrolling rail) |
| 33 | `fnd3aSpin` | v3.css:725 | 24s linear infinite (+ reverse on `__verified-tick`) | `.fnd3a__verified` |
| 34 | `ftr3-pulse` | v3.css:858 | 2s ease-glide infinite | `.ftr3-clock__pulse` |

**Unreferenced keyframes (from unused components.css):** `meshSpin` (line 162), `rise` (248). Both targeted at `.hero-*` legacy selectors that do NOT appear in the current `index.html`.

**Reduced-motion blanket (v3.css:1138-1151):** all the above are killed via `animation: none !important; transition: none !important; transform: none !important;` for ~50 listed selectors, plus base.css:266 kills `.pulse`. `[data-stagger]` is also forced visible.

### Custom @property declarations

| # | name | syntax | initial | inherits | used by |
|---|---|---|---|---|---|
| 1 | `--hero3-hue` | `<angle>` | 352deg | yes | hero3 atmosphere mesh + halo |
| 2 | `--hero3-shift` | `<length>` | 0px | yes | hero3 scroll-shift on @supports scroll-timeline |
| 3 | `--hero3-glow` | `<percentage>` | 60% | yes | hero3 hue-drift |
| 4 | `--mtd3-progress` | `<number>` | 0 | yes | unused — declared but not referenced (`mtd3` JS sets `--mtd3-sub` instead) |
| 5 | `--mtd3-active` | `<integer>` | 1 | yes | unused — declared but not referenced |
| 6 | `--mtd3-sub` | `<number>` | 0 | yes | drives sub-progress per active scroll-pinned frame |
| 7 | `--hue` | `<angle>` | 0deg | no | components.css:163 — legacy, no DOM consumer |

### Transitions (96 distinct `transition:` declarations in v3.css)

Sample of significant ones (full count via grep, all use the canonical `--ease-glide` Bézier `cubic-bezier(0.28, 0.11, 0.32, 1)` or `--ease-out`):

- `.hdr3` — `background 0.32s var(--ease-glide), border-color 0.32s var(--ease-glide), backdrop-filter 0.32s var(--ease-glide)` (v3.css:18)
- `.hdr3-mega` — `opacity 0.32s, transform 0.32s, visibility 0s linear 0.32s` (line 53)
- `.hdr3-search` — `border-color 0.24s, background 0.24s, box-shadow 0.24s, width 0.32s` (line 75)
- `.hdr3-search:focus-within` — width grows from 220→260px (line 76)
- `.hero3__signup-row` — `border-color var(--d-base), box-shadow var(--d-base)` (line 177)
- `.manif3__word` — `opacity .55s, transform .55s` (line 257)
- `.manif3__underline path` — `stroke-dashoffset 1.4s var(--ease-glide) .25s` (line 264)
- `.nw3a-card` — `transform .65s, border-color .4s, box-shadow .5s` (line 294)
- `.nw3a-cover img` — `transform 1.2s, filter .6s` (line 320)
- `.mtd3__frame` — `opacity 1s, transform 1s` (mobile @ line 389) / `opacity .7s, transform .8s, visibility 0s linear .7s` (desktop @ line 458)
- `.mtd3__frame.is-active .mtd3__art` — `opacity .5s, transform .14s linear` (sub-progress driven, 478)
- `.cnt3-img` — `opacity 0.6s, transform 1.2s` (line 568)
- `.nl3-form` — `border-color 320ms, box-shadow 320ms` (624)
- `.nl3-submit` — `transform 280ms, background 280ms, min-width 480ms` (line 632)
- `.fnd3a__photo` — `transform 1.6s` (720)

### JS scroll handlers / IntersectionObservers (v3.js)

| # | type | section | line(s) |
|---|---|---|---|
| 1 | `scroll` listener | hdr3 (set `is-scrolled` after y>12) | 35 |
| 2 | `IntersectionObserver` `threshold:.15` | manif3 root | 120 |
| 3 | `scroll` + RAF (manif3 word-by-word reveal) | manif3 | 137 |
| 4 | `IntersectionObserver` `threshold:0.18, rootMargin:'0px 0px -8% 0px'` | nw3a cards | 148 |
| 5 | `pointermove` RAF tilt | nw3a cards (3D tilt MAX=6°) | 159-171 |
| 6 | `scroll` + RAF (mtd3 desktop scroll-pinned scrollytelling) | mtd3 | 245-251 |
| 7 | `IntersectionObserver` thresholds `[0.3,0.5,0.7]` rootMargin `-20% 0px -30% 0px` | mtd3 mobile | 256-266 |
| 8 | `IntersectionObserver` threshold:0.15 rootMargin `0px 0px -40px 0px` | cnt3 cards | 297 |
| 9 | `scroll` + RAF (cnt3 featured progress bar) | cnt3 | 317 |
| 10 | `mousemove` (cnt3 CTA radial follow) | cnt3 | 323 |
| 11 | `mousemove` RAF tilt (perspective `--data-tilt`) | nl3 preview | 364 |
| 12 | `setInterval(60000)` countdown to next Riyadh Sunday 09:00 | nl3 | 393 |
| 13 | `IntersectionObserver` threshold:0.18 | fnd3a (counter trigger) | 401 |
| 14 | `requestAnimationFrame` tick (counter ease-out cubic, 1400-1800ms) | fnd3a counters | 414-422 |
| 15 | `mousemove` photo parallax | fnd3a portrait | 433 |
| 16 | `requestAnimationFrame` 1s loop (Riyadh clock) | ftr3 | 451-454 |
| 17 | `IntersectionObserver` threshold:0.18 rootMargin `0px 0px -40px 0px` | ftr3 columns stagger | 459 |
| 18 | header announce ticker `setInterval(6000)` | hdr3 | 22 |
| 19 | mega-menu hover `setTimeout(140)` open/close | hdr3 | 41 |

**Reduced-motion guard:** `reduceMotion = matchMedia('(prefers-reduced-motion: reduce)').matches` (v3.js:8) is consulted by hero3, manif3, nw3a, mtd3, nl3, fnd3a — each short-circuits its motion paths. cnt3 and ftr3 partially honor it via the CSS @media block but JS still runs.

---

## Inventory: Design Tokens

### Color tokens (tokens.css:8-44)

**Surfaces (dark):**
- `--void: #0A0F1E` — primary dark bg
- `--void-2: #0E1525` — card bg on dark
- `--void-3: #0A1628` — deeper variant

**Surfaces (light / paper):**
- `--paper: #F8F4EA`
- `--paper-2: #FDFBF4`
- `--paper-3: #F0E8D2`

**Ink (text on dark):**
- `--ink: #FBF7EE` — primary
- `--ink-2: #F5EAD2` — body
- `--ink-soft: rgba(245, 234, 210, 0.78)`
- `--ink-mute: rgba(245, 234, 210, 0.52)`
- `--ink-quiet: rgba(245, 234, 210, 0.32)`

**Ink (on paper):**
- `--on-paper: #1A0F08` — primary
- `--on-paper-2: #4A3829` — secondary
- `--on-paper-mute: rgba(26, 15, 8, 0.55)`

**Brand:**
- `--crimson: #E6213B` (primary brand red)
- `--crimson-2: #C41A30`
- `--crimson-3: #6E0F1F`
- `--crimson-glow: rgba(230, 33, 59, 0.24)`
- `--crimson-glow-2: rgba(230, 33, 59, 0.55)`

**Sub-brand accents:**
- `--verde: #1FA37A` (mindset)
- `--azure: #1E5AA8` (flousak)
- `--teal: #2AD4B8` (media)
- `--gold: #E8B855`

**Hairlines:**
- `--hairline: rgba(245, 234, 210, 0.08)`
- `--hairline-strong: rgba(245, 234, 210, 0.18)`
- `--hairline-paper: rgba(26, 15, 8, 0.10)`
- `--hairline-paper-strong: rgba(26, 15, 8, 0.22)`

**Surface aliases:** `--bg`, `--bg-2`, `--fg`, `--fg-2`, `--fg-soft`, `--fg-mute`, `--line`, `--line-strong` — all forwarded into `:root` then re-bound on `.slab-light` / `.slab-dark` (tokens.css:163-200).

### Spacing tokens (4px base)
`--s-1: 4px` · `--s-2: 8px` · `--s-3: 12px` · `--s-4: 16px` · `--s-5: 20px` · `--s-6: 24px` · `--s-7: 32px` · `--s-8: 40px` · `--s-9: 48px` · `--s-10: 56px` · `--s-12: 72px` · `--s-14: 96px` · `--s-16: 120px` · `--s-20: 160px` · `--s-24: 200px`

### Containers / radii
- `--container: 1320px` · `--container-tight: 980px` · `--container-narrow: 720px`
- `--gutter: clamp(20px, 3vw, 48px)`
- `--r-xs: 2px` · `--r-sm: 4px` · `--r-md: 8px` · `--r-lg: 16px` · `--r-xl: 20px` · `--r-2xl: 28px` · `--r-pill: 999px`

### Elevation
`--e-1`, `--e-2`, `--e-3`, `--e-4`, `--e-glow-crimson`, `--e-glow-verde`, `--e-glow-azure`, `--e-glow-teal` (tokens.css:134-141)

### Easing / duration
- `--ease-glide: cubic-bezier(0.28, 0.11, 0.32, 1)` (Apple-style)
- `--ease-counter: cubic-bezier(0.4, 0, 0.2, 1)`
- `--ease-snap: cubic-bezier(0.34, 1.56, 0.64, 1)`
- `--ease-out`, `--ease-in`
- `--d-fast: 150ms` · `--d-base: 320ms` · `--d-slow: 700ms` · `--d-glide: 1000ms`

### Z-index
`--z-grain: 2` · `--z-content: 5` · `--z-header: 50` · `--z-modal: 100` · `--z-toast: 200`

### Off-token / literal values (flagged)

These are colors / dimensions in the styles that bypass the token system. Most are intentional gradient stops (where `var(--…)` can't be interpolated cleanly), but they're still tokenization gaps to know about.

| File:Line | Selector | Literal | Notes |
|---|---|---|---|
| v3.css:170 | `.hero3__tagline-accent` | `#FF324A`, `#C41A30` | crimson gradient — `#FF324A` is **not** a defined token (lighter than `--crimson`) |
| v3.css:171 | `.hero3__tagline-accent--alt` | `#2AD4B8`, `#1FA37A` | duplicates `--teal` and `--verde` literally |
| v3.css:182 | `.hero3__signup-cta` | `#E6213B`, `#C41A30` | duplicates `--crimson` / `--crimson-2` literally |
| v3.css:245 | `.manif3` local-vars | `#FDFBF4` | duplicates `--paper-2` |
| v3.css:380 | `.mtd3__progress-fill` | `#E8B855` | duplicates `--gold` (gradient stop) |
| v3.css:397-402 | `.mtd3__frame[data-color]` | `#E6213B`, `#3CC58B`, `#E8B855`, `#5AA8FF`, `#1FB6A6` | the **method-frame palette is not in tokens.css** — `#3CC58B` (verde variant), `#5AA8FF` (azure variant), `#1FB6A6` (teal variant), `#E8B855` (gold) are Method-only off-token shades. |
| v3.css:639 | `.nl3-submit.is-success` | `#1a8c4a` | success-green not tokenized |
| v3.css:655 | `.nl3-live-dot` | `#34d27a` | live-green not tokenized; rgba(52,210,122) used |
| v3.css:820 | `.ftr3-bg` | `#000` | mask gradient endpoints |
| v3.js:184 | `colors` array | `'#E6213B','#3CC58B','#E8B855','#5AA8FF','#1FB6A6'` | hardcoded duplicate of mtd3 palette |
| v3.css:339 | `nw3a-tag-en border-right` | `1px solid var(--hairline)` | OK |
| index.html:51-61 | inline SVG strokes/fills | `#0A0F1E`, `currentColor` | header logo verify-tick uses void literal |
| index.html:167 | `<linearGradient id="hero3-crimson-grad">` | `#FF324A`, `#E6213B`, `#9C1428` | hero SVG gradient — `#9C1428` is **another** off-token crimson shade |
| index.html:1149 | `<circle fill="#E6213B">` | `#E6213B` | footer wordmark accent dot — duplicates `--crimson` |
| components.css (unloaded) | many | `#1a2138`, `#0a1628` | dead code in unused stylesheet |

The Method palette (frames 1-5) is the largest tokenization gap: 5 off-token hex codes hardcoded twice (CSS attribute selectors + JS color array) and not surfaced in tokens.css.

---

## Inventory: Typography

### @font-face declarations (fonts.css)

| family | weight | format priority | line |
|---|---|---|---|
| `Mostaqbali` | 400 | woff2 → woff → otf → ttf | fonts.css:17 |
| `Mostaqbali` | 500 | …same chain | 27 |
| `Mostaqbali` | 700 | …same chain | 37 |
| `Mostaqbali` | 900 | …same chain | 47 |
| `Graphik Arabic` | 400 | …same chain | 59 |
| `Graphik Arabic` | 500 | …same chain | 69 |
| `Graphik Arabic` | 600 | …same chain | 79 |
| `Graphik Arabic` | 700 | …same chain | 89 |
| `Madani Arabic` | 400 | …same chain | 101 |
| `Madani Arabic` | 500 | …same chain | 111 |
| `Madani Arabic` | 700 | …same chain | 121 |

11 declarations. **CRITICAL: every src URL 404s** — `assets/fonts/` directory exists but contains only `README.md`. Console errors per breakpoint = 28 (7 × 4 formats for Mostaqbali+Graphik; Madani not preloaded so its 404s don't hit until needed).

### Font-family stacks (tokens.css:52-57)
- `--font-ar-display: 'Mostaqbali', 'Almarai', 'Reem Kufi', 'IBM Plex Sans Arabic', system-ui, sans-serif`
- `--font-ar-body: 'Graphik Arabic', 'Graphik', 'Tajawal', 'Almarai', 'IBM Plex Sans Arabic', -apple-system, BlinkMacSystemFont, system-ui, sans-serif`
- `--font-ar-alt: 'Madani Arabic', 'Cairo', 'Almarai', 'Tajawal', system-ui, sans-serif`
- `--font-en: 'Graphik', 'Geist', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', sans-serif`
- `--font-mono: 'JetBrains Mono', ui-monospace, 'SF Mono', 'Cascadia Code', monospace`
- `--font-serif: 'Instrument Serif', 'Times New Roman', Georgia, serif`

**Resolved fonts in browser** (because Mostaqbali/Graphik 404, the second-tier Google-Fonts faces win): Almarai for display, Tajawal for body, Cairo for alt, Instrument Serif for serif, JetBrains Mono for mono. The display rendering you see in screenshots is **Almarai**, not Mostaqbali.

### Type scale (tokens.css:60-70, all fluid clamp())

Sorted smallest → largest (resolved at 1440px viewport in parens):

| token | value | min · ideal · max | resolved @1440 |
|---|---|---|---|
| `--t-eyebrow` | 11px | static | 11px |
| `--t-caption` | 12.5px | static | 12.5px |
| `--t-small` | 14.5px | static | 14.5px |
| `--t-body` | 17px | static | 17px |
| `--t-lead` | clamp(18px, 1.6vw, 24px) | 18 / 23.04 / 24 | 23.04px |
| `--t-h3` | clamp(22px, 2.4vw, 32px) | 22 / 34.56 / 32 | 32px |
| `--t-h2` | clamp(30px, 4.2vw, 60px) | 30 / 60.48 / 60 | 60px |
| `--t-h1` | clamp(36px, 5vw, 72px) | 36 / 72 / 72 | 72px |
| `--t-display` | clamp(40px, 6.5vw, 96px) | 40 / 93.6 / 96 | 93.6px |
| `--t-stat` | clamp(40px, 5vw, 64px) | 40 / 72 / 64 | 64px (clamped) |
| `--t-hero` | clamp(64px, 11vw, 168px) | 64 / 158.4 / 168 | 158.4px |

**Off-scale font-size literals** found in v3.css (in addition to tokens, dozens of them — sample below):
- 9px, 9.5px, 10px, 10.5px, 11px, 11.5px, 12px, 12.5px, 13px, 13.5px, 14px, 14.5px, 15px, 15.5px, 16px, 17px, 18px, 19px, 20px, 21px, 22px, 24px, 26px, 28px, 30px, 32px, 36px, 38px, 40px, 44px, 56px, 92px
- Component-specific clamps: `clamp(13px,0.9vw,14px)`, `clamp(15px,1.1vw,17px)`, `clamp(15px,1.2vw,18px)`, `clamp(15px,1.3vw,18px)`, `clamp(16px,1.4vw,19px)`, `clamp(16px,1.5vw,20px)`, `clamp(17px,1.4vw,20px)`, `clamp(17px,1.4vw,21px)`, `clamp(18px,1.5vw,22px)`, `clamp(18px,1.6vw,24px)`, `clamp(18px,1.7vw,26px)`, `clamp(18px,1.8vw,28px)`, `clamp(20px,1.8vw,26px)`, `clamp(22px,1.8vw,28px)`, `clamp(22px,2.2vw,32px)`, `clamp(22px,2.4vw,32px)`, `clamp(22px,2.8vw,38px)`, `clamp(24px,2.6vw,36px)`, `clamp(26px,3.4vw,44px)`, `clamp(28px,3.2vw,44px)`, `clamp(28px,3.4vw,44px)`, `clamp(28px,3.4vw,48px)`, `clamp(28px,4.2vw,68px)`, `clamp(30px,4.2vw,60px)`, `clamp(32px,3.6vw,48px)`, `clamp(32px,4.4vw,60px)`, `clamp(32px,4.4vw,64px)`, `clamp(32px,4.6vw,68px)`, `clamp(36px,4.4vw,56px)`, `clamp(36px,9vw,56px)`, `clamp(40px,5vw,76px)`, `clamp(60px,7vw,92px)`, `clamp(140px,16vw,220px)`, `clamp(180px,22vw,320px)`. Each section reinvents its own scale rather than reusing `--t-h*`.

### Line-heights (sorted)
- `0` (logo SVG containers, `.ftr3-mark`), `0.8`, `0.84`, `0.85`, `0.86` (`--lh-hero`), `0.9`, `0.92`, `0.95` (`--lh-display`), `1.0` (`--lh-h1`), `1.04` (`--lh-h2`), `1.08`, `1.1`, `1.12`, `1.14`, `1.15`, `1.18` (`--lh-h3`), `1.2`, `1.22`, `1.24`, `1.25`, `1.3`, `1.32`, `1.35`, `1.4`, `1.45` (`--lh-lead`), `1.5`, `1.55`, `1.6`, `1.62`, `1.65` (`--lh-body`), `1.7`, `1.85`. Tokens cover only 8 of these; the rest are component-specific values typed inline.

### Letter-spacing tokens
`--ls-hero: -0.05em`, `--ls-display: -0.038em`, `--ls-h2: -0.028em`, `--ls-h3: -0.018em`, `--ls-lead: -0.005em`, `--ls-body: 0`, `--ls-eyebrow: 0.22em`, `--ls-mono: 0.04em`. Several literal letter-spacings appear inline (`-.04em` on mtd3 num, `0.06em`–`0.32em` on misc mono labels).

### Weight tokens
`--w-light:300, --w-regular:400, --w-medium:500, --w-semibold:600, --w-bold:700, --w-heavy:800, --w-black:900`. v3.css frequently uses bare `700`, `800`, `900` instead of the token.

---

## Inventory: Breakpoint Rules

37 `@media` rules total. Inflection points across all CSS files:

| breakpoint | files | what changes |
|---|---|---|
| `(prefers-reduced-motion: reduce)` | base.css:61, base.css:266, components.css:164, v3.css:1138 | kills animation/transition/transform globally; pulse off; data-stagger forced visible; sweeping kill of 50+ specific selectors |
| `min-width: 1100px` | v3.css:305, v3.css:315 | `.nw3a-grid` becomes 6-col; `.nw3a-card--wide` switches to inline image+body grid |
| `min-width: 901px` | v3.css:453 | mtd3 desktop scroll-pinned scrollytelling activates (stage = 5×100vh, frames absolute-positioned, slide animation via `--mtd3-sub`) |
| `min-width: 961px` | v3.css:660 | nl3 preview becomes sticky `top: 96px` |
| `max-width: 1180px` | v3.css:904 | ftr3 map collapses to 3 columns + brand spans full width |
| `max-width: 1080px` | v3.css:601 | cnt3 grid → 2 columns, featured spans both |
| `max-width: 1024px` | components.css:65, components.css:593, components.css:900, v3.css:115 | mobile nav drawer kicks in (legacy components.css); `.content-grid` to 1 col; `.footer-grid` to 2 cols; `.hdr3-search` shrinks to 180px and hides ⌘K |
| `max-width: 980px` | v3.css:240 | hero3 collapses to single column (panel below main); nav hides |
| `max-width: 960px` | v3.css:700, v3.css:813 | nl3 single-column with preview reordered below pitch; fnd3a switches to 1-col + 2-col follow grid |
| `max-width: 900px` | v3.css:527 | mtd3 falls back to mobile stacked layout (frames are normal block flow with intersection-observer, not sticky) |
| `max-width: 880px` | components.css:342, components.css:788, v3.css:116 | hdr3: nav/search/lang hidden, burger appears, CTA text hidden; legacy kpi 4→2; legacy founder 1col |
| `max-width: 780px` | v3.css:274 | manif3 head goes column; shadow shrinks; meta attribute hides |
| `max-width: 768px` | base.css:113, components.css:220, components.css:550, components.css:698 | section padding reduces; legacy hero-wordmark-svg shrinks; legacy method becomes 1col; legacy newsletter padding |
| `max-width: 760px` | v3.css:306, v3.css:905 | nw3a single column; ftr3 → 2 cols + base inner stacks |
| `max-width: 720px` | v3.css:910 | the mega comprehensive mobile pass (~150 rules across all sections) |
| `max-width: 680px` | v3.css:602 | cnt3 single-column |
| `max-width: 640px` | v3.css:117 | hdr3 announce-arrows hide, tighter padding |
| `max-width: 600px` | components.css:868, components.css:901, v3.css:241 | hero3 kpi-grid 1col + signup wraps + eyebrow shrinks; legacy founder-meta single col; legacy footer 1 col |
| `max-width: 560px` | v3.css:701 | nl3 form 1col + submit full-width |
| `max-width: 480px` | components.css:343, v3.css:906, v3.css:1064 | tight-mobile pass (~70 rules); legacy kpi 1col; ftr3 base wraps tags |

**System summary:** the v3-era files (v3.css) and the legacy components.css use overlapping but slightly different breakpoint scales. v3.css inflections are 480 / 560 / 600 / 640 / 680 / 720 / 760 / 780 / 880 / 900 / 901 / 960 / 961 / 980 / 1024 / 1080 / 1100 / 1180. There is no canonical breakpoint tokenization — each section invents its own thresholds. The 720px and 480px passes (v3.css:910 and 1064) are the dominant mobile rewrites.

---

## Console Audit

### Error inventory (28 errors at every breakpoint, 0 warnings, 0 JS errors)

All 28 errors are `Failed to load resource: 404` for fonts under `http://localhost:8765/assets/fonts/`:

```
Mostaqbali-Regular.woff2 / .woff / .otf / .ttf
Mostaqbali-Medium.woff2  / .woff / .otf / .ttf
Mostaqbali-Bold.woff2    / .woff / .otf / .ttf
Mostaqbali-Black.woff2   / .woff / .otf / .ttf
GraphikArabic-Regular.woff2 / .woff / .otf / .ttf
GraphikArabic-Medium.woff2  / .woff / .otf / .ttf
GraphikArabic-Semibold.woff2 / .woff / .otf / .ttf
GraphikArabic-Bold.woff2     / .woff / .otf / .ttf
```

**Cause:** `assets/fonts/` directory contains only `README.md`. Browsers fall through `@font-face` src chains then to the Google-Fonts fallback stack (Almarai, Tajawal, Cairo, Reem Kufi, Instrument Serif, JetBrains Mono).

**MadaniArabic-*.woff2 etc.** would also 404 if a `.fnd3a__follow-card` or other Madani consumer were rendered — but Madani is currently unused in DOM (its `--font-ar-alt` is declared but no selector references it in v3.css), so its loads are deferred and don't appear in the console at any breakpoint.

**Confirmed identical at all three anchor breakpoints (1440, 768, 390):** same 28 errors, no JS errors, no warnings. The errors fire once on initial parse — they don't recur on resize.

---

## Notable observations for downstream agents

1. **Font fidelity gap is the #1 visual hit.** Every "billion-dollar website" target (Stripe / Linear / Apple) ships its custom display font. Almobadir is currently rendering Almarai instead of Mostaqbali — the wordmark, hero tagline, manifesto, and section H2s are all "wrong" font. Wave 2+ agents must work assuming the brand fonts are not loaded (or get the font files placed first).

2. **Two stylesheet generations exist in the repo.** `components.css` (957 lines) is older `.hero` / `.channel` / `.method-item` / `.post` / `.newsletter` / `.founder-photo` / `.footer` etc. — and is **not loaded** by index.html. Auditing agents should ignore it unless explicitly cleaning dead code. v3.css (1151 lines) is the live stylesheet.

3. **The Method section (`.mtd3`) is the most complex piece.** 5108 px tall on desktop; the stage is 5×100vh sticky-pin scrolling driven by JS-set `--mtd3-sub` 0..1; sub-progress drives a per-frame slide-in plus staggered text reveal at thresholds 0.32 / 0.42 / 0.56 / 0.70. Each frame has a hand-illustrated SVG (crosshair, mess→clean lines, film reel + strip, magazine cover, broadcast tower with radio-wave rings). This is the section most likely to look broken in screenshots without scroll context.

4. **The Method palette is unsynchronized between CSS and JS.** `v3.css:398-402` and `v3.js:184` both hardcode the same 5 hex colors (`#E6213B #3CC58B #E8B855 #5AA8FF #1FB6A6`). None are defined in tokens.css. Any palette refactor needs to touch both files.

5. **The ticker / marquee animations are not paused on focus.** `.hero3__ticker-track` (50s linear infinite) and `.nl3-issues-row` (38s, hover-pause only) keep running for keyboard-focus users — accessibility concern.

6. **Search input has no behavior.** `input.hdr3-search-input` (index.html:99) accepts text; only ⌘K/Ctrl+K focuses it (v3.js:64). Submit / suggestions / results have no implementation.

7. **The newsletter mock and previous-issue chips are placeholder.** All `<a class="nl3-issue" href="#">` (index.html:985-992) are `#` links — not navigable. The 4 hidden-aria duplicates exist solely to seed the marquee loop.

8. **Founder counters animate to numbers in `data-target`.** `#6` (Favikon rank), `+325K` (followers), `+5,000` (content). The "+" prefix is rendered separately as `.fnd3a__stat-plus` so the digit-only count doesn't include it. data-format="comma" on 5,000 only.

9. **Headers have two languages of CTA + language toggle.** `.hdr3-lang-btn[data-lang]` (line 103, 104) only adds is-active to the clicked one — does not actually swap content. Same for `.ftr3-lang__btn` (lines 1207, 1209) which dispatches a `ftr3:lang` CustomEvent that nothing listens for.

10. **Reduced-motion CSS is comprehensive but JS is incomplete.** Several JS modules (cnt3 progress bar, ftr3 column observer) still run their work even when `reduceMotion` is true; the CSS @media block then erases the visual effect. Performance is unchanged but the JS overhead remains.
