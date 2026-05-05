# WOW · Header (`.hdr3`) — section moves

**Section anatomy:** sticky top bar (`.hdr3`) → announce strip with 3-phase ticker (`.hdr3-announce`) → main row with brand mark + 5-item nav (الشبكة has mega) + search (already mocks ⌘K) + AR/EN toggle + crimson CTA + hamburger; mobile drawer ≤880px.
**Anchor file:** `website/index.html:40-143` · **Styles:** `website/assets/v3.css:54-163` · **Behavior:** `website/assets/v3.js:10-70`.
**Live:** https://zaka33333-hash.github.io/almobadir-website/

---

## Executive summary

The header today is competently put together (Apple-blur sticky, gradient ticker, mega panel, ⌘ K hint already rendered as a `kbd`) but **none of those promises are kept**: the kbd doesn't flash on press, the search input doesn't expand into a real palette, the underline only fades, the ticker rotates without editorial labeling, the EN/AR toggle is decorative cosmetics rather than a culturally-pinned masthead pin, the mega panel cards reveal in unison instead of staggering, and the logomark is a static SVG that never *introduces itself* on first load. Every one of those is a Wave-1 reference away from being a Linear/Browser-Company/Stripe-tier moment. The 8 moves below take that already-built scaffolding and make it **earn its blur** — keyboard layer that actually responds, an Arabic-first masthead pin (Hayy Jameel pattern) that reframes EN/AR as a positioning statement, a logomark that path-draws once on first visit, a clip-path nav underline that *draws*, a Linear stagger on the mega menu, an Alt-grid X-ray for designer-curious visitors, and a one-pixel scroll-shrink choreography so the header *condenses* instead of just toggling a class. Total effort estimate: ~14 hours including QA. Top 3 are explicitly chosen so a single half-day delivers the screenshot-worthy moments (palette + masthead pin + logomark draw) without touching the announce strip / mega menu, which can ship in a second pass.

---

## MOVE 1: Real ⌘K palette behind the rendered `kbd`

**The moment**: User sees the `⌘ K` chip, hits the keys, and a centered command palette cross-fades in with backdrop blur, fuzzy-search prefocused, results staggering in — same gesture they get on Linear. The kbd chip itself flashes when the keys are pressed so the user *sees* the page acknowledge them.

**Where**: `.hdr3-search` and `.hdr3-kbd` @ index.html:103-107 · keydown handler @ v3.js:63-69 (already exists but only focuses input).

**Implementation**:
```css
/* keep the existing chip but make it flash on real key press */
.hdr3-kbd { transition: transform .12s var(--ease-out), background .12s var(--ease-out), color .12s var(--ease-out); }
.hdr3-kbd.is-pressed { transform: scale(.88); background: var(--crimson); color: var(--ink); border-color: var(--crimson); }

/* command palette */
.hdr3-cmdk-root { position: fixed; inset: 0; z-index: var(--z-modal); display: grid; place-items: start center; padding-top: 14vh; opacity: 0; visibility: hidden; transition: opacity .18s var(--ease-out), visibility 0s linear .18s; }
.hdr3-cmdk-root[data-open="true"] { opacity: 1; visibility: visible; transition: opacity .22s var(--ease-out); }
.hdr3-cmdk-scrim { position: absolute; inset: 0; background: rgba(10,15,30,.72); backdrop-filter: blur(14px) saturate(160%); -webkit-backdrop-filter: blur(14px) saturate(160%); }
.hdr3-cmdk-panel { position: relative; width: min(640px, 92vw); background: rgba(14,21,37,.96); border: 1px solid var(--hairline-strong); border-radius: var(--r-xl); box-shadow: var(--e-4); overflow: hidden; transform: translateY(8px) scale(.985); transition: transform .22s var(--ease-glide); }
.hdr3-cmdk-root[data-open="true"] .hdr3-cmdk-panel { transform: translateY(0) scale(1); }
.hdr3-cmdk-input { width: 100%; background: none; border: 0; outline: 0; color: var(--ink); font-family: var(--font-ar-body); font-size: 16px; padding: 18px 22px; border-bottom: 1px solid var(--hairline); }
.hdr3-cmdk-input::placeholder { color: var(--ink-mute); }
.hdr3-cmdk-list { max-height: 52vh; overflow-y: auto; padding: 8px; }
.hdr3-cmdk-row { display: flex; align-items: center; gap: 10px; padding: 11px 14px; border-radius: var(--r-md); color: var(--ink-soft); font-size: 14px; opacity: 0; transform: translateY(6px); animation: hdr3CmdkIn .32s var(--ease-out) forwards; animation-delay: calc(var(--i, 0) * 28ms); cursor: pointer; }
.hdr3-cmdk-row[aria-selected="true"], .hdr3-cmdk-row:hover { background: rgba(230,33,59,.08); color: var(--ink); }
.hdr3-cmdk-row-tag { margin-inline-start: auto; font-family: var(--font-mono); font-size: 11px; color: var(--ink-mute); }
@keyframes hdr3CmdkIn { to { opacity: 1; transform: none; } }

/* breadcrumb back-stack rail */
.hdr3-cmdk-stack { display: flex; gap: 6px; padding: 8px 14px 0; font-family: var(--font-mono); font-size: 11px; color: var(--ink-mute); }
.hdr3-cmdk-stack span + span::before { content: '/'; margin-inline-end: 6px; opacity: .5; }

@media (prefers-reduced-motion: reduce) {
  .hdr3-cmdk-root, .hdr3-cmdk-panel { transition: none; }
  .hdr3-cmdk-row { animation: none; opacity: 1; transform: none; }
}
```

