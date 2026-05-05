# The Almobadir Motion Signature

**Authored:** 2026-04-26
**Inputs:** `design-audit/wow-research-motion.md` (primary), `design-audit/03-motion-animation.md` (audit history), `design-audit/wow-research-{interaction,typography,arabic}.md`, live `assets/v3.css` (1,210 LOC), `assets/tokens.css`, `assets/v3.js` (502 LOC), `index.html`.
**Status:** This document is the contract. Anything that animates on almobadir.com must trace back to a token + a ritual defined here. If a future change can't, the change is wrong, not the system.
**Audience:** designer, developer, founder. Read once, ship every motion change against it.

---

## 0. Executive summary

The audit found the right bones (one canonical glide curve used 105 times, IO-driven reveals, RAF-throttled scroll handlers, reduced-motion guards) sitting under the wrong execution: **27 distinct durations** across 96 transitions, **5 different durations for "card hover lift"**, **6 always-on infinite ornaments**, **8 layout-property transitions**, **4 of 5 declared easing tokens unused**, and **`:focus-visible` on 1 of 80 interactive elements**. The chassis is bench-grade. The discipline is not.

This signature collapses 27 durations into **3 tiers** (Notion's discipline, motion REF 14), pins the easing system to **5 named curves** with explicit owners, and binds every animatable element to **4 named rituals**: the **Almobadir Reveal** (one entrance, scroll-driven), the **Almobadir Hover** (one tactile lift, applied to every interactive surface), the **Almobadir Transition** (one between-section beat), and the **Almobadir Counter** (one easeOutCubic ramp for every number on the page). What the brand rejects is named explicitly so future PRs can be denied with a citation, not an opinion.

By the bottom of this document, two things should be true: (1) anyone can ship motion that "feels like Almobadir" without asking another question, and (2) every audit-flagged finding (27 durations, 8 layout-thrash, the spinning verified badge, the manifesto orbit, the inactive-frame GPU burn, the missing focus rings) has a one-line fix mapped to a token or ritual.

---

## 1. The token system

### 1.1 Three durations — Notion's discipline (motion REF 14)

```css
:root {
  /* ── Durations · 3 tiers ─────────────────────────────
     Repeated >100×/day actions feel slow when animated.
     Reserve animation budget for low-frequency moments. */
  --d-instant:   140ms;   /* hover, focus, micro-feedback, menu open */
  --d-base:      320ms;   /* state changes, card lift, reveal */
  --d-cinematic: 880ms;   /* entrances, scroll-driven, manifesto */
}
```

**Why these three values, and only these three.**

| Tier | Value | Bench | Rationale |
|---|---|---|---|
| `--d-instant` | **140ms** | Notion menu = ~120ms · Linear hover color = 150ms · Apple SF Symbol switch = 150ms | Below ~120ms reads as "instant" but the tail of the curve still smooths the snap. 140ms is the bench-tested floor where users can't perceive lag yet a designer can perceive easing. The current site has 6 distinct values (`0.12s, 0.14s, 0.15s, 0.2s, 0.24s, 0.25s`) where this single value wins. |
| `--d-base` | **320ms** | Linear card lift = 320ms · Stripe form border = 300ms · Things sheet open = 350ms | The "did something happen" tier. Long enough that the eye perceives transformation, short enough that it doesn't feel theatrical. Already declared in tokens.css and used 9 times correctly; the audit flagged ~85 hand-typed values clustering 280–350ms — this is the single token they should all collapse to. |
| `--d-cinematic` | **880ms** | Apple hero entrance = ~800–1000ms · Stripe Atlas section reveal = 900ms · Linear feature reveal = 900ms | The "this is a moment" tier. The audit's existing `--d-glide: 1000ms` is slightly draggy at 1s — Apple and Stripe sit at 850–950ms. 880ms is the centroid. Used only for first-paint entrances, scroll-driven reveals, and the manifesto. **Never** for hover. |

**What gets retired:** `--d-fast (150ms)`, `--d-slow (700ms)`, `--d-glide (1000ms)`, plus all 27 hand-typed values. `--d-fast → --d-instant`. `--d-slow + --d-glide → --d-cinematic`. `--d-base` keeps its name and value.

**The forbidden middle.** Anything between 320ms and 880ms is rejected by definition. The audit lists 9 durations in that gap (`0.4s, 0.45s, 0.48s, 0.5s, 0.52s, 0.55s, 0.6s, 0.65s, 0.7s`). The brand decision: **this entire band is excluded**. A motion either feels responsive (`--d-base`) or cinematic (`--d-cinematic`); there is no middle. This is the single hardest constraint and the most important one — the audit's "system has no rhythm" finding is exactly this gap.

### 1.2 Five curves — owners, not options

```css
:root {
  /* ── Curves · 5 named, every one has an owner ──────── */
  --ease-glide:   cubic-bezier(0.28, 0.11, 0.32, 1);   /* primary · 90% of motion */
  --ease-out:     cubic-bezier(0.22, 1, 0.36, 1);      /* exits · expo-out · departures */
  --ease-spring:  cubic-bezier(0.34, 1.56, 0.64, 1);   /* moments of joy · overshoot */
  --ease-snap:    cubic-bezier(0.4, 0, 0.2, 1);        /* state changes · symmetric */
  --ease-counter: cubic-bezier(0.215, 0.61, 0.355, 1); /* easeOutCubic · numbers only */
}
```

**Why these five, and what owns each one.**

| Curve | Bezier | Bench | Owner — *exclusively* used for |
|---|---|---|---|
| `--ease-glide` | `(.28, .11, .32, 1)` | Apple's signature site curve · Cyd Stumpel's scroll reveals · already 105× in v3.css | The default. Every reveal, every transform, every hover that isn't explicitly assigned another curve. If a designer asks "which curve?" and there isn't a special reason, the answer is `--ease-glide`. |
| `--ease-out` | `(.22, 1, .36, 1)` | Stripe Press exits · Apple sheet dismiss · "expo-out" in Penner library | **Exits only** — element leaves the viewport, modal closes, drawer dismisses. Asymmetric to entry: things arrive on `--ease-glide` and leave on `--ease-out`. The audit flagged that `--ease-out` was declared and used 0 times; this assigns it ownership. |
| `--ease-spring` | `(.34, 1.56, .64, 1)` | Things 3 hover (REF 11) · Press.stripe book grid · Linear ⌘K command palette open | **One-shot moments of delight only**. Explicit list: (a) hero CTA `:active` (mousedown bounce-back), (b) network card `:hover` lift on touchpad-precision pointers, (c) newsletter `nl3-submit.is-success` morph, (d) footer "back to top" arrow, (e) the manifesto period punctuation pop. Five surfaces, no more. Bouncy spring on text or on every card cheapens it; reserve. |
| `--ease-snap` | `(.4, 0, .2, 1)` | Material Design's "standard" curve · Notion menu open/close · symmetric in/out | Binary state changes that should feel mechanical, not organic: header `is-scrolled`, language toggle pill slide, mega-menu open/close, drawer open. The symmetric easing makes the back-and-forth feel like a switch, not a creature. |
| `--ease-counter` | `(.215, .61, .355, 1)` | jQuery easeOutCubic · used in v3.js founder counters via inline `1 - Math.pow(1-t, 3)` | **Numbers only**. Every `<span>` that ramps from 0 to a target value uses this exact curve. The current site has `--ease-counter` declared but JS hand-codes the cubic; we're aligning the token to JS so the system has a single source of truth. |

**What gets retired:**
- `--ease-in: cubic-bezier(.4, 0, 1, 1)` — declared, used 0 times. **Delete from tokens.css.** "Things accelerate offscreen" is not an Almobadir motion; the brand rejects it (see §3).
- 8 hardcoded `ease` keywords (audit §3) — replace each with `var(--ease-glide)`.
- 1 hardcoded `ease-in-out` (line 283 keyframe) — replace with `var(--ease-snap)`.

**Justification, line-item:**

- **`--ease-glide` stays at Apple's `(.28, .11, .32, 1)`** because (a) it's already the brand's most-used curve and (b) the audit shows it's the one part of the motion system that *is* working — 105 of 96 distinct transitions reference it. Don't re-tune what's already passing.
- **`--ease-out` at expo-out `(.22, 1, .36, 1)`** because departures want to feel decisive and out-of-sight, not lingering. Stripe and Apple both use a high-front-loaded exit so the user's eye is freed to refocus on what arrives next. Linear and Bezier `(0, 0, .2, 1)` is the W3C "ease-out" but it's perceptually the same as `--ease-glide`'s tail; expo-out gives a real personality difference so the token earns its slot.
- **`--ease-spring` at `(.34, 1.56, .64, 1)`** is the canonical Things/Penner overshoot. Y-overshoot 1.56 is the visible-but-not-cartoony band; below 1.3 you can't perceive it, above 1.8 it reads as juvenile.
- **`--ease-snap` at `(.4, 0, .2, 1)`** is Material's "standard" curve, intentionally borrowed because Material is the bench for *symmetric* state changes. It is *not* used for reveals (reveals are asymmetric — `--ease-glide` in, `--ease-out` out).
- **`--ease-counter` at `(.215, .61, .355, 1)`** is the standard easeOutCubic, isomorphic to the JS `1 - Math.pow(1-t, 3)` already in v3.js:420. Token + JS are now interchangeable; future migration to WAAPI is a one-line swap.

---

## 2. The four rituals

Every animatable element on almobadir belongs to exactly one ritual. If you find yourself writing motion that doesn't fit any of these four, stop — either the design is wrong or the system needs a fifth ritual (and a 5th ritual requires founder sign-off).

---

### Ritual 1 — The Almobadir Reveal

**The single entrance pattern every non-decorative element on the page shares.** First-time-in-viewport: rise 12px, fade in, both axes glide for `--d-cinematic`. Optional 60ms-per-row stagger when the element is part of a list (Linear pattern, motion REF 4). One named keyframe, one applied class.

**Pattern (CSS):**

```css
/* The keyframe — the one and only entrance */
@keyframes almobadir-reveal {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: none; }
}

/* The applied class — IO toggles [data-in-view] */
[data-reveal] {
  opacity: 0;
  transform: translateY(12px);
  transition: opacity var(--d-cinematic) var(--ease-glide),
              transform var(--d-cinematic) var(--ease-glide);
  transition-delay: calc(var(--reveal-i, 0) * 60ms);
}
[data-reveal][data-in-view] {
  opacity: 1;
  transform: none;
}

/* Future: progressive enhancement to scroll-driven (motion REF 3) */
@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    [data-reveal] {
      animation: almobadir-reveal linear both;
      animation-timeline: view();
      animation-range: cover 0% cover 32%;
      transition: none;             /* CSS scroll wins over JS IO */
    }
  }
}
```

**Tokens consumed:** `--d-cinematic`, `--ease-glide`, optional `--reveal-i` integer for stagger index.

**Why 12px, not 18 or 24.** The current site uses `translateY(12px)` (manifesto shadow), `translateY(18px)` (data-stagger), `translateY(20px)` (mtd3 inner), `translateY(24px)` (network card pre-load), `translateY(28px)` (network card / cnt3 card), `translateY(36px)` (mtd3 frame), `translateY(48px)` (mtd3 outer). **8 different distances for "rise on reveal."** 12px is the smallest that registers as motion and the largest that doesn't betray itself as an animation. Linear, Stripe, and Vercel all sit at 8–16px. Anything ≥24px reads as "slide" — not the same gesture.

**Why 60ms stagger, not 90 or 120.** REF 4 (Linear). Below 50ms the eye doesn't perceive sequence; above 80ms the rhythm becomes plodding. 60ms across 6 rows = 360ms total stagger plus 880ms reveal = 1.24s to fully settle, which is exactly Apple's hero-paint budget.

**Where this ritual currently lives in v3.css:**

| Surface | Current code | Status |
|---|---|---|
| `[data-stagger]` (8 hero elements) | `transform: translateY(18px); animation: hero3-rise var(--d-glide) var(--ease-glide) forwards` (lines 195–203) + bug at index 5 (`540ms` repeats `540ms`) | **Refactor:** rename keyframe to `almobadir-reveal`, distance 18→12px, duration `--d-glide`→`--d-cinematic`, replace 8 hand-typed `animation-delay` values with `--reveal-i` × 60ms ladder. **Fixes the index-5 typo as a side effect.** |
| `.cnt3-card` (3 article cards) | `transform: translateY(28px); transition: transform 0.55s ...; .is-in { transform: none; }` + 3 hand-typed delays (50/180/310ms) (lines 615–620) | **Refactor:** distance 28→12, duration 0.55s→`--d-cinematic`, delays → `--reveal-i: 0/1/2`. |
| `.nw3a-card` (7 network cards) | `transform: translateY(28px); transition: opacity .9s ..., transform .9s ...; .nw3a-in { transform: translateY(0); }` (lines 344–345) | **Refactor:** distance 28→12, duration .9s→`--d-cinematic`, add `--reveal-i` per card index. |
| `.fnd3a__name-block, .fnd3a__bio, .fnd3a__quote, .fnd3a__stats, .fnd3a__authority, .fnd3a__follow-card, .fnd3a__auth-card` (founder block, 7 elements) | `transform: translateY(18px); transition: opacity 0.9s ..., transform 0.9s ...; .is-inview .X { transform: none; }` + per-child hand-typed delays (lines 858–871) | **Refactor:** distance 18→12, duration 0.9s→`--d-cinematic`, replace 11 hand-typed delays with index ladder. |
| `.mtd3__frame .mtd3__kicker/h/body/meta/art` (5 elements per frame × 5 frames = 25) | `transform: translateY(20px); transition: opacity .8s ..., transform .8s ...` + hand-typed delays 0.18/0.32/0.46/0.6s (lines 441–446) | **Refactor:** distance 20→12, duration .8s→`--d-cinematic`, delays → `--reveal-i` ladder. |
| `.ftr3-col` (5 footer columns) | IO-driven via JS, current delays unspecified (audit notes JS overhead) | **Refactor:** delete JS observer, use shared `[data-reveal]` selector; add `--reveal-i` to existing `data-stagger` attributes. |

**That's 6 surfaces, ≥51 elements collapsing to one CSS class.**

**Where it should additionally appear:**

7. `.manif3__shadow` (currently `transform: translateY(12px); transition: opacity .9s ..., transform .9s ... .35s` — already on-grid, just needs renaming to `[data-reveal]`).
8. `.nl3-pitch, .nl3-frame` (newsletter section — currently no entrance, just sits there).
9. `.fnd3a__auth-rule, .fnd3a__auth-stamp` (currently no entrance).
10. `.hdr3-mega-card` (mega-menu on first open per session — currently snap-in; should rise+fade with 40ms internal stagger).

**prefers-reduced-motion fallback:**

```css
@media (prefers-reduced-motion: reduce) {
  [data-reveal],
  [data-reveal][data-in-view] {
    animation: none !important;
    opacity: 1 !important;
    transform: none !important;
    transition: none !important;
  }
}
```

Already covered by the `*` blanket in base.css; the explicit rule above is what replaces the brittle 14-line selector list at v3.css:1199–1210. Drop the list.

---

### Ritual 2 — The Almobadir Hover

**The single hover behavior every interactive surface shares.** Lift 2px, brighten the border to the brand-strong line, glide both for `--d-instant`. Two-step asymmetric: hover-in on `--ease-glide`, hover-out on `--ease-out` (faster perceived recovery — the bench pattern).

**Pattern (CSS):**

```css
/* Apply via class .hover-lift OR via :where() group selector below.
   Three properties only: transform, border-color, box-shadow.
   No background swap (background-flicker on hover is forbidden — see §3). */

.hover-lift,
:where(.hdr3-card, .hdr3-cta, .hdr3-mega-card,
       .nw3a-card, .cnt3-card,
       .fnd3a__follow-card, .fnd3a__auth-card,
       .nl3-issue, .ftr3-cta__btn, .ftr3-mailbtn) {
  transition: transform var(--d-instant) var(--ease-glide),
              border-color var(--d-instant) var(--ease-glide),
              box-shadow  var(--d-instant) var(--ease-glide);
}

.hover-lift:hover,
.hover-lift:focus-visible,
:where(/* same list */):hover,
:where(/* same list */):focus-visible {
  transform: translateY(-2px);
  border-color: var(--hairline-strong);
  box-shadow: var(--e-2);
}

/* Asymmetric exit — faster perceived recovery (Stripe/Linear pattern) */
.hover-lift,
:where(/* same list */) {
  transition: transform var(--d-instant) var(--ease-out),
              border-color var(--d-instant) var(--ease-out),
              box-shadow  var(--d-instant) var(--ease-out);
}
.hover-lift:hover, /* override on :hover for the in-state curve */
:where(/* same list */):hover {
  transition: transform var(--d-instant) var(--ease-glide),
              border-color var(--d-instant) var(--ease-glide),
              box-shadow  var(--d-instant) var(--ease-glide);
}

/* :active — applies everywhere on every hoverable, no exceptions */
.hover-lift:active,
:where(/* same list */):active {
  transform: translateY(0) scale(0.98);
  transition-duration: 80ms;
  transition-timing-function: var(--ease-snap);
}

/* :focus-visible — global brand-on ring (audit gap: 1 of 80) */
:focus-visible {
  outline: 2px solid var(--crimson);
  outline-offset: 4px;
  border-radius: inherit; /* matches the host's curvature */
}
```

**Tokens consumed:** `--d-instant`, `--ease-glide` (in), `--ease-out` (out), `--ease-snap` (active), `--e-2`, `--hairline-strong`, `--crimson`.

**Why -2px, not -1 or -4.** The audit lists 5 different lift values (`-1px header, -2px CTA, -3px follow-card, no-lift content, no-lift cnt3-card`). 2px is the centroid Linear/Vercel/Stripe converge on — the smallest distance the eye reads as "rising," the largest before it reads as "popping." Below 2px feels like nothing happened; above 3px on a card grid creates jitter as the page reflows the grid's apparent baseline.

**Why `--d-instant` (140ms) for hover, not `--d-base` (320ms).** Audit found 5 card-hover durations in the wild (`.24s, .65s, .55s, .5s, 280ms`). Of those, 280ms (newsletter issue) and 240ms (header card) are the right neighborhood. 320ms feels heavy on hover — by the time the lift completes, the user's cursor has moved on. Notion's pattern (REF 14): high-frequency interactions should feel near-instant. 140ms is the bench floor.

**Why asymmetric in/out.** When the user hovers in, they're committing attention; the curve can be a slow-in, fast-end glide. When they hover out, they're disengaging; the curve should clear quickly so the next hover doesn't fight residual motion. `--ease-out` (expo-out) on exit feels ~30% faster than the same duration on `--ease-glide`. This is the Stripe/Linear nuance the audit flagged as missing.

**Why `:active { scale(0.98) }` everywhere.** Audit's strongest finding in §10: zero `:active` states sitewide. Touch users get no feedback. Apple, Linear, Stripe all run scale(0.96–0.98) on active. 80ms keeps it feeling like a button press, not an animation.

**Where this ritual currently lives in v3.css (and the 7 different durations it needs to collapse):**

| Surface | Current duration | Current curve | Status |
|---|---|---|---|
| `.hdr3-card` | 0.24s | `var(--ease-glide)` (✓) | Keep curve, retune duration to `--d-instant`. |
| `.hdr3-cta` | 0.32s | `var(--ease-glide)` | Retune to `--d-instant`. |
| `.nw3a-card` | 0.65s | `var(--ease-glide)` + `0.4s ease + 0.5s ease` (audit §3) | Retune duration; replace `ease` keywords with `var(--ease-glide)`. |
| `.cnt3-card` | 0.55s | `var(--ease-glide)` (✓) | Retune duration; collapse to ritual. |
| `.fnd3a__follow-card` | 0.5s | `var(--ease-glide)` + `0.4s ease` | Retune; replace `ease`. |
| `.fnd3a__auth-card` | unspecified, line 761 uses `0.32s ease` | `ease` (audit §3) | Replace `ease` with `var(--ease-glide)`; add transform lift. |
| `.nl3-issue` | 280ms | `var(--ease-glide)` (✓) | Retune to `--d-instant`. |
| `.ftr3-cta__btn` | 0.3s | `var(--ease-glide)` (✓) | Retune. |
| `.ftr3-mailbtn` | unspecified | (default browser) | Add ritual binding. |

**That's 9 surfaces consolidating to one rule.**

**Additional surfaces that should be on the ritual but currently aren't:**

10. `.hero3__chip` (currently `transition: transform var(--d-base) var(--ease-glide), border-color..., color...`) — already 90% there, just needs `--d-instant` and the `:active` state.
11. `.hero3__panel-link, .hero3__signup-cta` — currently `--d-base`, needs `--d-instant`.
12. `.hdr3-nav-link` — uses `0.2s` color + `0.32s` underline scale; collapse.
13. `.hdr3-mega-all` — uses `0.2s` `var(--ease-glide)`; on-system, just rename for tracking.
14. `.mtd3__cta` — uses `.35s` `var(--ease-glide)`; retune.
15. `.fnd3a__follow-arrow` — uses `0.4s ease`; replace `ease`, retune.
16. `.ftr3-list a` — currently no transition on the lift; add ritual binding.

**Six new surfaces brought into the ritual.**

**prefers-reduced-motion fallback:**

```css
@media (prefers-reduced-motion: reduce) {
  .hover-lift, .hover-lift:hover, .hover-lift:active,
  :where(/* full list */):hover, :where(/* full list */):active {
    transform: none !important;
    transition: border-color 0.01ms, box-shadow 0.01ms !important;
  }
  /* Hover state still flips border + shadow; the lift is what's removed. */
}
```

Important: **reduced-motion users still get the hover acknowledgment** — border and shadow change instantly. Removing all hover feedback breaks affordance. Only the kinetic part is suppressed.

---

### Ritual 3 — The Almobadir Transition

**The single between-section beat.** As the user crosses from one section to the next, the *outgoing* section's content drifts up and dims to 0.6 opacity over `--d-cinematic`, while the *incoming* section's reveal ritual fires. No parallax (rejected — see §3). No pinned card stack between every section (only one section pins per page). The signature is restraint: most section transitions on Almobadir are *just scroll*, with one or two cinematic moments per page where the brand turns up the volume.

**Pattern (CSS — pure scroll-driven where supported, IO fallback otherwise):**

```css
/* Section dim-as-you-leave — scroll-driven (REF 3, REF 12) */
@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    .section-glide {
      animation: almobadir-section-glide linear both;
      animation-timeline: view();
      animation-range: exit 0% exit 100%;
    }
    @keyframes almobadir-section-glide {
      from { opacity: 1; transform: translateY(0); }
      to   { opacity: 0.6; transform: translateY(-24px); }
    }
  }
}

/* The 1 cinematic between-section moment per page: hero → manifesto.
   The hero card pushes back in Z while the manifesto rises forward. */
@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    .hero3__panel {
      animation: almobadir-handoff linear both;
      animation-timeline: view();
      animation-range: exit -20% exit 100%;
    }
    @keyframes almobadir-handoff {
      from { transform: none; opacity: 1; }
      to   { transform: perspective(1200px) translateZ(-160px); opacity: 0.4; }
    }
  }
}
```

**Tokens consumed:** `--d-cinematic` (for IO fallback), `--ease-glide`. Pure scroll-driven: no duration token (timeline IS the duration).

**Why one handoff moment per page, not eight.** REF 17 (Lusion) demonstrates the dolly *as a moment*. If every section transition pushes a handoff, the page becomes a theme-park ride. Almobadir is editorial — restraint is the brand. **One** handoff (hero → manifesto) signals "you're entering the heart of the page" and earns its weight.

**Why exit-on-leave at 0.6 opacity, not 0.** Going to fully dark on exit reads as a slide deck or app navigation, not a continuous editorial scroll. 0.6 + a 24px upward drift gives the user the sense the previous section is "still there, just quieter," which matches how the brand treats its own past work.

**Where this ritual currently lives in v3.css:**

The honest answer: it doesn't. The current site has zero between-section transition logic. Sections cut hard. The only proto-handoff is the mtd3 scroll-pin choreography, which is a *within-section* primitive (smoothstep sub-progress text reveal) and stays where it is.

**Where it should appear (5+ surfaces minimum):**

1. **Hero → Manifesto** — the cinematic handoff (Z dolly).
2. **Manifesto → Network** — section-glide (dim+drift).
3. **Network → Method** — section-glide.
4. **Method → Content** — section-glide.
5. **Content → Newsletter** — section-glide.
6. **Newsletter → Founder** — section-glide.
7. **Founder → Footer** — section-glide.

**Apply by adding `.section-glide` to each `<section>` element except the cinematic handoff section (hero-panel itself, which uses `.almobadir-handoff` mechanics).** Six sections gain a class; one section gains a Z dolly. Total new code: ~30 lines of CSS.

**prefers-reduced-motion fallback:**

```css
@media (prefers-reduced-motion: reduce) {
  .section-glide, .hero3__panel { animation: none !important; }
}
```

Already inside the `@supports + @media` guards above — fallback is automatic. No animation-timeline support? No section-glide. Reduced-motion? No section-glide. Section still scrolls; user still sees content; nothing breaks.

---

### Ritual 4 — The Almobadir Counter

**Every number on the page ramps with the same easing and timing.** No exceptions.

**Pattern (JS, single shared module — replace v3.js:409–428 + v3.js founder code):**

```js
// One counter, used by every counting element.
// Tokens read from CSS so designer-controlled values stay in tokens.css.
const COUNTER_DURATION = 1400;        // matches --d-cinematic + 60% (extended for legibility)
const COUNTER_DESYNC   = 400;         // ±200ms randomized jitter when ≥2 counters in same view
const easeOutCubic     = (t) => 1 - Math.pow(1 - t, 3);  // mirrors --ease-counter

function almobadirCount(el, opts = {}) {
  const target  = parseFloat(el.dataset.target || el.textContent || '0');
  const suffix  = el.dataset.suffix || '';
  const format  = el.dataset.format || '';        // 'comma' | 'decimal' | ''
  const reduce  = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  if (reduce) { el.textContent = formatNum(target, format) + suffix; return; }
  const dur     = COUNTER_DURATION + (Math.random() - 0.5) * COUNTER_DESYNC;
  const start   = performance.now();
  const tick = (now) => {
    const t = Math.min(1, (now - start) / dur);
    const v = target * easeOutCubic(t);
    el.textContent = formatNum(v, format, target) + suffix;
    if (t < 1) requestAnimationFrame(tick);
    else el.textContent = formatNum(target, format) + suffix;
  };
  requestAnimationFrame(tick);
}

function formatNum(v, format, target) {
  if (format === 'comma')   return Math.round(v).toLocaleString('en-US');
  if (format === 'decimal') return v.toFixed(1);
  if (target && target < 100) return v.toFixed(0);
  return Math.round(v).toString();
}

// One IntersectionObserver fires every counter on first viewport entry.
const counterIO = new IntersectionObserver((entries, obs) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.querySelectorAll('[data-counter]').forEach(almobadirCount);
      obs.unobserve(e.target);
    }
  });
}, { threshold: 0.18 });

document.querySelectorAll('[data-counter-root]').forEach(r => counterIO.observe(r));
```

**HTML contract — every counter on the page uses this exact attribute set:**

```html
<span data-counter
      data-target="478"
      data-suffix="K"
      data-format=""></span>
```

**Tokens consumed:** `--ease-counter` (CSS — for any counter that animates via CSS instead of JS), `--d-cinematic` (the 880ms base that the 1400ms duration extends from for legibility — see rationale).

**Why 1400ms ± 200ms desync, not the same 880ms `--d-cinematic` budget.** Numbers need *time to be read*. A counter ramping in 880ms blurs through the digits — by the time the eye locks onto the running figure, the animation is over. Founder section's existing 1400–1800ms is the bench-correct band (Stripe Atlas: 1500ms, Linear: 1400ms). The ±200ms desync is the existing v3.js pattern (`1400 + Math.random() * 400`) — keep it; it's the one piece of the founder counter that's already bench-grade and the audit calls out as feeling "human." The shared module preserves it.

**Why the JS easeOutCubic and the CSS `--ease-counter` token must match.** Today the CSS token is declared but unused; the JS hard-codes `1 - Math.pow(1-t, 3)`. The token's bezier `(.215, .61, .355, 1)` is the analytical-form match for easeOutCubic. Future migration to WAAPI (which accepts CSS bezier strings) is a one-liner.

**Where this ritual currently lives:**

| Surface | Current code | Status |
|---|---|---|
| `.fnd3a__count` (3 founder stats) | v3.js:409–428 — RAF + cubic ease, randomized 1400–1800ms | **Already on-ritual.** Move logic to shared `almobadirCount`, keep behavior. |
| `.nw3a-count-fig` (7 network stat blocks) | static text in HTML | **Add `data-counter` + targets, register on `data-counter-root`.** Trigger when `.nw3a-section` enters view. |
| Hero panel KPI counters (if any added per REF 13 hero convergence vision) | n/a | Add as numbers appear. |

**Where it should additionally appear:**

4. Newsletter subscriber count (currently static in `.nl3-pitch`).
5. Footer "Riyadh time" digit changes — *exception:* this is a clock, not a counter; it ticks 1Hz and stays as-is. Documented so future devs don't try to ramp it.
6. Method scroll progress percentage (currently no counter readout).
7. Any future "X published pieces" / "Y countries" stat the studio wants to surface.

**prefers-reduced-motion fallback:**

The JS already short-circuits to `el.textContent = formatNum(target, format) + suffix` when `reduceMotion` is true. **No animation, but the final value still appears** — that's the spec-correct behavior (numbers must be readable). Verify by toggling System Settings → Accessibility → Reduce Motion and reloading.

---

## 3. The negative space — what Almobadir motion REJECTS

This list is enforceable. PR comments and design reviews can cite a row.

| Pattern | Why rejected | Citation |
|---|---|---|
| **Bouncy spring on body text or paragraphs** | Springs on text caricature the editorial gravitas. Reading copy that overshoots feels juvenile — Notion, Stripe, NYT all use linear/glide on text reveals. The audit found `manif3__word` reveals using `--ease-glide` (correct); guard that this never gets "upgraded" to `--ease-spring`. | motion REF 11 (Things 3 spring is reserved for *cards*, never text) |
| **Page-driven parallax (multi-layer translateY at differing rates)** | Reads as 2018 portfolio. Parallax is associated with "I'm a designer who learned ScrollMagic." Almobadir is editorial; depth comes from typographic hierarchy and brand color, not from layers sliding at 0.3/0.6/0.9. | motion REF 12 evaluated and *not* included in top-8 portable; explicitly excluded here |
| **Glitch / VHS-distortion / RGB-split filters** | Aesthetically opposed to the brand. Almobadir's positioning is "editorial trust," and glitch effects code as "tech-bro / cyberpunk." If a future design references Cassie Evans or Codrops glitch demos, redirect to text-scramble (REF 15) which is the *one* lo-fi-tech moment the brand permits, and only on the hero h1 once per session. | wow-research-motion §REF 7 (Igloo Inc — *don't ship this*) |
| **Swipe-reveal or curtain-wipe section transitions** | Hard cuts disguised as transitions. Reads as PowerPoint or 2019 React-spring tutorials. The brand has one section transition: section-glide (Ritual 3). | excluded by design |
| **Persistent infinite-loop ornaments (orbit spinners, rotating verifieds, halo rings)** | Audit §1 lists 6 always-on infinite ornaments (manifesto orbit, founder verified spin ×2, hero halo, hero pulse-ring, hero hue-drift past 5s). These are decoration without a job. Brand decision: motion serves a function or it doesn't run. | audit §1 + §8 — already flagged for removal |
| **Animation on `width`, `height`, `min-width`, `backdrop-filter`** | Layout-property animation forces reflow each frame; backdrop-filter rerenders the blur radius. Always re-express as `transform: scaleX/scaleY` or accept the property change as instant. | audit §4 — 8 transitions to convert |
| **Asymmetric durations (different in/out times on the same property)** | Audit's `nl3-submit` has `transform 280ms` + `min-width 480ms` on the same hover; 480ms outlives the user's awareness of why anything moved. Hover in and hover out are the same length. (Curves can be asymmetric; durations cannot.) | audit §2 finding 5 |
| **Hover transitions ≥ `--d-base` (320ms)** | Hover is a high-frequency interaction (Notion REF 14). Anything over `--d-instant` (140ms) feels like the page is "loading" the hover. The forbidden middle (320–880ms) is *especially* forbidden on hover. | this doc §1.1 |
| **Custom JS scroll engines (Locomotive, GSAP ScrollSmoother, Pjax)** | Lenis is permitted (motion REF 9, 3kb, doesn't break sticky/100vh). Anything heavier breaks the brand's load budget. If Lenis isn't enough, the answer is "use less motion," not "use a bigger library." | wow-research-motion §REF 9 |
| **`ease-in` curves (slow start, fast end)** | Things ease into the viewport; the brand has no surface that *exits* the page in the user's primary attention. The `--ease-in` token is deleted from tokens.css. | audit §3 — declared, never used; this doc §1.2 |
| **Bouncing scroll cues, jiggling chevrons, tilting attention-grabbers (>2°)** | The bouncing arrow at mtd3:421 (`mtd3-bounce` 6px translateY) is the *one* allowed jiggle, because it's a navigation affordance with semantic meaning. Anything else (a card that tilts to draw attention, a button that bounces every 4s) is rejected. | audit §1 — keeping mtd3-bounce, rejecting future copies |
| **3D card tilt on touch devices** | Audit confirms current code gates tilt with `matchMedia('(hover: none)').matches → return`. **This is the right pattern.** Mobile/tablet users get the static card. Don't try to "translate" tilt to gyroscope events; it always feels broken. | v3.js:155–171 — already correct, document for posterity |
| **Counters that animate on every viewport re-entry** | Once per session per element. The IO `obs.unobserve` after first fire is mandatory. Re-counting on scroll-back makes the page feel busy and reactive instead of authored. | wow-research-interaction §REF 1 + this doc §2.4 |
| **Cursor trails, pixel auras, sparkle particles around the cursor** | The custom-cursor pattern (motion REF — Cobe / interaction REF 5) is permitted but currently scoped out of this audit (interaction signature handles it). Anything more than cursor + lerp + label-on-hover (e.g., trailing 6 dots, sparkle emit) is rejected. | wow-research-interaction §REF 5 — bounded |

**Citation rule:** If a future PR adds motion that violates any row above, the review comment is a link to that row. No debate. Restraint is the brand.

---

## 4. Migration plan — retiring 27 distinct durations + 8 layout-thrash + 6 always-on ornaments

Three sprints. Each ships independently, none depends on another. Total estimate: 1 day of dev time + 0.5 day of QA across breakpoints.

### Sprint 1 — Token foundation (4 hours)

**1.1 Update tokens.css** (15 min):

```css
/* REPLACE the existing easing + duration block (lines 147–158) with: */

  /* ── Easing ──────────────────────────────────────── */
  --ease-glide:   cubic-bezier(0.28, 0.11, 0.32, 1);
  --ease-out:     cubic-bezier(0.22, 1, 0.36, 1);
  --ease-spring:  cubic-bezier(0.34, 1.56, 0.64, 1);
  --ease-snap:    cubic-bezier(0.4, 0, 0.2, 1);
  --ease-counter: cubic-bezier(0.215, 0.61, 0.355, 1);

  /* ── Durations · 3 tiers ─────────────────────────── */
  --d-instant:   140ms;
  --d-base:      320ms;
  --d-cinematic: 880ms;
```

Delete `--ease-in`. Delete `--d-fast`, `--d-slow`, `--d-glide`. (Anything still referencing these will break loudly — that's the point; they're the things that need migration.)

**1.2 Find & replace pass on v3.css** (90 min):

Mass replace using IDE multi-cursor or `sd` / `sed`:

```
0.12s → var(--d-instant)
0.14s → var(--d-instant)
0.15s → var(--d-instant)
0.2s  → var(--d-instant)
0.24s → var(--d-instant)   [card hovers]
0.25s → var(--d-instant)
0.28s → var(--d-instant)   [previously 280ms]
280ms → var(--d-instant)

0.3s  → var(--d-base)
0.32s → var(--d-base)
0.35s → var(--d-base)
0.4s  → var(--d-base)
320ms → var(--d-base)

0.45s → var(--d-base)      [if hover/state] OR var(--d-cinematic) [if reveal]
0.48s → var(--d-base)      [if hover/state] OR delete (forbidden middle)
0.5s  → var(--d-cinematic) [if reveal]      OR var(--d-base) [if hover]
0.55s → var(--d-cinematic)
0.6s  → var(--d-cinematic)
0.65s → var(--d-cinematic) [card hover OUT only — verify]
0.7s  → var(--d-cinematic)
0.8s  → var(--d-cinematic)
0.9s  → var(--d-cinematic)
1.0s  → var(--d-cinematic)
1.1s  → var(--d-cinematic)
1.2s  → var(--d-cinematic) [if image reveal] — drop to --d-base if too cinematic
1.4s  → var(--d-cinematic) [manif3 underline path — verify visually]
1.6s  → var(--d-cinematic) [founder photo zoom — audit #21 already flagged]
```

After find/replace, **manually visit every changed line** — about 85 lines — and confirm the mapping. Spreadsheet-style decision: is this hover/state (→ `--d-base`)? Is this reveal/entrance (→ `--d-cinematic`)? Is this micro-feedback (→ `--d-instant`)?

**1.3 Replace 8 hardcoded `ease` keywords** (audit §3 table) (15 min):

```
v3.css:294 .nw3a-card                  ease → var(--ease-glide)
v3.css:320 .nw3a-cover img             ease → var(--ease-glide)
v3.css:761 .fnd3a__auth-card           ease → var(--ease-glide)
v3.css:770 .fnd3a__auth-shine          ease → var(--ease-glide)
v3.css:778 .fnd3a__follow-card         ease → var(--ease-glide)
v3.css:779 .fnd3a__follow-card::before ease → var(--ease-glide)
v3.css:790 .fnd3a__follow-arrow        ease → var(--ease-glide)
v3.css:283 .nw3a-eyebrow-dot @keyframe ease-in-out → var(--ease-snap)
```

**1.4 Add the `:focus-visible` global** (5 min):

```css
/* base.css — append once */
:focus-visible {
  outline: 2px solid var(--crimson);
  outline-offset: 4px;
  border-radius: inherit;
}
:focus:not(:focus-visible) { outline: 0; }   /* mouse users get no ring */
```

**1.5 Add the global `:active` rule** (5 min):

```css
/* base.css — append once */
:where(button, a, [role="button"]):active {
  transform: translateY(0) scale(0.98);
  transition: transform 80ms var(--ease-snap);
}
```

**1.6 Drop the v3.css selector-specific reduced-motion list** (5 min):

Delete v3.css lines 1199–1210. The global `*` rule in base.css covers everything; the specific list is maintenance debt.

**Sprint 1 verification:**
- Visit hero, hover every chip and CTA — should feel ~30% snappier.
- Tab through every interactive element — every one should now show a crimson outline.
- Press-and-hold any CTA on touch — should see the scale(0.98) press feedback.
- Toggle System Reduce Motion → reload → confirm nothing animates but everything is readable.

### Sprint 2 — The four rituals (4 hours)

**2.1 Add the three ritual classes to v3.css**:

Append the CSS from §2 (Reveal, Hover, Transition, Counter) above. ~120 lines total.

**2.2 Migrate existing reveals to `[data-reveal]`** (90 min):

Find every selector currently doing the rise-and-fade pattern (`.cnt3-card`, `.nw3a-card`, `.fnd3a__name-block` etc.) and:
- Add `data-reveal` attribute in HTML.
- Add `style="--reveal-i: N"` where the stagger applies (or use a CSS attribute selector for known indexes).
- Delete the per-selector `transform: translateY(...); transition: ...` opacity block.
- Delete the per-selector `.is-X { transform: none; }` reveal completion block.

**The IO observer** in v3.js needs a one-line addition:

```js
// Replace any per-section IO with this single one.
const revealIO = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.setAttribute('data-in-view', '');
      revealIO.unobserve(e.target);
    }
  });
}, { threshold: 0.18, rootMargin: '0px 0px -10% 0px' });
document.querySelectorAll('[data-reveal]').forEach(el => revealIO.observe(el));
```

**2.3 Migrate counters to shared `almobadirCount` module** (45 min):

Move the founder counter logic from v3.js:409–428 to a top-level helper. Add `data-counter` + targets to the 7 `.nw3a-count-fig` spans in HTML. Wire the network section's `data-counter-root` attribute.

**2.4 Add `.section-glide` to 6 of 7 `<section>` elements** (15 min):

```html
<section class="manif3 section-glide">…
<section class="nw3a section-glide">…
<section class="mtd3">…   <!-- mtd3 has its own pin choreography; no .section-glide -->
<section class="cnt3 section-glide">…
<section class="nl3 section-glide">…
<section class="fnd3a section-glide">…
```

The hero `.hero3__panel` gets `.almobadir-handoff` instead.

**Sprint 2 verification:**
- Scroll the page once top to bottom. Every section's content should rise+fade in (or, on scroll-driven-supporting Chromium, progress in with scroll).
- Hover every `[data-reveal]` element after first reveal — entry should not re-trigger.
- Reload mid-scroll — sections above the viewport should already be revealed; sections below should reveal as they cross.
- Counters: the network section's 7 numbers should now ramp once when first visible.

### Sprint 3 — Removing dead motion (90 min)

**3.1 Remove 6 always-on infinite ornaments** (audit §1 verdicts):

```css
/* DELETE these animation declarations: */
.hero3__pulse-ring         { animation: hero3-pulse ...    }   →  remove
.hero3__panel-halo         { animation: hero3-halo-spin... }   →  remove
.manif3__ornament svg      { animation: manif3-spin ...    }   →  remove
.fnd3a__verified           { animation: fnd3aSpin ...      }   →  remove
.fnd3a__verified-tick      { animation: fnd3aSpin ...      }   →  remove
.nl3-issues-row            { animation: nl3-marquee ...    }   →  remove (replace markup with static row)
```

Then delete the 6 `@keyframes` definitions. Net: -50 LOC.

**3.2 Pause inactive-frame mtd3 animations** (1 line):

Append after v3.css line 521:

```css
.mtd3__frame:not(.is-active) *,
.mtd3__frame:not(.in-view) * {
  animation-play-state: paused !important;
}
```

This single rule retires 12 of the audit's "PAUSE OFFSCREEN" findings.

**3.3 Reduce hero hue-drift to 18s + pause when offscreen** (5 min):

```css
@keyframes hero3-hue-drift { /* unchanged ramp */ }
.hero3 { animation: hero3-hue-drift 18s linear infinite; }   /* was 60s */

/* Pause when section out of view — IO toggle */
.hero3.is-offscreen { animation-play-state: paused; }
```

JS: 5-line IO observer toggles `.is-offscreen`.

**3.4 Reduce hero ticker to 28s + pause on focus-within** (2 min):

```css
.hero3__ticker-track { animation-duration: 28s; }   /* was 50s */
.hero3__ticker:focus-within .hero3__ticker-track { animation-play-state: paused; }
```

**3.5 Remove `transition: backdrop-filter` from `.hdr3`** (line 18, 1 char delete).

**3.6 Replace 8 layout-thrash transitions** (audit §4 table) (30 min):

```
.hdr3-search        width 0.32s  → transform: scaleX with origin: right  (or fix at 260px)
.cnt3-accent-bar    width 0.5s   → transform: scaleX with origin: left   (already partially scaleX'd; finish)
.nl3-submit         min-width 480ms → fixed wider min-width + label/success swap inside
.hdr3-announce      backdrop-filter 0.32s → drop from transition list
```

**Sprint 3 verification:**
- Open the homepage. The verified spin, manifesto orbit, hero halo+pulse-ring, newsletter marquee should all be GONE.
- Scroll to method, then to founder. The method section's frame 1 SVG decorations should have stopped (open DevTools → Elements → mtd3 frame 1 → confirm `animation-play-state: paused`).
- Resize window during a hover-search — should not reflow on every keypress.

### Migration smoke test (after all sprints)

- DevTools Performance tab: record 5 seconds of idle on the homepage. Idle CPU should drop ≥40% vs. pre-migration.
- Lighthouse: animation-related "avoid non-composited animations" audit should pass.
- Reduce-motion toggle: every reveal, every hover, every counter should fall back as documented.
- RTL: switch the page to `dir="rtl"` (it already is) and confirm — *we never wrote any motion that depends on horizontal direction except the section-glide vertical drift*. Should be fully RTL-safe.
- Visit on a Windows mouse-wheel laptop. Now is the right time to also ship Lenis (motion REF 9, 3kb) — separate PR, but the motion system is now compatible.

---

## 5. The signature, in one paragraph (200 words)

Almobadir motion is governed by **3 durations, 5 curves, 4 rituals**. Durations: `--d-instant` (140ms · hover · focus · menu), `--d-base` (320ms · state · card lift), `--d-cinematic` (880ms · entrance · scroll-driven). The forbidden middle — anything between 320ms and 880ms — is rejected by definition; this is the discipline that retires the audit-flagged 27 distinct durations. Curves: `--ease-glide` (Apple's signature, the default), `--ease-out` (expo-out, exits only), `--ease-spring` (Things' overshoot, five named moments only), `--ease-snap` (Material's standard, symmetric state), `--ease-counter` (easeOutCubic, numbers only). Rituals: every animatable element belongs to one. **The Almobadir Reveal** is the single rise-12px-fade-in entrance with optional 60ms-stagger. **The Almobadir Hover** is the single -2px lift with asymmetric in/out curves and a global `:focus-visible` ring + `:active` press. **The Almobadir Transition** is the single between-section dim-and-drift, with one cinematic Z-dolly handoff between hero and manifesto. **The Almobadir Counter** is the single easeOutCubic ramp, 1400ms ± 200ms desync, once per session. Everything else — bouncy text, parallax, glitch, swipe-reveal, persistent ornaments, layout-property transitions, hover ≥ `--d-base` — is rejected with a citation. Restraint is the brand.
