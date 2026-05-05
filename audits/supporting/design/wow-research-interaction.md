# Almobadir — Interaction × Delight Reference Recon

**Brief:** Build a library of signature micro-interactions from 2024-2026 reference sites. Not general polish — specific gestures that become talking points. Editorial Saudi business brand: tasteful, no sound, no Three.js cursor cosplay.

**Method:** Live Playwright audits of Linear, Stripe, Stripe Press, Vercel, Rauno's craft archive, Raycast, Cultured Code (Things 3), Notion docs, Figma blog, plus targeted searches for Apple press-and-hold haptics, Konami easter-egg taste tier, and copy-clipboard confirmation patterns.

---

## REF 1: cmd-K Command Palette (with breadcrumb back-stack)
- **Where:** linear.app (logged-in product), raycast.com hero demo, rauno.me/craft/⌘K-Breadcrumbs
- **The gesture:** User hits ⌘K, a centered modal cross-fades over a backdrop blur, fuzzy-search input is focused, results animate in stagger; Backspace at empty input pops one breadcrumb level instead of closing.
- **Why it earns wow:** The breadcrumb back-stack — each navigation feels like Finder, not a typeahead. It becomes the site's "I know what I'm doing" badge.
- **Implementation:** `cmdk` library (pacocoursey/cmdk) or Radix Command. Open via global `keydown` listener; trap focus; render in a portal with `backdrop-filter: blur(8px)`. Breadcrumb state in a stack array; Backspace at `query === ''` pops.
- **Code sketch:**
  ```js
  useHotkeys('mod+k', () => setOpen(o => !o));
  const onKeyDown = e => {
    if (e.key === 'Backspace' && !query && stack.length) {
      e.preventDefault(); setStack(s => s.slice(0,-1));
    }
  };
  // Modal: bg rgba(0,0,0,0.6) + backdrop-blur(8px); 100ms cubic-bezier(.25,.46,.45,.94)
  ```
- **Almobadir analog:** Global ⌘K opens a palette with: jump to case study, copy contact email, switch EN↔AR, view manifesto. Pre-loads 12 actions; site stays a brochure but feels like a tool.
- **prefers-reduced-motion fallback:** Skip the backdrop-blur fade, just toggle `display`. No stagger on results. Search still works.

---

## REF 2: Keyboard Hint Tags Inline with Buttons
- **Where:** linear.app (any CTA, e.g. "Open app" surfaces ⌘K hint near search trigger), raycast.com pricing
- **The gesture:** Buttons display a small `kbd` chip with the shortcut in their right edge ("Try free   T"). Hovering elevates the chip 1px and tints it; pressing the actual key triggers a 50ms scale on the chip so the user sees their key registered.
- **Why it earns wow:** Teaches the keyboard layer without a tutorial. The chip-press flash is the magic; it makes the page feel like an app.
- **Implementation:** Render `<kbd>` siblings inside CTAs. Global keydown finds matching `[data-key="t"]` and adds `.pressed` class for 120ms; CSS does the rest.
- **Code sketch:**
  ```css
  kbd { transition: transform .12s, background .12s; }
  kbd.pressed { transform: scale(.92); background: rgba(255,255,255,.18); }
  ```
  ```js
  document.addEventListener('keydown', e => {
    const k = document.querySelector(`kbd[data-key="${e.key.toLowerCase()}"]`);
    if (k) { k.classList.add('pressed'); setTimeout(() => k.classList.remove('pressed'), 120); }
  });
  ```
- **Almobadir analog:** Hero CTA gets a tiny ⏎ chip; Manifesto gets `M`; Contact gets `C`. Side benefit: builds the keyboard vocabulary the Almobadir audience expects from a "thinking" brand.
- **prefers-reduced-motion fallback:** Chip stays visible, just no scale flash on press; class still toggles for assistive tech, just CSS skips the transform.

---