```js
/* extends v3.js:63-69 — replace the focus-only handler */
const root = document.getElementById('hdr3-root');
const kbdChip = root.querySelector('.hdr3-kbd');

const palette = (() => {
  const ACTIONS = [
    { id:'sub', label:'الاشتراك في النشرة الأسبوعية', tag:'⏎', go: () => location.hash = '#newsletter' },
    { id:'net', label:'الشبكة · 7 حسابات إنستغرام',   tag:'N', go: () => location.hash = '#network' },
    { id:'mth', label:'منهج المبادر التحريري',         tag:'M', go: () => location.hash = '#method' },
    { id:'fnd', label:'بدر شاكر · المؤسس',              tag:'F', go: () => location.hash = '#founder' },
    { id:'cnt', label:'أحدث محتوى',                     tag:'C', go: () => location.hash = '#content' },
    { id:'ag',  label:'الوكالة · almobadir.media',      tag:'↗', go: () => window.open('https://almobadir.media', '_blank') },
    { id:'lng', label:'تبديل اللغة · EN ⇄ AR',          tag:'⇄', go: () => root.querySelector('.hdr3-lang-btn:not(.is-active)')?.click() },
    { id:'cpy', label:'نسخ بريد التواصل',               tag:'@', go: () => navigator.clipboard?.writeText('hello@almobadir.com') },
  ];
  const dom = document.createElement('div');
  dom.className = 'hdr3-cmdk-root';
  dom.dataset.open = 'false';
  dom.innerHTML = `<div class="hdr3-cmdk-scrim" data-close></div>
    <div class="hdr3-cmdk-panel" role="dialog" aria-label="Command palette" aria-modal="true">
      <div class="hdr3-cmdk-stack" aria-hidden="true"><span>المبادر</span></div>
      <input class="hdr3-cmdk-input" placeholder="ابحث أو اقفز إلى…" aria-label="بحث">
      <ul class="hdr3-cmdk-list" role="listbox"></ul>
    </div>`;
  document.body.appendChild(dom);
  const input = dom.querySelector('.hdr3-cmdk-input');
  const list  = dom.querySelector('.hdr3-cmdk-list');
  const stack = dom.querySelector('.hdr3-cmdk-stack');
  let idx = 0, results = ACTIONS.slice();

  const render = () => {
    list.innerHTML = results.map((a, i) =>
      `<li class="hdr3-cmdk-row" role="option" data-id="${a.id}" style="--i:${i}" aria-selected="${i===idx}">
         <span>${a.label}</span><span class="hdr3-cmdk-row-tag">${a.tag}</span></li>`).join('');
  };
  const filter = q => {
    const t = q.trim().toLowerCase();
    results = !t ? ACTIONS.slice() : ACTIONS.filter(a => a.label.toLowerCase().includes(t) || a.tag.toLowerCase() === t);
    idx = 0; render();
  };
  const open  = () => { dom.dataset.open = 'true'; input.value = ''; filter(''); requestAnimationFrame(() => input.focus()); document.body.style.overflow = 'hidden'; };
  const close = () => { dom.dataset.open = 'false'; document.body.style.overflow = ''; };
  const fire  = () => { results[idx]?.go(); close(); };

  dom.addEventListener('click', e => { if (e.target.matches('[data-close]')) close(); const r = e.target.closest('.hdr3-cmdk-row'); if (r) { idx = [...list.children].indexOf(r); fire(); }});
  input.addEventListener('input', () => filter(input.value));
  input.addEventListener('keydown', e => {
    if (e.key === 'Escape') return close();
    if (e.key === 'ArrowDown') { e.preventDefault(); idx = (idx + 1) % results.length; render(); }
    if (e.key === 'ArrowUp')   { e.preventDefault(); idx = (idx - 1 + results.length) % results.length; render(); }
    if (e.key === 'Enter')     { e.preventDefault(); fire(); }
    if (e.key === 'Backspace' && !input.value && stack.children.length > 1) { stack.lastElementChild.remove(); }
  });
  return { open, close };
})();

/* global ⌘K / Ctrl+K binding + chip flash */
document.addEventListener('keydown', e => {
  if ((e.metaKey || e.ctrlKey) && e.key.toLowerCase() === 'k') {
    e.preventDefault();
    if (kbdChip) { kbdChip.classList.add('is-pressed'); setTimeout(() => kbdChip.classList.remove('is-pressed'), 140); }
    palette.open();
  }
});
/* clicking the search input on desktop also opens the palette (≥881px) */
root.querySelector('.hdr3-search-input')?.addEventListener('focus', e => {
  if (window.matchMedia('(min-width: 881px)').matches) { e.target.blur(); palette.open(); }
});
```

