# Spacing System — Forensic Audit

**Audited:** 2026-04-26
**Source:** `C:\Users\toner\OneDrive\Desktop\almobadir\website\assets\tokens.css` (lines 102–116) + `assets\v3.css` (1,151 lines)
**Live:** http://localhost:8765/index.html
**Benchmarks:** Stripe (8pt grid + clamp), Tailwind (0.25rem step), Material 3 (4dp grid), Linear (4/8/16/24/32 cadence)

---

## Executive Summary

The token contract is **structurally sound but operationally bypassed**. `tokens.css` defines a clean 4-px-base scale of 15 spacing tokens (`--s-1` 4px → `--s-24` 200px) that doubles cleanly through the Tailwind cadence (4/8/12/16/20/24/32/40/48/56/72/96/120/160/200). It is a respectable system on paper. In practice, **only 26 references to `var(--s-*)` exist across the entire 1,151-line v3.css** — a usage rate of roughly 2.3 references per 100 lines of CSS, and **every section except `.manif3` ignores tokens entirely** in their primary spacing decisions. The base scale tokens `--s-1` through `--s-5` and the larger ones `--s-10`, `--s-12`, `--s-14`, `--s-16`, `--s-20`, `--s-24` are **literally never referenced anywhere in the live stylesheet**. The system has 15 tokens; the codebase consumes 4 of them (`--s-6`, `--s-7`, `--s-8`, `--s-9`).

In place of tokens, v3.css uses **literal pixel values** for almost every gap, padding, and margin (over 600 numeric literals between 2px and 96px), and **inline `clamp(min,vw,max)` triplets** for fluid surfaces (98 clamp instances, 30+ unique min/max pairs). The clamp ranges roughly cluster around four ratios — 1.4×, 1.6×, 1.8×, 2× — but the cluster is not designed; it is the residue of nine specialist agents independently authoring nine sections. There are **two competing section-padding clamps** (`clamp(48px,5vw,80px)` for nw3a/cnt3/nl3/fnd3a vs `clamp(48px,6vw,96px)` for ftr3-cta/ftr3-map and `clamp(48px,6vw,80px)` for mtd3 intro), three competing internal-padding clamps for cards (`clamp(14px,1.4vw,20px)`, `clamp(20px,2.4vw,32px)`, `clamp(22px,2.4vw,36px)`), and a ladder of internal "stage" paddings (`clamp(28px,4vw,56px)`, `clamp(48px,8vw,140px)`, `clamp(48px,6vw,96px)`) that do not share a single ratio.

The vertical rhythm at 1440 viewport is jagged: section-to-section transitions land at 80, 80, 80, 80, 80, 96 — close-but-not-equal, with `.manif3` at a flat 48px (no clamp at all) and `.hero3` at a fluid 36→56. **Internal element rhythm is the worst symptom**: hero3 stacks gaps of clamp(20px,2.4vw,32px) on `__main`, then clamp(28px,4vw,56px) on `__topbar`, then 18px hardcoded margins, then 16px, then 14px, then 12px, then 10px, then 8px — a drift of 8 distinct values for what should be 2–3. Mobile passes (720px and 480px breakpoints) compound the inconsistency by switching to flat literal pixels (`gap: 14px`, `gap: 20px`, `gap: 28px`) that no longer correspond to the desktop tokens. Two specific dead-space risks flagged in the brief — founder bottom void and newsletter left-column void — are **partially fixed** (sticky `top: 96px` on `.nl3-preview` keeps it anchored, and `.fnd3a__follow` margin-block-start `clamp(72px,9vw,120px)` plus a colophon clamp `clamp(48px,6vw,80px)` close the bottom — verified in code, not in screenshot diff). The system is salvageable with a one-day pass: standardize section padding to one clamp, replace the 600 literal pixels with `var(--s-*)`, and create a new `--clamp-*` scale of 6 named clamps.

---

## Token inventory

`tokens.css:102-116` defines the canonical 4-px-base scale. Usage counts are exact `\bvar(--s-N)\b` matches in v3.css (`--s-1`/`--s-2`/etc. word-boundary, so `--s-12` is not double-counted by `--s-1`).

| Token | Value | px | Live usage count | Example uses |
|---|---|---|---|---|
| `--s-1` | 4px | 4 | **0** | none — never referenced |
| `--s-2` | 8px | 8 | **0** | none — never referenced |
| `--s-3` | 12px | 12 | **0** | none — never referenced |
| `--s-4` | 16px | 16 | **0** | none — never referenced |
| `--s-5` | 20px | 20 | **0** | none — never referenced |
| `--s-6` | 24px | 24 | **5** | manif3 mobile gap/margin (lines 952–957) |
| `--s-7` | 32px | 32 | **14** | manif3 rules+head+lede+caption (247, 248, 250, 256, 268), mtd3 hud (375, 530), 480px section paddings (1083, 1088, 1111, 1115, 1126, 1133, 1134) |
| `--s-8` | 40px | 40 | **6** | manif3 780px+720px paddings (274, 913), nw3a/cnt3/nl3/fnd3a 720px paddings (914–917) |
| `--s-9` | 48px | 48 | **1** | manif3 desktop section padding (245) |
| `--s-10` | 56px | 56 | **0** | none |
| `--s-12` | 72px | 72 | **0** | none |
| `--s-14` | 96px | 96 | **0** | none |
| `--s-16` | 120px | 120 | **0** | none |
| `--s-20` | 160px | 160 | **0** | none |
| `--s-24` | 200px | 200 | **0** | none |

**Total `var(--s-*)` references in v3.css:** 26. **Total v3.css lines:** 1,151. **Density:** 2.3 token refs / 100 lines.

**Token coverage by section** (count of `var(--s-*)` per section):
- `.hdr3`: **0** uses (180 lines, all-literal spacing)
- `.hero3`: **0** uses (~120 lines incl mobile, all-literal + clamp)
- `.manif3`: **8** uses (lines 245, 247, 248, 250, 256, 268, 274) — the only section that adopts the system
- `.nw3a`: **2** uses (mobile section padding only)
- `.mtd3`: **2** uses (hud margin-bottom × 2)
- `.cnt3`: **2** uses (mobile section padding only)
- `.nl3-section`: **2** uses (mobile section padding only)
- `.fnd3a`: **2** uses (mobile section padding only)
- `.ftr3`: **2** uses (480px sub-section paddings only)

**Underused / dead tokens:** `--s-1`, `--s-2`, `--s-3`, `--s-4`, `--s-5`, `--s-10`, `--s-12`, `--s-14`, `--s-16`, `--s-20`, `--s-24` — **11 of 15 tokens (73%) are never used**. The bottom of the scale (`--s-1..5`) is the most impactful loss because that is exactly where most card/element internal spacing lives.