## REF 3: Stripe-Style Animated Number Tabular Roll
- **Where:** stripe.com / stripe.com/pricing (region toggle re-renders fees), stripe.com/atlas (signup counters)
- **The gesture:** User toggles a region/tier and each number digit rolls vertically like a slot reel — old digit slides up out of frame as new digit slides in from below. Tabular-numeric font keeps everything monospaced so the layout never jitters.
- **Why it earns wow:** The roll respects digit-place (only changing digits animate); the rest stays stone-still. Feels expensive and precise.
- **Implementation:** Per-digit `<span>` with `font-variant-numeric: tabular-nums`. On change, run a Framer Motion `<AnimatePresence mode="popLayout">` with `y: -100% → 0 → 100%`. 240ms duration, easeOut.
- **Code sketch:**
  ```jsx
  const Digit = ({ d }) => (
    <AnimatePresence mode="popLayout" initial={false}>
      <motion.span key={d}
        initial={{ y: '100%' }} animate={{ y: 0 }} exit={{ y: '-100%' }}
        transition={{ duration: 0.24, ease: [0.25,0.46,0.45,0.94] }}
        style={{ display:'inline-block', fontVariantNumeric:'tabular-nums' }}>
        {d}
      </motion.span>
    </AnimatePresence>
  );
  ```
- **Almobadir analog:** "Cases shipped" counter animates on scroll into view. Service-rates table rolls when toggling currency (SAR ↔ USD).
- **prefers-reduced-motion fallback:** Cross-fade only (`opacity 0 → 1`, no `y`). Number still updates instantly.

---

## REF 4: Stripe Press Book Tilt (cursor parallax)
- **Where:** press.stripe.com (book grid hover)
- **The gesture:** Hover any book cover; the cover tilts on X/Y based on cursor position relative to its center, like turning a real book in your hand. Spring-physics lerp; release returns home with a slight overshoot.
- **Why it earns wow:** Physical-feeling without WebGL. Cursor becomes a finger holding the object; the spring on release sells the weight.
- **Implementation:** `pointermove` handler on each card. Compute `(cursorX - rect.center.x) / rect.width` to get -0.5..0.5; multiply by max-tilt (8deg). Apply `transform: perspective(1000px) rotateY(...) rotateX(...)` with `transform-style: preserve-3d`. On `pointerleave`, animate back via `requestAnimationFrame` lerp at ~0.12.
- **Code sketch:**
  ```js
  card.addEventListener('pointermove', e => {
    const r = card.getBoundingClientRect();
    const x = ((e.clientX - r.left)/r.width - .5) * 16;
    const y = ((e.clientY - r.top)/r.height - .5) * -16;
    card.style.transform = `perspective(900px) rotateY(${x}deg) rotateX(${y}deg)`;
  });
  card.addEventListener('pointerleave', () => card.style.transform = '');
  // CSS: .card { transition: transform .35s cubic-bezier(.34,1.56,.64,1); }
  ```
- **Almobadir analog:** Case-study cards in the grid get this tilt. The cubic-bezier overshoot on leave (`(.34,1.56,.64,1)`) gives the springiness without a JS spring engine.
- **prefers-reduced-motion fallback:** No tilt; replace with a 1px translateY lift on hover.

---

## REF 5: Things-3 "Magic Plus" Drag-to-Insert
- **Where:** culturedcode.com/things (mobile/desktop demo)
- **The gesture:** A floating "+" button. Tap = new task at current location. Drag-and-drop = inserts a new task at the drop location (dropping into a date target schedules it). The button itself slightly deforms (liquid jelly) as you drag — squashes in the direction of motion.
- **Why it earns wow:** A single button reveals new affordances on hold. The liquid deformation is the screenshot moment.
- **Implementation:** Pointer capture + drag tracking. SVG-based "+" icon with two children whose `cx/cy` lerp toward velocity vector during drag (gives jelly squash). Drop target highlight via `:has(.is-dragging)` once Tailwind/modern CSS is in.
- **Code sketch:**
  ```js
  let vx=0, vy=0, lastX, lastY;
  btn.addEventListener('pointermove', e => {
    if (!dragging) return;
    vx = e.clientX - lastX; vy = e.clientY - lastY;
    btn.style.transform = `translate(${e.clientX-x0}px, ${e.clientY-y0}px) scale(${1+Math.hypot(vx,vy)*.005}, ${1-Math.hypot(vx,vy)*.003})`;
    lastX = e.clientX; lastY = e.clientY;
  });
  ```
- **Almobadir analog:** Manifesto page floating "Note" pill — drag it onto a paragraph to highlight it (saves to local "your annotations" panel). Editorial brands love tools that feel like reading-as-craft.
- **prefers-reduced-motion fallback:** No jelly squash, button just translates with the pointer; drop targets get a static border highlight instead of a pulse.