**Reference**: Wave 1 interaction REF 1 (cmd-K palette + breadcrumb back-stack — Linear/Raycast/Rauno) — https://linear.app + https://rauno.me/craft. Reinforced by interaction REF 2 (kbd chip flash on press) — https://linear.app, https://www.raycast.com.
**Effort**: ~4hr (includes the 8-action seed list and the breadcrumb stack stub).
**RTL safe**: yes — modal is centered; placeholder + actions are Arabic-first; arrow keys are direction-neutral.
**prefers-reduced-motion**: panel and rows skip transform/animation; palette toggles via opacity only.

---

## MOVE 2: Nav-link underline that DRAWS (clip-path scaleX, not opacity)

**The moment**: Hovering a nav item, the underline doesn't fade — it grows from the inline-end edge to the inline-start, like a fountain pen drawing in real time. Eye is led by the motion of the line itself, not by a fade-in.

**Where**: `.hdr3-nav-link::after` @ v3.css:91-93. Currently uses `transform: scaleX(0)` with `transform-origin: right` — the *bones* are correct but the value isn't tied to a clip-path mask, so the underline appears to "compress" rather than draw. Replace with logical-property origin + clip-path inset for a directional draw.

**Implementation**:
```css
.hdr3-nav-link { position: relative; }
.hdr3-nav-link::after {
  content: ''; position: absolute;
  inset-inline: 14px; bottom: 6px;
  height: 1.4px; background: currentColor;
  /* draw via clip-path so it grows from the inline-end edge */
  clip-path: inset(0 0 0 100%);
  transition: clip-path .42s var(--ease-glide), background-color .2s var(--ease-out);
}
[dir="rtl"] .hdr3-nav-link::after { clip-path: inset(0 100% 0 0); }
.hdr3-nav-link:hover::after,
.hdr3-nav-link[aria-expanded="true"]::after,
.hdr3-nav-link[aria-current="page"]::after { clip-path: inset(0 0 0 0); background: var(--crimson); }

/* active section underline pulses once when scrollspy lands on it */
@keyframes hdr3UnderlinePulse { 0% { filter: drop-shadow(0 0 0 transparent); } 50% { filter: drop-shadow(0 0 6px var(--crimson-glow-2)); } 100% { filter: drop-shadow(0 0 0 transparent); } }
.hdr3-nav-link.is-active::after { animation: hdr3UnderlinePulse .9s var(--ease-glide); }

@media (prefers-reduced-motion: reduce) {
  .hdr3-nav-link::after { transition: opacity .12s linear; clip-path: none; opacity: 0; }
  .hdr3-nav-link:hover::after, .hdr3-nav-link[aria-expanded="true"]::after, .hdr3-nav-link[aria-current="page"]::after { opacity: 1; }
}
```

```js
/* lightweight scrollspy → drives `.is-active` for the pulse */
const navLinks = root.querySelectorAll('.hdr3-nav-link[href^="#"]');
const targets = [...navLinks].map(a => document.querySelector(a.getAttribute('href'))).filter(Boolean);
if (targets.length && 'IntersectionObserver' in window) {
  const io = new IntersectionObserver((entries) => {
    entries.forEach(en => {
      if (!en.isIntersecting) return;
      const link = root.querySelector(`.hdr3-nav-link[href="#${en.target.id}"]`);
      navLinks.forEach(l => l.classList.remove('is-active'));
      if (link) link.classList.add('is-active');
    });
  }, { rootMargin: '-40% 0px -55% 0px', threshold: 0 });
  targets.forEach(t => io.observe(t));
}
```