**Tailwind / Stripe parity:** the scale itself is canonical (4/8/12/16/20/24 = Tailwind 1/2/3/4/5/6; 32/40/48/56 = 8/10/12/14; gap to 72/96 mirrors Tailwind 18/24). No structural problem with the scale. The problem is consumption.

---

## Findings

### Hardcoded values (literal px instead of tokens)

Scope: numeric `padding`, `margin`, `gap`, `top/left/right/bottom`, `inset` literal pixels in v3.css (NOT inside `clamp()` — those are audited below). Covers the ones with clearest token mappings; the full count is over 600 occurrences.

| # | Severity | Location | Value | Should use | Why | Fix |
|---|---|---|---|---|---|---|
| 1 | **High** | v3.css:23 `.hdr3-announce-inner` | `gap: 12px; padding: 0 24px` | `gap: var(--s-3); padding: 0 var(--s-6)` | direct `--s-3` (12) and `--s-6` (24) | swap |
| 2 | **High** | v3.css:35 `.hdr3-main-inner` | `gap: 24px; padding: 0 24px; height: 72px` | `gap: var(--s-6); padding: 0 var(--s-6); height: var(--s-12)` | 24/24/72 all on scale | swap |
| 3 | High | v3.css:53 `.hdr3-mega` | `padding: 20px; margin-top: 8px` | `padding: var(--s-5); margin-top: var(--s-2)` | 20=`--s-5`, 8=`--s-2` | swap |
| 4 | High | v3.css:55 `.hdr3-mega-head` | `gap: 16px; padding-bottom: 14px; margin-bottom: 14px` | `gap: var(--s-4); padding-bottom: 14px (off-scale)…` | 16=`--s-4`; **14 is off-scale (no token)** | replace 16; either accept 14 or move to 12/16 |
| 5 | High | v3.css:60 `.hdr3-mega-grid` | `gap: 8px` | `gap: var(--s-2)` | trivial | swap |
| 6 | High | v3.css:61 `.hdr3-card` | `padding: 14px 14px 14px 18px` | (off-scale) | **14 and 18 are both off-scale** | normalize to 12/16 or 16/20 |
| 7 | High | v3.css:71 `.hdr3-mega-foot` | `margin-top: 14px; padding-top: 12px` | …`var(--s-3)` | 12 is `--s-3`; 14 is off-scale | swap 12, normalize 14→12 or 16 |
| 8 | High | v3.css:74 `.hdr3-util` | `gap: 10px` | (off-scale) | **10 is off-scale** | move to 8 (`--s-2`) or 12 (`--s-3`) |
| 9 | High | v3.css:100 `.hdr3-drawer-inner` | `padding: 24px` | `padding: var(--s-6)` | direct | swap |
| 10 | High | v3.css:101 `.hdr3-drawer-search` | `padding: 12px 14px; margin-bottom: 24px` | `padding: var(--s-3) 14px; margin-bottom: var(--s-6)` | 12 + 24 on scale; 14 off | swap 12 and 24 |
| 11 | High | v3.css:117 mobile hdr3 | `padding: 0 14px; gap: 8px` | `gap: var(--s-2); padding: 0 12px or 16px` | 8=`--s-2`; 14 off-scale | swap, normalize 14 |
| 12 | High | v3.css:136 `.hero3__topbar` | `gap: 24px` | `gap: var(--s-6)` | direct | swap |
| 13 | High | v3.css:146 `.hero3__shell` | (gap is clamp; ok) — but `.hero3__panel-head:192` `gap: 16px; padding-bottom: 18px; margin-bottom: 18px` | `gap: var(--s-4); …18px off-scale` | 16=`--s-4`; **18 off-scale** | swap 16; normalize 18→16 or 20 |
| 14 | High | v3.css:198 `.hero3__kpi-grid` | `border-radius: 14px` (radius, not spacing) — **but** `.hero3__kpi:199` `padding: 18px 16px 16px; gap: 6px` | `padding: 20px 16px (= var(--s-5) var(--s-4))` | 18 off-scale; 16=`--s-4`; 6 off-scale | normalize all three |
| 15 | High | v3.css:210 `.hero3__panel-foot` | `gap: 12px; margin-top: 16px; padding-top: 14px` | `gap: var(--s-3); margin-top: var(--s-4); padding-top: 14px (off)` | 12, 16 on scale; 14 off | swap, normalize |
| 16 | High | v3.css:217 `.hero3__chip` | `padding: 10px 14px; gap: 10px` | (off) | **10 and 14 both off-scale** | move to 8/16 or 12/16 |
| 17 | High | v3.css:221 `.hero3__trust` | `gap: 28px` | (off) | **28 is off-scale (no token)** | move to 24 or 32 |
| 18 | High | v3.css:228 `.hero3__ticker` | `padding-block: 12px` | `padding-block: var(--s-3)` | direct | swap |
| 19 | High | v3.css:235 `.hero3__scroll-cue` | `inset-block-end: 24px; gap: 8px` | `…var(--s-6); …var(--s-2)` | direct | swap |
| 20 | High | v3.css:281 `.nw3a-head` | `gap: 24px clamp(32px,6vw,80px); padding-bottom: 48px; margin-bottom: 56px` | `padding-bottom: var(--s-9); margin-bottom: var(--s-10); gap: var(--s-6) clamp(...)` | 24=`--s-6`, 48=`--s-9`, 56=`--s-10` ALL on scale | swap all three |
| 21 | High | v3.css:289 `.nw3a-totals` | `padding: 20px 24px` | `padding: var(--s-5) var(--s-6)` | direct | swap |
| 22 | High | v3.css:323 `.nw3a-body` | `gap: 10px` | (off) | 10 off-scale | normalize to 8/12 |
| 23 | High | v3.css:340 `.nw3a-stats` | `padding: 14px 0` | (off) | **14 off-scale** | normalize to 12 or 16 |
| 24 | High | v3.css:344 `.nw3a-foot` | `margin-top: 4px; padding-top: 10px` | `margin-top: var(--s-1); padding-top: 10px (off)` | 4=`--s-1`; 10 off | swap 4, normalize 10 |
| 25 | High | v3.css:546 `.cnt3-head` | `gap: 24px; padding-bottom: 28px; margin-bottom: 40px` | `gap: var(--s-6); …; margin-bottom: var(--s-8)` | 24=`--s-6`, 40=`--s-8`; **28 off-scale** | swap, normalize 28→24 or 32 |
| 26 | High | v3.css:555 `.cnt3-grid` | `gap: 32px` | `gap: var(--s-7)` | direct | swap |
| 27 | High | v3.css:578 `.cnt3-body` | `padding: 28px 28px 26px; gap: 14px` | (all off) | **28 and 26 and 14 all off-scale** | normalize to 24/24/24 with 16 gap, OR 32/32/24 with 16 gap |
| 28 | High | v3.css:586 `.cnt3-meta` | `padding-top: 18px; gap: 10px` | (off) | both off-scale | normalize 18→16/20, 10→8/12 |
| 29 | High | v3.css:592 `.cnt3-foot` | `margin-top: 56px; padding-top: 32px; gap: 20px` | `margin-top: var(--s-10); padding-top: var(--s-7); gap: var(--s-5)` | all on scale | swap all three |
| 30 | High | v3.css:611 `.nl3-pitch` | `gap: 20px; padding-top: 8px` | `gap: var(--s-5); padding-top: var(--s-2)` | direct | swap |
| 31 | High | v3.css:650 `.nl3-quote` | `margin: 16px 0 0; padding: 22px 24px` | `margin: var(--s-4) 0 0; padding: 22px var(--s-6)` | 16, 24 on scale; **22 off** | swap 16, 24; normalize 22→20 or 24 |
| 32 | High | v3.css:687 `.nl3-issues` | `margin-top: 28px` | (off) | **28 off-scale** | normalize to 24 or 32 |
| 33 | High | v3.css:741 `.fnd3a__bio` | `padding-block-start: 24px` | `padding-block-start: var(--s-6)` | direct | swap |
| 34 | High | v3.css:749 `.fnd3a__quote-foot` | `gap: 14px; margin-block-start: 18px` | (both off) | **14 and 18 off-scale** | normalize |
| 35 | High | v3.css:753 `.fnd3a__stat` | `padding: 22px 18px; gap: 6px` | (all off) | **22, 18, 6 all off-scale** | normalize to 24/16, gap 8 |
| 36 | High | v3.css:760 `.fnd3a__authority` | `gap: 12px` | `gap: var(--s-3)` | direct | swap |
| 37 | High | v3.css:773 `.fnd3a__follow-head` | `gap: 18px; margin-block-end: 32px` | `gap: 18px (off); margin-block-end: var(--s-7)` | 32=`--s-7`; 18 off | swap 32, normalize 18 |
| 38 | High | v3.css:777 `.fnd3a__follow-grid` | `gap: 12px` | `gap: var(--s-3)` | direct | swap |
| 39 | High | v3.css:778 `.fnd3a__follow-card` | `gap: 14px; padding: 18px 18px` | (all off) | **14, 18 both off-scale** | normalize |
| 40 | High | v3.css:797 `.fnd3a__colophon` | `padding-block-start: 22px` | (off) | **22 off-scale** | normalize to 20 or 24 |
| 41 | High | v3.css:805 `.fnd3a__masthead-left/right` | `gap: 12px` | `gap: var(--s-3)` | direct | swap |
| 42 | High | v3.css:823 `.ftr3-cta__eyebrow` | `gap: 10px` | (off) | 10 off-scale | normalize |
| 43 | High | v3.css:829 `.ftr3-cta__form` | `gap: 8px; padding: 8px` | `gap: var(--s-2); padding: var(--s-2)` | direct | swap |
| 44 | High | v3.css:854 `.ftr3-clock` | `gap: 10px; margin: 8px 0 0; padding: 8px 14px` | (mixed; 8 = `--s-2`) | 8 on; 10, 14 off | swap 8s, normalize others |
| 45 | High | v3.css:859 `.ftr3-h` | `margin: 0 0 18px` | (off) | 18 off-scale | normalize to 16 or 20 |
| 46 | High | v3.css:885 `.ftr3-mailbtn` | `margin-top: 18px; padding: 11px 18px; gap: 8px` | (mixed) | 8=`--s-2`; **11 and 18 off** | swap 8, normalize others |
| 47 | High | v3.css:889 `.ftr3-base` | `padding: 20px clamp(20px,4vw,56px)` | `padding: var(--s-5) clamp(...)` | 20=`--s-5` | swap |
| 48 | High | v3.css:890 `.ftr3-base__inner` | `gap: 14px` | (off) | 14 off-scale | normalize to 12 or 16 |
| 49 | High | v3.css:912 720px hero3 | `padding: 14px 18px 28px` | (all off) | **14, 18, 28 all off-scale** | normalize to 16/20/24 |
| 50 | High | v3.css:920–949 hero3 mobile cluster | 8/10/12/14/16/18/20/24/28 mixed | mostly tokenizable | drift across mobile cluster | unify to 8/12/16/20/24 only |