---

## REF 6: Rauno-Style Hover Detail Reveal (X-Ray / Underlay Mode)
- **Where:** rauno.me/craft/x-ray-interaction (concept), vercel.com (button states), rauno.me/craft/devouring-details
- **The gesture:** Hold a modifier key (Option/Alt) and the page reveals its grid, baseline, and component edges as a faint outline overlay. Release = back to clean. Power-user mode that makes the page feel infinitely deep.
- **Why it earns wow:** Editorial brands win by letting the curious pull back the curtain. Designers and dev-curious clients screenshot it.
- **Implementation:** Listen for `keydown`/`keyup` on Alt; toggle `data-xray="on"` on `<html>`. CSS targets `[data-xray="on"] *` with `outline: 1px dashed rgba(...)`. Optional: show a small toast "X-Ray: Alt to toggle".
- **Code sketch:**
  ```js
  addEventListener('keydown', e => e.altKey && document.documentElement.dataset.xray='on');
  addEventListener('keyup',   e => !e.altKey && delete document.documentElement.dataset.xray);
  ```
  ```css
  [data-xray="on"] [data-grid] { outline: 1px dashed rgba(212,175,55,.4); }
  [data-xray="on"]::before { content: ''; position: fixed; inset: 0;
    background-image: linear-gradient(to right, rgba(212,175,55,.05) 1px, transparent 1px);
    background-size: 8.333% 100%; pointer-events: none; }
  ```
- **Almobadir analog:** Alt reveals the 12-column grid + baseline; a small `kbd` lozenge fades in saying "Alt = grid". The grid itself is a brand asset (gold rule lines).
- **prefers-reduced-motion fallback:** Same toggle, no fade-in — outlines appear immediately. No animation involved here; it's purely state-based.

---

## REF 7: Cursor-as-Character (section-aware cursor swap)
- **Where:** rauno.me archive (Vanish Input, Wheel Input prototypes), gallery sites that change cursor per section
- **The gesture:** Default cursor is a single 6px dot following the pointer with a 60ms delay. Over interactive elements: dot inflates into a 28px ring labeled "Read" / "View" / "Copy" depending on section data-attr. Over text: dot becomes a thin I-beam.
- **Why it earns wow:** Cursor stops being browser chrome; it's part of the brand. The label-on-hover ("Read", "View") replaces tooltips entirely.
- **Implementation:** A fixed 0×0 div tracks `pointermove` with lerp. CSS `cursor: none` on `<html>`. Targets with `data-cursor="view"` set a CSS var `--cursor-mode` on the parent which restyles the dot via `:has()`.
- **Code sketch:**
  ```js
  const dot = document.querySelector('.dot');
  let x=0,y=0,tx=0,ty=0;
  addEventListener('pointermove', e => { tx=e.clientX; ty=e.clientY; });
  (function tick(){ x+=(tx-x)*.18; y+=(ty-y)*.18; dot.style.transform=`translate(${x}px,${y}px)`; requestAnimationFrame(tick); })();
  // hover detection via :has() or JS
  document.querySelectorAll('[data-cursor]').forEach(el =>
    el.addEventListener('pointerenter', () => dot.dataset.mode = el.dataset.cursor));
  ```
- **Almobadir analog:** Default = quiet dot. Over case studies: ring with "View case". Over founder portrait: ring with "Read story". Over copy: I-beam. **Touch devices: skip the custom cursor entirely** (`@media (hover: hover)` gates the whole thing).
- **prefers-reduced-motion fallback:** Disable the lerp; cursor instantly snaps to pointer (or restore native cursor entirely — safer).

---

## REF 8: Scroll-Cue "Wormhole" (the next section reaches up)
- **Where:** rauno.me/craft (scroll cues throughout), portfolio sites of motion designers, Apple's iPhone product pages
- **The gesture:** Near the bottom of each section, a small vertical line + label ("Scroll", or just `↓`) pulses gently — but the real move: as you scroll within 200px of the next section, the next section's first heading lifts up *into* the line, like the line is a magnet pulling content from below.
- **Why it earns wow:** Most "scroll down" arrows are dead. This one *does something* as you obey it; the cue becomes the transition.
- **Implementation:** IntersectionObserver on the next section's heading. Map intersection ratio to `translateY` and `opacity` of the heading; the cue line shrinks proportionally. Use Framer Motion `useScroll` for cleaner code.
- **Code sketch:**
  ```js
  const { scrollYProgress } = useScroll({ target: nextRef, offset: ['start end','start center'] });
  const y = useTransform(scrollYProgress, [0, 1], [40, 0]);
  const cueLen = useTransform(scrollYProgress, [0, 1], [48, 0]);
  return <>
    <motion.div style={{ height: cueLen }} className="cue-line" />
    <motion.h2 style={{ y, opacity: scrollYProgress }} ref={nextRef}>{title}</motion.h2>
  </>;
  ```