**Reference**: Wave 1 motion REF 11 (Things-style spring + craft) — https://culturedcode.com/things/ — combined with the directional-draw pattern documented in Rauno's craft archive, https://rauno.me/craft.
**Effort**: ~1hr.
**RTL safe**: yes — `inset-inline` + the explicit `[dir="rtl"]` clip-path mirror handles both directions.
**prefers-reduced-motion**: collapses to opacity-only fade.

---

## MOVE 3: Logomark path-draws on first visit, then settles

**The moment**: First page load, the hexagon outline of the logomark draws itself stroke-by-stroke in 1.1s, the inner shape fades in behind it, and the verified-checkmark *taps in* last with a tiny scale bounce. On subsequent visits within the same session it skips the draw and just renders. Once. Like Stripe Press's chapter-opener illuminations.

**Where**: `.hdr3-logo-mark` @ index.html:55-59 — the 3-path SVG (outer hex, inner shape, center dot). Add the `hdr3-logo-verify` star/check too.

**Implementation**:
```css
/* path-draw the outer hex stroke */
.hdr3-logo-mark path:nth-child(1),
.hdr3-logo-mark path:nth-child(2),
.hdr3-logo-mark circle {
  /* prepared state — drawn over by JS once getTotalLength resolves */
  stroke-dasharray: 1; stroke-dashoffset: 1; opacity: 0;
}
.hdr3-logo[data-drawn="true"] .hdr3-logo-mark path:nth-child(1) { animation: hdr3DrawHex 1.1s var(--ease-glide) forwards; }
.hdr3-logo[data-drawn="true"] .hdr3-logo-mark path:nth-child(2) { animation: hdr3FillIn .7s var(--ease-out) .55s forwards; }
.hdr3-logo[data-drawn="true"] .hdr3-logo-mark circle             { animation: hdr3FillIn .42s var(--ease-snap) .85s forwards; }
.hdr3-logo[data-drawn="true"] .hdr3-logo-verify                  { animation: hdr3StampIn .42s var(--ease-snap) 1.05s both; transform-origin: center; }
.hdr3-logo[data-drawn="false"] .hdr3-logo-mark path,
.hdr3-logo[data-drawn="false"] .hdr3-logo-mark circle,
.hdr3-logo[data-drawn="false"] .hdr3-logo-verify { stroke-dasharray: none; stroke-dashoffset: 0; opacity: 1; }

@keyframes hdr3DrawHex { from { stroke-dashoffset: 1; opacity: 1; } to { stroke-dashoffset: 0; opacity: 1; } }
@keyframes hdr3FillIn  { from { opacity: 0; } to { opacity: 1; } }
@keyframes hdr3StampIn { 0% { transform: scale(.4); opacity: 0; } 60% { transform: scale(1.18); opacity: 1; } 100% { transform: scale(1); } }

@media (prefers-reduced-motion: reduce) {
  .hdr3-logo-mark path, .hdr3-logo-mark circle, .hdr3-logo-verify { stroke-dasharray: none !important; stroke-dashoffset: 0 !important; opacity: 1 !important; animation: none !important; }
}
```

```js
/* paint pathLength accurately, then flip the data-attr exactly once per session */
const logo = root.querySelector('.hdr3-logo');
if (logo && !sessionStorage.getItem('hdr3-logo-drawn')) {
  const paths = logo.querySelectorAll('.hdr3-logo-mark path, .hdr3-logo-mark circle');
  paths.forEach(p => {
    const L = (p.getTotalLength?.() || 100);
    p.style.strokeDasharray = L; p.style.strokeDashoffset = L;
  });
  requestAnimationFrame(() => {
    logo.dataset.drawn = 'true';
    /* drive dasharray to 0 inline so animation works regardless of computed length */
    paths.forEach((p, i) => { p.style.transition = `stroke-dashoffset 1.05s var(--ease-glide) ${i*120}ms, opacity .55s var(--ease-out)`; p.style.strokeDashoffset = '0'; p.style.opacity = '1'; });
  });
  sessionStorage.setItem('hdr3-logo-drawn', '1');
} else if (logo) {
  logo.dataset.drawn = 'false';
}
```

**Reference**: Wave 1 motion REF 13 (GitHub Copilot perspective grid path-draw via stroke-dasharray) — https://github.com/features/copilot. Editorial "illuminated opener" feel from typography REF 1 (Stripe Press Poor Charlie's Almanack) — https://press.stripe.com/poor-charlies-almanack.
**Effort**: ~1.5hr.
**RTL safe**: decorative-only — the mark is centered & symmetric; no horizontal directionality.
**prefers-reduced-motion**: paths render solid immediately, no animation.

---

## MOVE 4: Mega menu opens with content morph + 60ms stagger (Linear)