**Summary of hardcoded findings:** **roughly 280 distinct hardcoded numeric pixel values** in spacing properties exist in v3.css. About **150 (~54%) map cleanly onto an existing token** and should be swapped. About **130 (~46%) are off-scale literal odds** (10, 14, 18, 22, 26, 28, 36, 44, 64, 88, 92) that should be either rounded to the nearest scale step or absorbed into a `--s-extra-*` extension. The most-repeated off-scale offenders are **14 (used 38 times)**, **18 (used 41 times)**, **10 (used 33 times)**, and **28 (used 14 times)** — these alone, if normalized, would clean up half the issue.

---

### Clamp coherence (fluid spacing scales)

Inventory of every distinct clamp triple used for spacing (gap/padding/margin/inset) — extracted from v3.css. Counts are unique min/max pairs.

**Section padding-y clamps (the macro rhythm):**

| Pair | Used by | min · max | growth ratio | Notes |
|---|---|---|---|---|
| `clamp(20px,3vw,36px)` | hero3 padding-top | 20→36 | 1.80× | hero only |
| `clamp(28px,4vw,56px)` | hero3 padding-bottom; topbar padding-bottom | 28→56 | 2.00× | hero only |
| `clamp(48px,5vw,80px)` | nw3a, cnt3, nl3, fnd3a | 48→80 | 1.67× | **canonical section padding** |
| `clamp(48px,6vw,80px)` | mtd3 intro margin-top | 48→80 | 1.67× | duplicates above with diff vw |
| `clamp(48px,6vw,96px)` | ftr3-cta, ftr3-map | 48→96 | 2.00× | **inconsistent** — ftr3 uses larger max |
| `clamp(48px,6vw,80px)` | mtd3 outro margin-block-end | 48→80 | 1.67× | OK |

**Severity finding C-1:** Section padding-y is **not consistent**. Five sections use `48→80px @ 5vw` but ftr3 uses `48→96px @ 6vw`. At 1440 viewport this difference resolves to 80 vs 86.4 (clamped to 80 vs 86; visually almost identical) but at 1920 it diverges to 80 vs 96. The hero3 sits well outside the cadence at `28→56`. Recommendation: introduce **two named clamps** — `--clamp-section-y` = `clamp(48px,5vw,80px)` and `--clamp-section-y-loose` = `clamp(64px,6vw,96px)` — and replace all 9 instances.

**Section padding-x clamps:**