- **Almobadir analog:** Between Manifesto → Method, the gold rule line shrinks as Method's `01` lifts up. Between Method → Cases, the line becomes the leading "0" of "01 / Discover".
- **prefers-reduced-motion fallback:** Static cue line; heading appears at full position immediately when section is in viewport.

---

## REF 9: Copy-to-Clipboard with Scribble Confirmation
- **Where:** vercel.com docs (copy snippets), supabase docs, every-shadcn-site copy buttons, but the editorial twist on tobiasahlin/codypearce blog posts
- **The gesture:** Click a small copy icon next to email / URL / code snippet. Icon morphs (clipboard → checkmark) with a 220ms path-draw animation. A tiny ink-stroke "✓ copied" floats up 12px and fades over 1.4s. Returns to clipboard icon after.
- **Why it earns wow:** The path-draw checkmark feels handwritten — fits an editorial brand way better than a stamp/toast.
- **Implementation:** Inline SVG with two path elements (clipboard, check). Animate `stroke-dashoffset` on the check path from `path.getTotalLength()` to 0. State machine: `idle → copied (1400ms) → idle`.
- **Code sketch:**
  ```jsx
  const [copied, setCopied] = useState(false);
  const copy = () => {
    navigator.clipboard.writeText(text);
    setCopied(true); setTimeout(() => setCopied(false), 1400);
  };
  // SVG: <path className={copied ? 'check drawn' : 'check'} d="M4 12 l5 5 11-12" pathLength="1" />
  // CSS: .check { stroke-dasharray: 1; stroke-dashoffset: 1; transition: stroke-dashoffset .22s .05s; }
  //      .check.drawn { stroke-dashoffset: 0; }
  ```
- **Almobadir analog:** Footer email + WhatsApp number get this. Each case-study quote has a "copy quote" button — readers share quotes back to clients.
- **prefers-reduced-motion fallback:** No path-draw — checkmark pops in instantly with `opacity 0 → 1` (40ms). Floating "copied" label still appears.

---

## REF 10: Press-and-Hold to Confirm (Apple-style)
- **Where:** Apple App Store / Music in-app purchases (native), web replicas: Vercel deletion confirmations, Stripe destructive actions
- **The gesture:** For destructive or "expensive" actions (e.g. "Empty inbox", "Send"). User presses and holds the button; a circular progress ring fills counter-clockwise around the button over 600-800ms. Release early = cancels (ring snaps back). Hold to fill = action fires + a single soft pulse.
- **Why it earns wow:** Removes the "Are you sure?" modal entirely. The user *feels* the commitment; momentum signals intent.
- **Implementation:** `pointerdown` starts a CSS animation on a circular SVG `stroke-dashoffset`; `pointerup`/`pointerleave` cancels. `animationend` fires the action. No haptics on web — the visual ring is the only feedback.
- **Code sketch:**
  ```jsx
  const onDown = () => ring.classList.add('filling');
  const onUp   = () => ring.classList.remove('filling');
  const onEnd  = () => { /* fire action */ ring.classList.remove('filling'); };
  // CSS:
  // .ring { stroke-dasharray: 100; stroke-dashoffset: 100; }
  // .ring.filling { transition: stroke-dashoffset .7s linear; stroke-dashoffset: 0; }
  // .ring:not(.filling) { transition: stroke-dashoffset .2s ease-out; stroke-dashoffset: 100; }
  ```
- **Almobadir analog:** "Send brief" CTA on the contact form uses press-and-hold. Visitors who try-it-once will tell colleagues — and zero spam-bots will hold the button for 700ms.
- **prefers-reduced-motion fallback:** Convert to a standard click + native `confirm()` dialog (or a one-step click for non-destructive variants). Ring not animated.