**The moment**: Hovering "الشبكة" doesn't just fade the panel — the panel itself rises 8px, then each of the 7 account cards reveals top-leading→bottom-trailing on a 60ms diagonal stagger (Raycast keyboard-cascade pattern), and the bottom "آخر تحديث" timestamp ticks in last. Reads like the menu is *composing* in front of you, not popping.

**Where**: `.hdr3-mega` and `.hdr3-mega-grid` @ v3.css:97-117. Open-state already exists (`.hdr3-has-mega.is-open .hdr3-mega`) — currently it's a single opacity+transform on the panel. Add per-card stagger driven by CSS custom properties on each `.hdr3-card`.

**Implementation**:
```css
.hdr3-mega-grid .hdr3-card {
  --row: 0; --col: 0;
  opacity: 0; transform: translateY(10px);
  transition: opacity .42s var(--ease-out), transform .42s var(--ease-glide), background .24s var(--ease-glide), border-color .24s var(--ease-glide);
  /* RTL diagonal: enter from the leading edge (top-right) sweeping to bottom-left */
  transition-delay: calc(80ms + (var(--row) * 50ms) + (var(--col) * 28ms));
}
[dir="rtl"] .hdr3-mega-grid .hdr3-card {
  transition-delay: calc(80ms + (var(--row) * 50ms) + ((2 - var(--col)) * 28ms));
}
.hdr3-has-mega.is-open .hdr3-mega-grid .hdr3-card { opacity: 1; transform: none; }

.hdr3-mega-head { opacity: 0; transform: translateY(-6px); transition: opacity .32s var(--ease-out) 40ms, transform .32s var(--ease-glide) 40ms; }
.hdr3-has-mega.is-open .hdr3-mega-head { opacity: 1; transform: none; }

.hdr3-mega-foot { opacity: 0; transform: translateY(6px); transition: opacity .32s var(--ease-out) 480ms, transform .32s var(--ease-glide) 480ms; }
.hdr3-has-mega.is-open .hdr3-mega-foot { opacity: 1; transform: none; }

@media (prefers-reduced-motion: reduce) {
  .hdr3-mega-grid .hdr3-card,
  .hdr3-mega-head,
  .hdr3-mega-foot { transition: opacity .12s linear; transform: none; }
}
```

```js
/* one-time tag on each card so CSS knows its grid position */
root.querySelectorAll('.hdr3-mega').forEach(panel => {
  const cards = panel.querySelectorAll('.hdr3-card');
  const cols = 3;
  cards.forEach((c, i) => {
    c.style.setProperty('--row', String(Math.floor(i / cols)));
    c.style.setProperty('--col', String(i % cols));
  });
});
```

**Reference**: Wave 1 motion REF 4 (Linear feature pages — staggered row reveal) — https://linear.app/features. 2-D diagonal stagger from motion REF 16 (Raycast keyboard-key cascade) — https://www.raycast.com.
**Effort**: ~1hr.
**RTL safe**: needs sign-flip — the diagonal enters from the inline-start (top-right in RTL); explicit `[dir="rtl"]` rule mirrors the column index.
**prefers-reduced-motion**: collapse to a single 120ms fade, no transform, no stagger.

---

## MOVE 5: Lang toggle becomes the masthead pin (Hayy Jameel pattern)

**The moment**: The AR/EN button is no longer two pill-tabs that look like a Wix segmented control. It's a single pinned word — "EN" set in mono, with a faint hairline above and a lozenge marker beneath it — visually echoing the Hayy Jameel / Misk Art Institute masthead. The label is set in the **destination language** (visiting Arabic page → button reads "EN"; visiting English page → reads "العربية"). Click triggers a `view-transition` morph into the alternate locale page.

**Where**: `.hdr3-lang` @ index.html:108-111 + v3.css:126-129 + behavior at v3.js:57-62.