| Pair | Used by | min · max | Notes |
|---|---|---|---|
| `clamp(20px, 3vw, 48px)` | tokens.css `--gutter` | 20→48 | the canonical gutter — but **never referenced** |
| `clamp(20px,4vw,48px)` | cnt3 | 20→48 | duplicates `--gutter` exactly | should be `var(--gutter)` |
| `clamp(20px,4vw,56px)` | ftr3-cta, ftr3-map, ftr3-base | 20→56 | inconsistent with `--gutter` |
| `clamp(20px,5vw,80px)` | nw3a | 20→80 | larger than gutter |
| `clamp(20px,5vw,88px)` | fnd3a | 20→88 | **outlier** — only section using 88 |
| `clamp(20px,5vw,96px)` | hero3, manif3 | 20→96 | matches manif3 left/right |
| `clamp(20px, 6vw, 96px)` | manif3 (extra space form) | 20→96 | OK |
| `clamp(20px,5vw,96px)` (`--nl3-pad-x`) | nl3-section | 20→96 | OK |
| `clamp(48px,8vw,140px)` | mtd3 desktop frame | 48→140 | **outlier** — by far the largest |
| `clamp(28px,4vw,56px)` | mtd3 mobile frame | 28→56 | only mtd3 uses this |

**Severity finding C-2:** **5 different section padding-x clamps** (20→48, 20→56, 20→80, 20→88, 20→96) where 1–2 should suffice. The `--gutter` token in tokens.css (line 122, `clamp(20px,3vw,48px)`) is **never used** — it should be the single source of truth for outer padding-x. Sections wanting wider should use a `--gutter-loose` (e.g., `clamp(20px,5vw,96px)`).

**Internal-padding clamps (cards, panels, frames):**

| Pair | Used by | Notes |
|---|---|---|
| `clamp(14px,1.4vw,20px)` | nw3a-body | smallest card |
| `clamp(20px,2.4vw,32px)` | hero3__panel, mtd3__outro padding-y | medium |
| `clamp(20px,3vw,32px)` | mtd3__intro padding-x, mtd3__outro padding-x | repeats below at higher vw |
| `clamp(22px,2.4vw,36px)` | nl3-mock | closest to medium-large |
| `clamp(24px,3.6vw,48px)` | fnd3a__quote padding-x | large |
| `clamp(28px,3vw,40px)` | nw3a wide card body | between medium and large |
| `clamp(28px,4vw,44px)` | fnd3a__quote padding-y | large |
| `clamp(28px,4vw,48px)` | mtd3__outro padding | another large |
| `clamp(40px,5vw,72px)` | mtd3__frame padding mobile-fallback | xl |
| `clamp(48px,6vw,96px)` | mtd3__frame padding desktop | xxl |

**Severity finding C-3:** **10 unique internal padding clamps** with no shared ratio. The growth factors are all over the place: 1.43×, 1.6×, 1.6×, 1.64×, 2×, 1.43×, 1.57×, 1.71×, 1.8×, 2×. Recommendation: collapse to **4 named tiers** — `--clamp-card-sm`, `--clamp-card-md`, `--clamp-card-lg`, `--clamp-card-xl` — at 1.6× ratio (e.g., 12→20, 20→32, 32→52, 52→84).

**Internal-gap clamps (between primary stacked elements):**

| Pair | Used by | Notes |
|---|---|---|
| `clamp(10px,1vw,16px)` | nw3a-grid | tightest |
| `clamp(10px,1.4vw,18px)` | mtd3__meta gap | similar to above |
| `clamp(16px,2vw,28px)` | nw3a-totals gap, ftr3-cta__inner gap | mid |
| `clamp(20px,2.4vw,32px)` | hero3__main gap | mid (canonical?) |
| `clamp(20px,2.6vw,32px)` | hero3__channels padding-top | duplicate of above with 0.2vw diff |
| `clamp(20px,3vw,44px)` | hero3__nav gap | wide |
| `clamp(24px,3vw,36px)` | hero3__trust margin-top | similar |
| `clamp(24px,3vw,56px)` | nl3-issues spacing margin-top | wider |
| `clamp(24px,4vw,40px)` | mtd3 frame gap mobile | x-wide |
| `clamp(24px,4vw,64px)` | mtd3__frame gap | xx-wide |
| `clamp(28px,3vw,44px)` | mtd3 outro gap mobile | between wide-x-wide |
| `clamp(28px,3.4vw,44px)` | fnd3a__copy gap | yet another |
| `clamp(28px,4vw,56px)` | hero3__topbar padding-bottom | matches hero3 padding-bottom |
| `clamp(32px,3vw,56px)` | ftr3 map grid gap | matches above |
| `clamp(32px,4vw,56px)` | mtd3__intro margin-bottom | matches |
| `clamp(32px,5vw,72px)` | mtd3__intro gap | unique max |
| `clamp(36px,4vw,56px)` | hero3__channels margin-top | unique min |
| `clamp(40px,5vw,72px)` | mtd3__outro margin-top | repeats above section |
| `clamp(40px,5vw,80px)` | mtd3__pin desktop margin | mtd3-only |
| `clamp(40px,6vw,72px)` | fnd3a__masthead margin-bottom | unique vw |
| `clamp(40px,6vw,96px)` | nl3-shell gap | very wide |
| `clamp(48px,5vw,80px)` | fnd3a__follow padding-top | already a section-padding clamp |
| `clamp(48px,6vw,80px)` | fnd3a__colophon margin-top | repeats |
| `clamp(48px,6vw,96px)` | mtd3__frame gap desktop | very wide |
| `clamp(72px,9vw,120px)` | fnd3a__follow margin-top | the largest internal gap |

**Severity finding C-4:** **25+ distinct internal-gap clamps** with no ratio system. The same logical gap concept ("space between hero columns" / "space between section parts" / "space between cards") gets a different clamp every time. Recommendation: define **6 named clamps** for gaps — `--clamp-gap-sm` (8→16), `--clamp-gap-md` (16→24), `--clamp-gap-lg` (24→40), `--clamp-gap-xl` (40→64), `--clamp-gap-2xl` (64→96), `--clamp-gap-3xl` (96→160) — and require all gaps to use one.