---

## REF 11: Live Presence Dots / "Someone is reading this" Cursor Echo
- **Where:** Figma multiplayer canvas (cursor + name), Supabase realtime demos, Liveblocks site
- **The gesture:** A tiny floating dot in the corner: "3 reading now". On hover, it expands into 3 named cursors (anonymized: "Reader from Riyadh", "Reader from Jeddah") drifting across the page like ghosts for 2-3s, then fade. Updates via WebSocket from a presence service.
- **Why it earns wow:** Editorial site feels alive without comments or chat. Place-of-origin labels (Riyadh / Jeddah / Cairo) make it locally relevant.
- **Implementation:** Liveblocks or Supabase Realtime presence. Each visitor publishes throttled `pointermove` (~16Hz). Render others' cursors as absolutely-positioned dots with name pill. **Throttle hard, capture only first city via geolocation IP API server-side, never PII.**
- **Code sketch:**
  ```js
  const room = liveblocks.enter('almobadir-home', { initialPresence: { cursor: null, city }});
  addEventListener('pointermove', throttle(e =>
    room.updatePresence({ cursor: { x: e.clientX/innerWidth, y: e.clientY/innerHeight }}), 60));
  // render others.cursor, others.city in a portal
  ```
- **Almobadir analog:** Manifesto page only. "Someone is reading this with you." (Stays subtle.) For an editorial Saudi brand it reinforces "this is read, this is alive."
- **prefers-reduced-motion fallback:** Drop the cursor drift entirely; show a static count chip only ("3 reading").

---

## REF 12: Type-the-Phrase Easter Egg (tasteful, brand-anchored)
- **Where:** Konami-code era classics, modern editorial: type "ahoy" on Slack, type "matrix" on certain dev sites, tobiasahlin's blog
- **The gesture:** User types a phrase anywhere on the page (no input focused). The phrase matters: it's the brand's word — for Almobadir, type **"mubadir"** (Arabic transliteration of "initiator"). Result: the page's serif headings shift into the brand's display weight for 8s, the rule lines pulse once, and a faint footnote appears: "أهلاً" (welcome). One-time per session.
- **Why it earns wow:** It's not a Konami code (that's tired). It's the brand's name. Discovering it makes the discoverer feel like an insider, not a gamer.
- **Implementation:** Buffer last N keystrokes in a string; on match, fire effect; clear buffer. Persist `sessionStorage` flag to prevent repeats.
- **Code sketch:**
  ```js
  let buf = '';
  addEventListener('keydown', e => {
    if (e.target.matches('input, textarea')) return;
    buf = (buf + e.key).slice(-10);
    if (buf.toLowerCase().endsWith('mubadir') && !sessionStorage.eg) {
      sessionStorage.eg = 1;
      document.documentElement.classList.add('eg-mubadir');
      setTimeout(() => document.documentElement.classList.remove('eg-mubadir'), 8000);
    }
  });
  ```
- **Almobadir analog:** Exactly as described. Tasteful: no rickrolls, no dinosaurs, no "Wilkie!". Just the brand revealing a deeper voice. Saudi business audience won't be alienated.
- **prefers-reduced-motion fallback:** No rule-line pulse, no font-weight transition; the welcome footnote still appears statically for 8s. The "secret" survives.

---

## REF 13: Hover-Earned Reveal (the "1px" payoff)
- **Where:** linear.app (sidebar item hover reveals secondary actions), notion.com (block-handles appear on row hover), figma.com/blog cards
- **The gesture:** List rows look completely static at rest — no chevrons, no metadata, no hint of interactivity. On hover: a `→`, a date stamp, and a meta tag (e.g. "Sept 2025 · Riyadh") fade in 8px from the right over 180ms. Cursor leaves: they fade back. Nothing pushes layout.
- **Why it earns wow:** The clean state stays clean. The reveal is "earned" — doesn't shout for attention. List browsing feels meditative.
- **Implementation:** Items have `position: relative`; secondary content is `position: absolute; right: 0; opacity: 0; transform: translateX(-8px)`. Hover toggles to `opacity: 1; translateX(0)`. **Crucially: do not change layout flow on hover** — hover-reveal that pushes content is the bug.
- **Code sketch:**
  ```css
  .row .meta { 
    position: absolute; right: 0; top: 50%; transform: translate(-8px, -50%);
    opacity: 0; transition: opacity .18s, transform .18s;
    transition-timing-function: cubic-bezier(.25,.46,.45,.94);
  }
  .row:hover .meta { opacity: 1; transform: translate(0, -50%); }
  ```