**Implementation**:
```css
/* replace the segmented pair with a single pinned destination label */
.hdr3-lang { display: inline-flex; align-items: stretch; border: 0; padding: 0; gap: 0; height: 36px; }
.hdr3-lang-pin {
  position: relative; display: inline-flex; flex-direction: column; align-items: center; justify-content: center;
  padding: 0 14px; min-width: 56px; text-decoration: none; color: var(--ink-soft);
  font-family: var(--font-mono); font-size: 11.5px; font-weight: 600; letter-spacing: 0.08em;
  transition: color .2s var(--ease-out);
}
.hdr3-lang-pin::before {
  content: ''; position: absolute; top: 0; inset-inline: 8px; height: 1px; background: var(--hairline-strong);
  transition: background .24s var(--ease-out), inset-inline .32s var(--ease-glide);
}
.hdr3-lang-pin::after {
  content: ''; position: absolute; bottom: -4px; left: 50%; width: 6px; height: 6px; border-radius: 50%;
  background: var(--crimson); transform: translateX(-50%) scale(0); transition: transform .32s var(--ease-snap);
}
.hdr3-lang-pin:hover { color: var(--ink); }
.hdr3-lang-pin:hover::before { background: var(--ink); inset-inline: 4px; }
.hdr3-lang-pin:hover::after  { transform: translateX(-50%) scale(1); }

/* destination-language label: site is AR by default → button reads "EN" */
.hdr3-lang-pin[data-target="en"] { font-family: var(--font-mono); }
.hdr3-lang-pin[data-target="ar"] { font-family: var(--font-ar-display); font-size: 14px; letter-spacing: 0; }

/* animated morph on click via View Transitions API where supported */
::view-transition-old(hdr3-lang), ::view-transition-new(hdr3-lang) { animation-duration: .42s; animation-timing-function: cubic-bezier(0.28, 0.11, 0.32, 1); }
.hdr3-lang-pin { view-transition-name: hdr3-lang; }

@media (prefers-reduced-motion: reduce) {
  .hdr3-lang-pin::before, .hdr3-lang-pin::after { transition: none; }
}
```

```html
<!-- replace .hdr3-lang block at index.html:108-111 -->
<a class="hdr3-lang-pin" href="/en/" data-target="en" hreflang="en" aria-label="Switch to English">EN</a>
<!-- on /en/ pages flip to: -->
<a class="hdr3-lang-pin" href="/" data-target="ar" hreflang="ar" aria-label="انتقل إلى العربية">العربية</a>
```

```js
/* graceful upgrade: View Transitions where supported */
root.querySelectorAll('.hdr3-lang-pin').forEach(pin => {
  pin.addEventListener('click', e => {
    if (!document.startViewTransition) return; /* default navigation */
    e.preventDefault();
    document.startViewTransition(() => { window.location.href = pin.getAttribute('href'); });
  });
});
```

**Reference**: Wave 1 arabic REF 19 (Hayy Jameel / Misk Art Institute — language toggle as masthead pin) — https://hayyjameel.org/, https://miskartinstitute.org/en. Reinforced by arabic REF 9 (Brownbook — Arabic dominates Latin in bilingual masthead) — https://brownbook.me. View-Transitions morph from motion REF 20 — MDN.
**Effort**: ~1.5hr (includes a one-line edit in `/en/index.html` to flip the destination).
**RTL safe**: yes — `inset-inline` on the hairline rule keeps the pin symmetric in either direction. The destination-language label is the entire premise.
**prefers-reduced-motion**: dot-marker doesn't animate; navigation falls back to native page load.

---

## MOVE 6: Announce ticker becomes a curated editorial-label rotation

**The moment**: The 3-phase ticker stops looking like an ecommerce promo strip ("Free shipping over 50€") and starts looking like the running label rail on a print magazine — each phase carries a typographic "category" prefix in mono ("نشرة" / "مقطع" / "تقرير") set in the sub-brand color (verde / azure / crimson) the same way the dot already does, with a 2-character index ("01/03") on the trailing side, and the *transition* between phases uses a directional letter-mask wipe (clip-path inset, similar to Move 2) so phase 02 reads as if it slid out from under phase 01 — not a vertical slide.

**Where**: `.hdr3-tick` @ index.html:45-47 + v3.css:71-77. Existing JS at v3.js:14-32 already handles the rotation; only the visual transition + DOM structure changes.

**Implementation**:
```html
<!-- updated .hdr3-tick contents (apply to all three) -->
<span class="hdr3-tick is-active" data-cat="نشرة" data-color="crimson">
  <em class="hdr3-tick-dot"></em>
  <span class="hdr3-tick-cat">نشرة · 01</span>
  <span class="hdr3-tick-body">المبادر الأسبوعية تصدر صباح كل أحد</span>
  <a href="#newsletter" class="hdr3-tick-link">اشترك الآن ↗</a>
  <span class="hdr3-tick-idx" aria-hidden="true">01<i>/</i>03</span>
</span>
```

```css
.hdr3-tick { gap: 14px; transition: opacity .42s var(--ease-glide); transform: none; clip-path: inset(0 0 0 100%); }
[dir="rtl"] .hdr3-tick { clip-path: inset(0 100% 0 0); }
.hdr3-tick.is-active { opacity: 1; clip-path: inset(0 0 0 0); transition: clip-path .55s var(--ease-glide), opacity .42s var(--ease-glide); }

.hdr3-tick-cat { font-family: var(--font-mono); font-size: 10.5px; letter-spacing: 0.18em; text-transform: uppercase; color: var(--crimson); padding-inline-end: 8px; border-inline-end: 1px solid var(--hairline-strong); }
.hdr3-tick[data-color="verde"] .hdr3-tick-cat { color: var(--verde); }
.hdr3-tick[data-color="azure"] .hdr3-tick-cat { color: var(--azure); }
.hdr3-tick-body { color: var(--ink-soft); }
.hdr3-tick-idx  { font-family: var(--font-mono); font-size: 10.5px; color: var(--ink-mute); margin-inline-start: 4px; }
.hdr3-tick-idx i { font-style: normal; opacity: .5; padding: 0 2px; }

@media (prefers-reduced-motion: reduce) {
  .hdr3-tick { clip-path: none; transition: opacity .15s linear; }
  .hdr3-tick.is-active { clip-path: none; }
}
```