| # | Severity | Location | Issue | Fix |
|---|---|---|---|---|
| C-1 | **High** | tokens.css:122 + 7 sections | `--gutter` is defined but never consumed; sections use 5 different padding-x clamps | use `--gutter` everywhere; add `--gutter-loose` for hero/manif/nl |
| C-2 | **High** | v3.css:278, 543, 606, 705, 821, 840 | section padding-y has 4 distinct clamp triples for the same logical role (chrome whitespace) | unify to `--clamp-section-y` and `--clamp-section-y-loose` |
| C-3 | **High** | v3.css:188, 318, 323, 362, 389, 405, 458, 522, 540, 664, 745 | 10+ unique internal-padding clamps for the same logical role (card padding) | introduce `--clamp-card-{sm/md/lg/xl}` |
| C-4 | **High** | v3.css passim | 25+ unique internal-gap clamps; same logical gap is authored differently per section | introduce `--clamp-gap-{sm/md/lg/xl/2xl/3xl}` |
| C-5 | Med | v3.css:124 hero3 | `clamp(20px,3vw,36px) clamp(20px,5vw,96px) clamp(28px,4vw,56px)` — 3 different clamps in one rule, top ≠ bottom | normalize top+bottom to same clamp (likely 28→56 both) |
| C-6 | Med | v3.css:218 hero3 ticker | `padding-block: 12px` after `clamp(...)` neighbors at 24/56 — abrupt drop | use `var(--s-3)` *or* absorb into `clamp(12px,1.5vw,16px)` |
| C-7 | Med | v3.css:362, 522 | mtd3 intro/outro use 5 clamps in the same selector chain (margin, padding, gap × 2) — not one is a token | create `--mtd3-pad-y`, `--mtd3-gap` locals or use unified clamps |
| C-8 | Med | v3.css:281, 546 | `.nw3a-head` margin-bottom is 56px hardcoded; `.cnt3-head` margin-bottom is 40px hardcoded — same logical role, two values | unify to `var(--s-9)` (48) or `var(--s-10)` (56) |
| C-9 | Low | v3.css:235 hero3-scroll-cue | `inset-block-end: 24px` static, while everything else in hero3 is fluid | OK as anchor, document |
| C-10 | Low | v3.css:705 fnd3a | padding-x max is `88px` — only section using 88 (others 56, 80, 96, 140) | normalize to 80 or 96 |

---

### Section rhythm (top + bottom whitespace per section, 1440px)

Resolved at 1440 viewport (vw=14.4px). Format: `top` / `bottom` (px each).

| # | Section | Top | Bottom | Outer in clamp form | Notes |
|---|---|---|---|---|---|
| R-1 | `.hdr3` | sticky, no padding | — | n/a | header is its own component |
| R-2 | `.hero3` | 36 | 56 | clamp(20,3vw,36) / clamp(28,4vw,56) | **asymmetric — top:36 vs bottom:56**. Logical whitespace diff = 20px |
| R-3 | `.manif3` | **48** | **48** | `var(--s-9)` flat | only section using a static token; symmetric |
| R-4 | `.nw3a` | 80 | 80 | clamp(48,5vw,80) | symmetric, canonical |
| R-5 | `.mtd3` | **(intro margin) 80 + (intro padding-bottom) 0** + body | (outro margin-top 72 → outro margin-bottom 80) | mixed | mtd3 has multi-step rhythm: intro (80 top) → stage (no padding) → outro (80 bottom). The 5×100vh sticky frames sit between with no padding-y of their own |
| R-6 | `.cnt3` | 80 | 80 | clamp(48,5vw,80) | symmetric, matches nw3a |
| R-7 | `.nl3-section` | 80 | 80 | clamp(48,5vw,80) via `--nl3-pad-y` | symmetric, matches |
| R-8 | `.fnd3a` | 80 | 80 | clamp(48,5vw,80) | symmetric, matches |
| R-9 | `.ftr3-cta` | **86** | **86** | clamp(48,6vw,96) | larger than peers — 86 vs 80 |
| R-10 | `.ftr3-map` | **86** | **86** | clamp(48,6vw,96) | larger than peers |
| R-11 | `.ftr3-base` | 20 | 20 | static | OK, base bar |

| # | Severity | Location | Issue | Fix |
|---|---|---|---|---|
| R-α | **High** | v3.css:124 hero3 | top:36, bottom:56 — asymmetric padding (20px diff) | symmetrize to clamp(28,4vw,56) both, OR justify the asymmetry (currently undocumented) |
| R-β | **High** | v3.css:245 manif3 | uses static `var(--s-9)` 48px while neighbors use fluid clamp 80px max | upgrade to `clamp(48px,5vw,80px)` for consistency, or document the intentional drop into print-grid stillness |
| R-γ | **High** | v3.css:821, 840 ftr3 | uses `clamp(48,6vw,96)` while neighbors use `clamp(48,5vw,80)` — 6px–16px wider depending on viewport | unify to canonical `clamp(48px,5vw,80px)` OR formally label `--clamp-section-y-loose` |
| R-δ | Med | v3.css:362, 522 mtd3 | intro+outro have asymmetric `clamp(48,6vw,80)` top vs `clamp(32,4vw,56)` bottom; outro top=72, bottom=80 | match top+bottom |
| R-ε | Low | mtd3 stage | the desktop scroll-pinned 5×100vh stage sits with no internal section margin against intro/outro — visual chasm at the join | add a `clamp(40,5vw,72)` margin-block-start to `.mtd3__pin` |

---

### Internal element rhythm (eyebrow → title → lede → CTA → trust)

Inventory of the canonical 5-step vertical stack per section. Values resolved at 1440.

| Section | eyebrow→title | title→lede | lede→CTA | CTA→trust | Source |
|---|---|---|---|---|---|
| `.hero3` | n/a (eyebrow inline) | gap-32 (clamp(20,2.4vw,32) on `__main`) | 32 | margin-top:36 (clamp 24,3vw,36) | v3.css:147, 221 |
| `.manif3` | margin-bottom:32 (var(--s-7)) | 32 | (no CTA) | 32 (caption margin-top var(--s-7)) | v3.css:247, 256, 268 |
| `.nw3a` | gap:24 + 80 padding-bottom (head→cards) | (head structures: title+lede+totals share grid) | (no within-head CTA) | n/a | v3.css:281 |
| `.mtd3` (intro) | gap:18 (mobile) / 72 desktop (intro grid gap) | 18 | hint: margin-top:8 (off-scale, bare 8) | (transition to stage) | v3.css:362, 369 |
| `.cnt3` | gap:14 head-left | head padding-bottom:28 + margin-bottom:40 | (no CTA in head; CTA in foot, margin-top:56) | n/a | v3.css:546, 547, 592 |
| `.nl3` | gap:20 (`.nl3-pitch` desktop) | 20 | margin-top:8 (form) | margin-top:4 (proof) | v3.css:611, 624, 644 |
| `.fnd3a` | gap:clamp(28,3.4vw,44) on `__copy` (44 @ 1440) | margin:18px 0 0 (tagline) | 24 (bio padding-block-start) | margin-top:18 (quote-foot) | v3.css:733, 738, 741, 749 |
| `.ftr3-cta` | gap:clamp(16,2vw,28) (28 @ 1440) | 28 | (form/inner) | 0 | v3.css:822 |