- **Almobadir analog:** Case-study index. Every row is a clean title at rest; on hover the year, sector, and `→` fade in. Manifesto page section list = same.
- **prefers-reduced-motion fallback:** No translateX — opacity-only transition (opacity is exempt from reduced-motion guidance per WCAG 2.3.3).

---

## REF 14: Type-Specimen Live Slider (Frere-Jones / Hoefler-school)
- **Where:** frerejones.com (specimen pages with tracking sliders), commercialtype.com, OH no Type
- **The gesture:** A specimen line of type. User drags a horizontal slider; the specimen's weight (or size, or tracking) interpolates live via a variable font. No reload, no jump — every pixel of the drag changes the letterforms.
- **Why it earns wow:** Letters become a physical material. Hard to do without variable fonts; impossible to fake. Designers and editors will stay 60s on a page that does this.
- **Implementation:** Variable font with `wght` axis loaded as a single .woff2 (~80KB). Slider `<input type="range">` writes to CSS custom property `--wght`; specimen has `font-variation-settings: "wght" var(--wght)`.
- **Code sketch:**
  ```html
  <input type="range" min="100" max="900" step="1" id="w" value="400">
  <p class="specimen">المبادر</p>
  <style>
    .specimen { font-variation-settings: 'wght' var(--wght, 400); transition: font-variation-settings .08s; }
  </style>
  <script>
    w.addEventListener('input', e => document.documentElement.style.setProperty('--wght', e.target.value));
  </script>
  ```
- **Almobadir analog:** Manifesto page top: an Arabic + Latin specimen of the brand display face with a weight slider. Reinforces "we care about every glyph" — perfect signal for editorial Saudi B2B.
- **prefers-reduced-motion fallback:** Remove the `transition` — font weight still changes on input, just instantly per frame. Functionally identical without the smoothing.

---

## REF 15: Magnetic CTA (cursor pulls the button toward itself)
- **Where:** Vercel hero CTAs, Linear "Get started" CTAs, lots of award-show sites — but Linear/Vercel keep it subtle (3-6px), which is the right dose
- **The gesture:** Within ~80px of a primary CTA, the button gently translates toward the cursor (max 6px). The button's inner label translates further (8px) creating a tiny parallax. Leave the radius: spring back home.
- **Why it earns wow:** Buttons feel sentient. The label-leads-button parallax is the detail: the button feels like it's *trying* to be useful.
- **Implementation:** Wrapper div listens for `pointermove`. Distance from cursor → translate ratio. **Critical**: use `pointermove` on a parent zone (not the button itself), or magnetism only triggers on the button face.
- **Code sketch:**
  ```js
  const zone = btn.parentElement; // gives ~80px hit radius via padding
  zone.addEventListener('pointermove', e => {
    const r = btn.getBoundingClientRect();
    const dx = (e.clientX - (r.left+r.width/2)) * .15;
    const dy = (e.clientY - (r.top+r.height/2)) * .15;
    btn.style.transform = `translate(${Math.max(-6,Math.min(6,dx))}px,${Math.max(-6,Math.min(6,dy))}px)`;
    btn.querySelector('.label').style.transform = `translate(${dx*.4}px,${dy*.4}px)`;
  });
  zone.addEventListener('pointerleave', () => { btn.style.transform=''; /* spring CSS does the rest */});
  // CSS: .btn, .label { transition: transform .35s cubic-bezier(.34,1.56,.64,1); }
  ```
- **Almobadir analog:** Hero "Start a brief" CTA + footer "Contact us". Don't apply magnetism to navigation links — too gimmicky and breaks Fitts's law for everyday clicks.
- **prefers-reduced-motion fallback:** No magnetism at all. Static button with hover background-tint only.

---

# Top 5 Most-Portable Signature Interactions

Ranked for Almobadir specifically — editorial Saudi business brand, 10/10 craft bar, no sound, no Three.js.