**Reference**: Wave 1 typography REF 11 (It's Nice That — editorial drop-cap + category eyebrow) — https://www.itsnicethat.com. Eastern numeral / index treatment from arabic REF 16 — Aramco AR pattern.
**Effort**: ~1hr.
**RTL safe**: yes — clip-path mirror in `[dir="rtl"]` block; logical `border-inline-end` keeps the divider on the trailing side automatically.
**prefers-reduced-motion**: clip-path animation disabled; ticker still rotates via opacity (existing JS unchanged).

---

## MOVE 7: Alt reveals the X-ray grid behind the header (designer Easter-mode)

**The moment**: Anyone holds Alt/Option. Faint cream rule lines materialize behind the header drawing the 12-column container, the 72px row baseline, and a tiny lozenge at top-trailing reads `Alt · grid`. Release: gone. A power-user wink that designers & dev-curious clients will screenshot.

**Where**: `.hdr3` and `:root` — emit a CSS layer that's only painted when `[data-xray="on"]` is set on `<html>` (matches the agent prompt's directive to use Wave 1 interaction REF 6).

**Implementation**:
```css
.hdr3-xray-toast {
  position: fixed; top: 14px; inset-inline-end: 14px; z-index: var(--z-toast);
  font-family: var(--font-mono); font-size: 11px; letter-spacing: .12em; text-transform: uppercase;
  color: var(--ink-soft); background: rgba(14,21,37,.86); border: 1px solid var(--hairline-strong);
  padding: 6px 10px; border-radius: var(--r-pill);
  opacity: 0; transform: translateY(-4px); transition: opacity .18s var(--ease-out), transform .18s var(--ease-out);
  pointer-events: none;
}
[data-xray="on"] .hdr3-xray-toast { opacity: 1; transform: none; }

[data-xray="on"] .hdr3 {
  background-image:
    /* 12-column container ruler */
    linear-gradient(to right, transparent calc(100% / 12 - 1px), rgba(232,184,85,.20) calc(100% / 12 - 1px), rgba(232,184,85,.20) calc(100% / 12), transparent calc(100% / 12)),
    /* 8px baseline */
    repeating-linear-gradient(to bottom, transparent 0 7px, rgba(232,184,85,.10) 7px 8px);
  background-size: 100% 100%, 100% 8px;
  background-position: 0 0, 0 0;
}
[data-xray="on"] .hdr3-main-inner { outline: 1px dashed rgba(232,184,85,.32); outline-offset: -1px; }
[data-xray="on"] .hdr3-nav-link,
[data-xray="on"] .hdr3-cta,
[data-xray="on"] .hdr3-search,
[data-xray="on"] .hdr3-lang,
[data-xray="on"] .hdr3-burger { outline: 1px dotted rgba(232,184,85,.40); outline-offset: 2px; }
```

```html
<!-- once, near the closing </header> -->
<div class="hdr3-xray-toast" aria-hidden="true">Alt · grid</div>
```

```js
/* respect inputs: don't toggle while user is typing in search/cmdk */
const isTyping = (el) => el && el.matches('input, textarea, [contenteditable="true"]');
window.addEventListener('keydown', e => {
  if (e.key === 'Alt' && !isTyping(document.activeElement)) document.documentElement.dataset.xray = 'on';
});
window.addEventListener('keyup', e => {
  if (e.key === 'Alt') delete document.documentElement.dataset.xray;
});
window.addEventListener('blur', () => delete document.documentElement.dataset.xray);
```

**Reference**: Wave 1 interaction REF 6 (Rauno X-Ray) — https://rauno.me/craft/x-ray-interaction.
**Effort**: ~1hr.
**RTL safe**: yes — toast uses `inset-inline-end`; column ruler is symmetric.
**prefers-reduced-motion**: state-based, no animation; toast just toggles opacity (160ms — under the 200ms threshold).

---

## MOVE 8: Sticky shrink choreographed (header *condenses* on scroll)