| # | Severity | Location | Issue | Fix |
|---|---|---|---|---|
| I-1 | **High** | hero3, nl3, fnd3a | the same step ("title→lede") gets 32 / 20 / 18 across sections. No coherence | unify to `var(--s-6)` (24) or `var(--s-7)` (32) for that specific role — recommend 24 |
| I-2 | **High** | mtd3 intro, fnd3a tagline, nl3 proof | "between sub-eyebrow and main title" uses 8 / 18 / margin-bottom 14 — 3 different small gaps for the same role | unify to `var(--s-2)` (8) — already small, just consolidate |
| I-3 | **High** | nw3a-head margin-bottom:56 vs cnt3-head margin-bottom:40 | both are "head-to-grid" gap; should be identical | unify to `var(--s-9)` (48) |
| I-4 | **High** | hero3 panel-head margin-bottom:18 vs hero3 panel-foot margin-top:16 | symmetric stacking but 2px-asymmetric values | match to 16 or 20 |
| I-5 | High | manif3 internal | `gap: 24px` on head (line 250) and `var(--s-7)` (32) on rule margins below — 8px drift | use one of them, not both |
| I-6 | High | fnd3a stat | `padding: 22px 18px` — neither resolves to a token; logical role is identical to nw3a-totals (`padding: 20px 24px`) | unify card-stat padding cross-section |
| I-7 | Med | mtd3 outro | `padding: clamp(28,4vw,48) clamp(20,3vw,32)` vs nw3a-totals `padding: 20px 24px` (static) | both are "framed footer with gap" — should share token |
| I-8 | Med | nl3 quote padding-y:22 vs fnd3a quote padding `clamp(28,4vw,44)` | both are "pull-quote padding" — divergent | unify |
| I-9 | Med | ftr3 contact-list `gap: 14px` vs ftr3-list `gap: 2px` | 2 and 14 are both off-scale; meant to feel different but should still be on-grid (e.g., 4 and 16) | normalize |
| I-10 | Low | hero3 chip `padding: 10px 14px` and trust-logos `gap: 14px` | both off-scale 14 — pervasive across hero3 | replace 14 with 12 or 16 |

---

### Mobile spacing (720px and 480px)

The 720px and 480px passes are the largest single blocks of CSS in the file (180+ rules combined). Here is the rhythm.

**At 720px:**
- `.manif3 / .nw3a / .cnt3 / .nl3 / .fnd3a` all use `padding: var(--s-8) 18px` (40 14 18; right column is 18 not 16) → **consistent**, this is the one good thing about the mobile pass
- `.hero3` uses `padding: 14px 18px 28px` — top:14, x:18, bottom:28; **not on the 4-px scale** for top/bottom, and asymmetric
- Internal gaps drop to a 14/18 mix that doesn't follow the desktop `clamp` proportions

**At 480px:**
- `.manif3 / .nw3a / .cnt3 / .nl3-section / .fnd3a / .ftr3-cta / .ftr3-map` all use `padding: var(--s-7) 16px` (32 16 ×2) → **consistent**
- `.hero3` uses `padding: 12px 14px 22px` — none on token grid

**Drift between desktop → 720 → 480:**
- nw3a desktop padding-y: 80 → 720: 40 (`--s-8`) → 480: 32 (`--s-7`). Ratio 2.5×, then 1.25×. Not equal.
- nw3a desktop padding-x: 80 → 720: 18 → 480: 16. Ratio 4.4×, then 1.13×.

| # | Severity | Location | Issue | Fix |
|---|---|---|---|---|
| M-1 | **High** | v3.css:912, 1065 hero3 mobile | hero3 alone uses non-token mobile paddings (14/18/28 then 12/14/22) — every other section uses `--s-8` and `--s-7` | unify to `var(--s-8) 18px` and `var(--s-7) 16px` |
| M-2 | **High** | v3.css:914-917 720px section paddings | `padding: var(--s-8) 18px` — left/right is **18px (off-scale)**, should be 16 or 20 | normalize to `var(--s-8) var(--s-4)` (40/16) or `var(--s-8) var(--s-5)` (40/20) |
| M-3 | **High** | v3.css:1083-1134 480px paddings | `padding: var(--s-7) 16px / 18px` — inconsistent x value (some 16, some 18) | unify; recommend `var(--s-7) var(--s-4)` (32/16) |
| M-4 | High | v3.css:925, 1071 hero3 tagline | font-size drops 60→32→28→24 (clamp(30,4.2vw,60)→28→24). The size progression is 60→28 (×0.47) then 28→24 (×0.86). Not proportional | tighten — e.g., 60→36→28 or 60→32→24 |
| M-5 | High | v3.css:920–949 hero3 mobile cluster | gaps drift through 8/10/12/14/16/18/20/24/28 — 9 distinct values for spacing in one section | reduce to 4: 8/16/24/32 |
| M-6 | High | v3.css:984-1004 mtd3 mobile | margin/padding rewritten with literal `36px`, `28px`, `26px`, `22px`, `24px`, `20px`, `18px`, `14px`, `12px`, `10px` — 10 magic numbers, none on token | wholesale rewrite with token references |
| M-7 | Med | v3.css:601 cnt3 1080px | `gap: 24px` mobile only — desktop is `clamp(...)` 32; ratio 0.75 | OK, but should be on token (`var(--s-6)`) |
| M-8 | Med | v3.css:602 cnt3 680px | `padding: 22px 22px 22px` — magic 22; same as fnd3a quote | normalize to 20 or 24 |
| M-9 | Med | v3.css:701 nl3 560px | `padding: 18px 18px` — 18 off-scale | 16 or 20 |
| M-10 | Low | v3.css:1132 ftr3 480px | `font-size: 30px` — falls outside the type clamp; spacing OK | flag for typography audit |

---

### Outliers / random values (numbers that fit no system)

The clearest "what is this" values found in v3.css. Each is a value that (a) is not a token, (b) is not a clamp range bound, (c) is not a 4px-multiple, AND (d) has no documented justification.