### 1. **REF 1 — cmd-K Command Palette with breadcrumb back-stack**
The single most portable, screenshot-worthy gesture in the modern web. Battle-tested library (`cmdk`), accessible by default (Radix), positions Almobadir alongside Linear/Vercel/Raycast. Breadcrumb back-stack is the differentiator — keeps it from feeling like everyone else's palette. **Effort: 1 day. Wow per byte: highest.**

### 2. **REF 14 — Type-Specimen Live Variable-Font Slider**
Tells the entire brand story (typographic care, Arabic + Latin parity) in one gesture. Costs ~80KB of variable font + 30 lines of code. Saudi audience particularly receptive — Arabic letterforms + variability = rare web sighting. **Effort: 0.5 day. Brand fit: highest.**

### 3. **REF 13 — Hover-Earned Reveal (the "1px" payoff)**
Powers the entire case-study index, manifesto sections, and footer. Quietly portable across every list on the site; the discipline of "no layout shift on hover" elevates the perceived craft of everything around it. **Effort: 0.25 day. Use-frequency: every list.**

### 4. **REF 6 — X-Ray Grid (Alt-key reveal)**
Designer-bait that doesn't compromise the editorial reading experience. Self-reveals via small `kbd` lozenge. Keeps the "this team thinks in grids" subtext available to anyone curious enough to find it. **Effort: 0.5 day. Insider signal: high.**

### 5. **REF 12 — "mubadir" Type-the-Phrase Easter Egg**
The brand-anchored variant — not Konami, not gimmicky. Reveals a layer of voice (display weight + rule pulse + welcome footnote) only to those who type the brand's name. Tasteful, one-time per session, completely accessibility-safe. **Effort: 0.25 day. Storytelling payoff: disproportionate.**

---

## 200-word Summary

Fifteen signature interactions audited from live 2024-2026 reference sites. The dataset clusters into three families: **keyboard layer** (REF 1, 2, 6, 12) which makes the site feel like a tool rather than a brochure; **physical-feeling craft** (REF 3, 4, 5, 7, 14, 15) which uses pointer parallax, variable fonts, and lerp/spring tricks to make pixels feel like material; and **earned reveals** (REF 8, 9, 10, 11, 13) which keep the resting state clean and reward the curious gesture. Every entry is implementable in vanilla JS + CSS with optional Framer Motion, none requires WebGL, none uses sound, and every entry has a `prefers-reduced-motion` fallback that preserves the underlying functionality. The recommended starter pack — items #1, #14, #13, #6, #12 — is achievable in roughly three engineering days and gives Almobadir three distinct conversation pieces (the palette, the type slider, the brand easter egg) plus two ambient-craft pieces (the X-ray and the earned reveal) that elevate everything around them. The brand-anchored "mubadir" easter egg replaces the tired Konami code with something tasteful enough for an editorial Saudi business audience while still rewarding insider discovery.

---

## Sources

- [Linear app](https://linear.app)
- [Linear Method](https://linear.app/method)
- [Stripe](https://stripe.com)
- [Stripe Pricing](https://stripe.com/pricing)
- [Stripe Press](https://press.stripe.com)
- [Vercel](https://vercel.com)
- [Raycast](https://www.raycast.com)
- [Rauno Freiberg craft archive](https://rauno.me/craft)
- [Rauno X-Ray Interaction](https://rauno.me/craft/x-ray-interaction)
- [Rauno ⌘K Breadcrumbs](https://rauno.me/craft) (Dec 2022 entry)
- [Cultured Code - Things 3](https://culturedcode.com/things/)
- [Things 3 features - Magic Plus](https://culturedcode.com/things/features/)
- [Notion slash commands](https://www.notion.com/help/guides/using-slash-commands)
- [Figma Multiplayer Editing](https://www.figma.com/blog/multiplayer-editing-in-figma/)
- [Figma Multi-Cursor Presence in Dev Mode](https://designilo.com/2025/07/20/understanding-figmas-multi-cursor-presence-in-dev-mode/)
- [Frere-Jones Type](https://frerejones.com/about/)
- [Supabase Realtime](https://supabase.com/realtime)
- [The Browser Company](https://thebrowser.company)
- [cmdk library (pacocoursey/cmdk)](https://github.com/pacocoursey/cmdk)
- [Konami code easter egg patterns](https://www.viget.com/articles/breaking-the-konami-code-adding-an-easter-egg-to-your-site)
- [Copy-to-clipboard tooltip patterns - Shoelace](https://shoelace.style/components/copy-button)