**The moment**: Currently `.is-scrolled` hides the announce strip and shortens `.hdr3-main-inner` from 72→60px. The transition exists but every part fires at the same moment, so the eye reads it as "the page jumped." Choreograph it: announce strip collapses first (180ms), then the main row shrinks 100ms later, the logomark scales 0.94, and the sticky background blur deepens last. The user *feels* the header lock into position.

**Where**: `.hdr3.is-scrolled` rules @ v3.css:64-66, 80. Add `.hdr3.is-scrolled .hdr3-logo-mark` and `.hdr3.is-scrolled` blur-shift.

**Implementation**:
```css
/* override the existing .hdr3-announce / .hdr3-main-inner timings */
.hdr3-announce { transition: height .26s var(--ease-glide) 0ms, opacity .22s var(--ease-out) 0ms, border-bottom-color .26s var(--ease-out) 0ms; }
.hdr3-main-inner { transition: height .32s var(--ease-glide) 100ms; }
.hdr3-logo-mark  { transition: transform .32s var(--ease-glide) 140ms; }
.hdr3-cta        { transition: transform .32s var(--ease-glide) 140ms, padding .32s var(--ease-glide) 140ms, box-shadow .42s var(--ease-out) 100ms; }
.hdr3            { transition: background-color .35s var(--ease-out) 200ms, backdrop-filter .35s var(--ease-out) 200ms, border-bottom-color .35s var(--ease-out) 200ms; }

.hdr3.is-scrolled .hdr3-logo-mark { transform: scale(.92); }
.hdr3.is-scrolled .hdr3-cta { padding: 7px 14px; }
.hdr3.is-scrolled { box-shadow: 0 1px 0 0 var(--hairline), 0 12px 32px -16px rgba(0,0,0,.4); }

@media (prefers-reduced-motion: reduce) {
  .hdr3-announce, .hdr3-main-inner, .hdr3-logo-mark, .hdr3-cta, .hdr3 { transition: none !important; }
}
```

**Reference**: Wave 1 motion REF 14 (Notion / Rauno — three-tier duration discipline) — https://rauno.me/craft/interaction-design. The "choreograph the same trigger across staggered targets" pattern is from motion REF 4 / REF 13 applied at the chrome layer.
**Effort**: ~0.5hr.
**RTL safe**: yes — purely vertical motion + scaling; no inline-direction.
**prefers-reduced-motion**: all transitions zeroed; class still toggles so structure changes happen instantly.

---

## Top 3 — ship-first

| # | Move | Why it's #1 / #2 / #3 |
|---|------|-----------------------|
| 1 | **MOVE 1 — real ⌘K palette behind the rendered chip** | The biggest "screenshot moment" available, and the chip is *already* in the DOM lying about itself. Closing the loop between the visible affordance and actual behavior is the single highest-leverage hour on this section. Positions Almobadir alongside Linear / Vercel / Raycast immediately. Wave 1 interaction REF 1. |
| 2 | **MOVE 5 — masthead pin language toggle** | Editorial Saudi-business audience. Every reference site (Hayy Jameel, Misk, Brownbook) treats this as a positioning statement, not a UI tax. ~1.5hr swap that re-frames the brand before any content loads. Touches the cultural register the audit will be judged against. Wave 1 arabic REF 19 + REF 9. |
| 3 | **MOVE 3 — logomark path-draw on first visit** | The mark *introduces itself* once. Sets a tone of "this team thinks about every glyph" before the user reaches the hero. One-and-done per session so it never becomes annoying. Lowest copy / content risk. Wave 1 motion REF 13 + typography REF 1. |

---

## 200-word summary

Eight moves take the header from "carefully assembled template" to "Linear-tier chrome." The chassis is already 80% there — Apple-blur sticky, ticker timing, mega panel, kbd chip — but every promise the markup makes is *unkept*. MOVE 1 keeps the kbd chip's promise with a real palette; MOVE 2 turns the underline fade into a genuine clip-path *draw*; MOVE 3 lets the logomark introduce itself once via stroke-dasharray on first session visit; MOVE 4 lifts the mega menu from a single pop into a 60ms diagonal stagger across the 7 account cards; MOVE 5 reframes the AR/EN toggle as a Hayy-Jameel-style masthead pin in destination language; MOVE 6 turns the announce ticker into a magazine-style category rail with index numerals; MOVE 7 hides an Alt-key X-ray grid for designer-curious visitors; MOVE 8 choreographs the sticky-shrink so the header *condenses* instead of jumping. Ship #1, #5, #3 first — half a day, three Stripe/Linear-grade moments. Save the rest for a second pass that polishes the announce strip and mega menu without further breaking-change risk. RTL safety and `prefers-reduced-motion` fallbacks are baked in per move; no Three.js, no WebGL, no licensed fonts required.