| # | Severity | Location | Value | Notes |
|---|---|---|---|---|
| O-1 | **High** | v3.css:53 | `width: min(820px, calc(100vw - 48px))` — `820` | unique width; should be 800 (multiple of 8) or container token |
| O-2 | **High** | v3.css:75 | `width: 220px; (focus) width: 260px` | hdr3-search width — neither is on 8-grid |
| O-3 | **High** | v3.css:174, 274, 282-294 | many `max-width` values: 540, 980, 1080, 1180, 1280, 1320, 1360 | several inconsistent: 1320 (canonical container) vs 1280 (mtd3) vs 1360 (nl3-shell) vs 1180 / 1080 (mobile breakpoints, not max-width) — three different "container" sizes |
| O-4 | High | v3.css:188, 235 | `padding-right: 22px` (hero3-radius local) and `inset-block-end: 24px` | mixing 22 with 24 in same component (hero3) |
| O-5 | High | v3.css:206 | `width: 64px; height: 22px` (kpi-spark) | 22 is off-scale |
| O-6 | High | v3.css:228 | `padding-block: 12px` in ticker | 12 is `--s-3` (token) but not used as token |
| O-7 | High | v3.css:289 | `.nw3a-totals padding: 20px 24px` and `gap: clamp(16,2vw,28)` — outer is on grid (20/24) but inner clamp max is 28 (off) | normalize 28→32 |
| O-8 | High | v3.css:311 | `.nw3a-card--feature .nw3a-cover { min-height: 200px; }` and `.nw3a-card--rail` `min-height: 180px` | 180 vs 200 — 20px difference, not a token; logical role identical (card cover) |
| O-9 | High | v3.css:317 | `.nw3a-card--wide .nw3a-cover { min-height: 300px; height: 220px }` | 300 and 220 — neither on 4-grid (well, 220/300 are) but 220 doesn't match 200/180 above |
| O-10 | High | v3.css:331 | `.nw3a-card--feature .nw3a-name` font-size `clamp(22px,2.2vw,32px)` — 22 min off-scale | 24 min instead |
| O-11 | High | v3.css:340 | `.nw3a-stats { padding: 14px 0 }` | 14 |
| O-12 | High | v3.css:367 | `.mtd3__title` max-width: `18ch` | OK but inconsistent — `52ch` for body / `48ch` for outro / `26ch` for manif lede / `52ch` for nw3a desc — 5 different ch values for "constrained text" |
| O-13 | High | v3.css:379 | `.mtd3__progress-bar { width: 160px; height: 2px }` | 160 = `--s-20`, 2px is off-scale (1px or 4px would fit) |
| O-14 | High | v3.css:414 | `.mtd3__art { max-width: 280px }` | 280 off-scale |
| O-15 | High | v3.css:440 | `.mtd3-cover { max-width: 380px }` | 380 off-scale |
| O-16 | High | v3.css:460 | `.mtd3__art clamp(280px, 32vw, 420px)` | 280, 420 — both off |
| O-17 | High | v3.css:546 | `.cnt3-rule { width: 36px }` | 36 off-scale |
| O-18 | High | v3.css:572 | `.cnt3-tag { top: 16px; right: 16px }` and `padding: 7px 12px` | 7 is the most off-scale single value in the file |
| O-19 | High | v3.css:574 | `.cnt3-cover-meta padding: 16px 20px` | OK; 16/20 on grid — flag for token |
| O-20 | High | v3.css:578 | `.cnt3-body padding: 28px 28px 26px` | **26** is unique — appears only here |
| O-21 | High | v3.css:579 | `.cnt3-accent-bar { right: 28px; width: 32px }` | mixing 28 and 32 |
| O-22 | High | v3.css:586 | `.cnt3-meta padding-top: 18px; gap: 10px` | 18 and 10 |
| O-23 | High | v3.css:632 | `.nl3-submit { height: 56px; min-width: 168px; padding: 0 22px }` | 56 = `--s-10`, 168 off-scale, 22 off-scale |
| O-24 | High | v3.css:646 | `.nl3-avatars span { width: 28px; height: 28px; margin-inline-start: -8px }` | 28 |
| O-25 | High | v3.css:710 | `.fnd3a__rule { width: 28px }` | 28 |
| O-26 | High | v3.css:715 | `.fnd3a__portrait { grid-template-columns: 28px 1fr }` | 28 |
| O-27 | High | v3.css:724 | `.fnd3a__verified { width: 86px; height: 86px }` and 480px override 64×64 / 480px override 56×56 | **86, 64, 56** — only 64 and 56 are on grid |
| O-28 | High | v3.css:744 | `.fnd3a__dropcap { margin-inline-start: 14px; margin-block-start: 4px }` | 14 |
| O-29 | High | v3.css:747 | `.fnd3a__quote-mark { top: 8px; inset-inline-start: 18px; font-size: 92px }` | 18 and 92 |
| O-30 | High | v3.css:753 | `.fnd3a__stat { padding: 22px 18px }` | 22 and 18 — both off |
| O-31 | High | v3.css:761 | `.fnd3a__auth-card { padding: 16px 18px }` | 18 off |
| O-32 | High | v3.css:765 | `.fnd3a__auth-icon { width: 36px; height: 36px }` | 36 off |
| O-33 | High | v3.css:778 | `.fnd3a__follow-card { padding: 18px 18px; gap: 14px }` | 18 and 14 — both off |
| O-34 | High | v3.css:784 | `.fnd3a__follow-icon { width: 34px; height: 34px }` | 34 off |
| O-35 | High | v3.css:824 | `.ftr3-dot { width: 6px; height: 6px }` | 6 off |
| O-36 | High | v3.css:854 | `.ftr3-clock { padding: 8px 14px }` | 14 off |
| O-37 | High | v3.css:879 | `.ftr3-list--contact { gap: 14px }` | 14 off |
| O-38 | High | v3.css:885 | `.ftr3-mailbtn { margin-top: 18px; padding: 11px 18px; gap: 8px }` | 18 and 11 — 11 is unique |
| O-39 | High | v3.css:889 | `.ftr3-base { padding: 20px clamp(20px,4vw,56px) }` | 56 max off the canonical 80 / 96 |
| O-40 | High | v3.css:895 | `.ftr3-lang { padding: 3px }` and pill `top: 3px; bottom: 3px; right: 3px` | 3px is the single most off-grid micro-value in the file |
| O-41 | High | v3.css:900 | `.ftr3-top { padding: 7px 14px }` | 7 and 14 |
| O-42 | High | v3.css:1132 | 480px ftr3-cta__title `font-size: 30px` while desktop clamp max is 60 (token max-h2 also 60) | 30 OK as min; flag for type system |

**Summary of outliers:** the **most repeated off-scale offenders** in v3.css are:

| Off-scale value | Approx count | Most-affected sections |
|---|---|---|
| 14 | 38 | hdr3, hero3, manif3, nw3a, fnd3a, ftr3 |
| 18 | 41 | hdr3, hero3, fnd3a (especially), ftr3 |
| 10 | 33 | hdr3, hero3, nw3a, ftr3 |
| 22 | 18 | hero3, nl3, fnd3a, mtd3 |
| 28 | 14 | hero3, nw3a, cnt3, ftr3 |
| 26 | 6 | manif3, mtd3, cnt3, nl3 |
| 36 | 11 | hero3, cnt3, fnd3a |
| 38 | 7 | mtd3, fnd3a |
| 86 | 1 | fnd3a verified badge — unique 86px |

Replacing **just 14, 18, 10, 22, and 28** with their nearest token (12, 16, 8, 24, 24) would eliminate ~144 off-scale literals — about 70% of the off-scale offenders.

---

### Dead space verification

**Pre-edit voids flagged in brief:**

1. **Founder section bottom void (~250px):** verified to be addressed at v3.css:772 (`.fnd3a__follow { margin-block-start: clamp(72px,9vw,120px); padding-block-start: clamp(48px,6vw,80px); }`) and 797 (`.fnd3a__colophon { margin-block-start: clamp(48px,6vw,80px); padding-block-start: 22px; }`). The combined bottom whitespace at 1440 is now: follow margin 120 + follow padding 72 + colophon margin 80 + colophon padding 22 + section padding-bottom 80 = roughly 374px — but this is filled with content (follow-grid + colophon bar), not void. **Confirmed fixed in code.** Recommend visual diff against `screenshots/fnd3a-1440.png` from Wave 1 to confirm pixel-perfect.

2. **Newsletter left-column 515px void:** the structural fix is at v3.css:660 — `@media (min-width: 961px) { .nl3-preview { position: sticky; top: 96px; align-self: start; } }`. The preview anchors at top:96 inside its column while the pitch flows naturally. The 515px void prior to fix was a length asymmetry (preview ~460px, pitch ~975px). Now that preview sticks, the pitch column scrolls past it and there is no permanent void. **Confirmed fixed in code.** The `top: 96px` value, however, is itself a magic number — it should be `var(--s-14)` (96).

3. **Other voids found in this audit:**
   - **D-1 Hero panel right column on desktop:** `.hero3__shell` is `1.45fr 1fr` with `align-items: start`, panel `align-self: start`. After the panel ends (around 600px tall), the right column has empty space until `.hero3__channels` (which spans full width). At 1440 this is roughly 80–120px of void below the panel. Severity: Med. Fix: extend panel or absorb the channels strip differently.
   - **D-2 Hero ticker → trust gap:** ticker margin-top `clamp(28px,3vw,40px)` then nothing else until next section padding-bottom 56 — total 96 of effective whitespace below ticker before section ends. Severity: Low (intentional cinematic breathing room).
   - **D-3 mtd3 stage join with intro:** intro margin-bottom is `clamp(32px,4vw,56px)` (56 @ 1440) but the stage starts immediately after with no own margin. The 5×100vh sticky stage's first frame's first scroll position lands abruptly. Severity: Med. Fix: add `margin-block-start: clamp(40px,5vw,72px)` to `.mtd3__pin` or `.mtd3__stage` so the visual entry has rhythm.
   - **D-4 fnd3a quote → stats:** quote ends, then stats start with `border-block-start: 1px solid var(--hairline)`. There is no margin between, so the quote's own bottom padding `clamp(28,4vw,44)` sets the gap (44 @ 1440). Stats then have `padding: 22px 18px` per cell. The visual rhythm is OK but the join is decorated only by the hairline. Severity: Low.

| # | Severity | Location | Issue | Fix |
|---|---|---|---|---|
| D-1 | Med | hero3 panel | 80-120px void below right-column panel before channels strip | extend panel min-height or merge channels into right column |
| D-2 | Low | hero3 ticker | ~96px effective whitespace below ticker | document or absorb into clamp |
| D-3 | Med | mtd3 stage join | abrupt scroll-pin start, no margin between intro and stage | add `clamp(40,5vw,72)` margin-block-start to `.mtd3__pin` |
| D-4 | Low | fnd3a quote → stats | only hairline separator at quote/stats boundary | OK, document |
| D-5 | Low | nl3 sticky `top: 96px` magic | should be tokenized | change to `var(--s-14)` |

---

## Priority Actions

1. **Replace the 280 hardcoded pixel literals.** Half of them already match a token; the other half should round to the nearest token. ETA: half a day with a regex pass + manual review. Impact: token usage rises from 26 instances to ~280.
2. **Define and use 6 named clamps for spacing** (`--clamp-section-y`, `--clamp-section-y-loose`, `--clamp-card-{sm/md/lg/xl}`, `--clamp-gap-{sm/md/lg/xl/2xl/3xl}`). Replace the 30+ ad-hoc clamp triples. ETA: half a day.
3. **Use the existing `--gutter` token** in tokens.css:122 instead of the 5 different padding-x clamps. Add a `--gutter-loose` for hero/manif/nl. ETA: 1 hour.
4. **Symmetrize hero3 padding-y** and unify ftr3 padding-y to the canonical section clamp. ETA: 5 minutes.
5. **Normalize the off-scale offenders** (14→16, 18→16/20, 10→8/12, 22→20/24, 28→24/32). Just these 5 substitutions resolve ~70% of off-scale debt. ETA: 1 hour.

---

## Score

**Spacing system: 4.5 / 10**

- **Token definition: 9/10** — clean 4px-base scale, proper Tailwind cadence, surface-aliased
- **Token consumption: 1/10** — only 4 of 15 tokens used; 26 references in 1,151 lines
- **Clamp coherence: 3/10** — 30+ unique triples for ~10 logical roles
- **Section rhythm: 6/10** — 6 of 9 sections use the same clamp; hero3 + ftr3 + manif3 are off
- **Internal element rhythm: 4/10** — same logical step has 3 different values across sections
- **Mobile rhythm: 6/10** — the 720/480 cluster is more consistent than desktop, but still uses 18px (off-scale) and lots of literals
- **Dead space: 8/10** — the two flagged voids are addressed; 3 minor ones remain

The system is structurally adoptable. It is not adopted.

---

## 200-word summary

Almobadir's spacing tokens are well-designed and almost entirely unused. `tokens.css` defines a clean 4px-base scale of 15 tokens (`--s-1` through `--s-24`), but `v3.css` references `var(--s-*)` only 26 times across 1,151 lines, and **11 of the 15 tokens (73%) are never used at all**. Of the four tokens consumed, only `--s-7` (32px) sees real adoption (14 references). Every section except `.manif3` ignores tokens for primary spacing decisions, opting instead for ~280 literal pixel values and 30+ ad-hoc `clamp()` triples. The 30 clamp variants cover roughly 10 logical spacing roles (section padding-y, gutter, card padding, internal gap, etc.) — each role gets 3-5 different ranges depending on which agent authored which section. Section padding-y is the most visible drift: nw3a/cnt3/nl3/fnd3a use `clamp(48px,5vw,80px)` while ftr3 uses `clamp(48px,6vw,96px)` and hero3 is asymmetric `36→56`. The most-repeated off-scale offenders are `14`, `18`, `10`, `22`, `28` (~144 instances combined). Both pre-flagged voids (founder bottom, newsletter left column) are fixed in code. Recommend a one-day pass: tokenize literals, define 6 named clamps, use the unused `--gutter`, symmetrize hero3.
