# Wave 2 — FOUNDER Section Audit (`.fnd3a`)

**Audited:** 2026-04-26
**Section:** `<section class="fnd3a" id="founder">` — index.html lines 1001–1122
**CSS:** assets/v3.css lines 704–1144
**Live URL:** http://localhost:8765/index.html#founder
**Reviewer:** UX architect / vanity-engineering / typography / design-audit / web-interface-guidelines passes
**Bar:** billion-dollar website. Benchmarks: NYT Magazine contributor pages, The Atlantic staff bios, Substack about-author, Stripe people, Linear blog authors, The Browser Co. team.

---

## Executive Summary

The FOUNDER section reads like an enthusiastic Behance template that has been told it works at *The New York Times*. Skeleton, eyebrows, FIG. 01 caption, mono masthead, vertical marquee rail, columned profile — the *grammar* of an editorial profile is all here. But every element has been turned up two clicks past authenticity: the masthead carries TWO datelines + a "DOSSIER · 001" stamp, the portrait spins a self-applied "VERIFIED" badge while a marquee orbits the photo, three "AUTHORITY" cards (one of which is literally a Favikon score), and a four-card follow grid that ends in a Calendly CTA. None of the institutions whose grammar is being borrowed (NYT, Atlantic, Stripe people page) wrap a single founder in this much vellum.

The section's biggest problem is not visual — the visual is competent. The problem is **vanity at the source**. A 150×150 px JPEG (7.7 KB) is doing the work of an editorial portrait. A single founder of a 9-month-old media brand is wearing a verified-account ring, a "DOSSIER" stamp, an "AUTHORITY SCORE: 8,545" card and a "NETWORK REACH: 1.5M+" card *that double-counts the audience* already shown in stats. The "TOP 20 B2B SAUDI ARABIA · 2025" appears 4 separate times in 1,663 px of vertical space. This is the vanity surface area: when you remove the editorial wallpaper, what is left is a single, decent line — *#6 Favikon B2B SA 2025, +325K followers, 5,000+ pieces shipped* — repeated five different ways.

Typography, RTL handling, and motion are solid; the recent fixes (name no longer clipped, dropcap removed) hold. The mobile rebuild at 720px is correct. But three things keep this from a 10/10: (1) photo asset is amateur, (2) authority/badge stack is performative, (3) the redundant repetition of the Favikon claim drains its credibility instead of compounding it.

**Verdict:** The chassis is editorial. The cargo is influencer. Cut the cargo by 40% and replace the photo and this section can hold its own next to The Atlantic. Keep it as-is and it telegraphs "founder reading too much *Monocle*."

**Vanity Severity:** V2 (Structural) — architecture is shaped around how the founder *wants to be seen* rather than what a reader needs to trust him.

**Requirement-to-Complexity Ratio (RCR):** 7/10 — should be 3/10. Job-to-be-done: "tell the reader who runs this and why to trust him." Current implementation: 14 typographic gestures, 3 simultaneous animations, 4 follow targets, 6 numeric proof points, ~140 DOM nodes for a single bio.

---

## Findings

### Severity legend
- **CRIT**: actively damages credibility, ships bugs, or fails accessibility/perf bars
- **HIGH**: meaningful gap from billion-dollar bar; visible to careful viewers
- **MED**: refinement that lifts polish floor
- **LOW**: nit / micro
- **WORKS**: positive — keep as-is

| # | ID | Severity | Area | Finding | Evidence | Recommendation |
|---|---|---|---|---|---|---|
| 1 | F-VANITY-PHOTO | **CRIT** | Asset quality | Portrait is **150×150 px, 7.7 KB**, and reads as an AI-stylized illustration on a generic mandala/floral background, not a photograph. At 1440 the frame renders ≈480×600 — image is being upscaled 3.2× and shows it. This is the single largest credibility hit in the section. NYT/Atlantic/Stripe people pages all use studio portraits at 1200×1500+. | `assets/Badrshaqer.jpg` is JPEG 150×150, 7,704 bytes (`file` output). Visible compression+pattern artifacts in fnd3a-1440.png. | Commission a real portrait at 1600×2000 in 4:5; alternatively, retake on phone in window light against a plain wall. Ship at 800×1000 (1×) and 1600×2000 (2×) via `srcset`. Until then, hide the image and replace with a typographic name-card — far better than a fake editorial portrait. |
| 2 | F-VANITY-VERIFIED | **CRIT** | Trust / vanity | Spinning red "VERIFIED" badge is **self-applied iconography**. The text on the ring reads "@BADRSHAQER · VERIFIED" — there is no platform behind that claim. Aria label "حساب موثّق" reinforces it. This is the strongest vanity signal on the page. | `index.html:1021–1027`, `v3.css:724–729` (`fnd3aSpin 24s linear infinite`). | **Remove the badge entirely.** If you must keep visual emphasis on the portrait, use a small crimson corner tab that reads "FIG. 01" or the date — neutral, editorial. Trust comes from named third-party authority (Favikon, publication mentions), never from a badge you award yourself. |
| 3 | F-VANITY-DOSSIER | HIGH | Editorial dressup | Masthead carries **THREE simultaneous editorial flags** for one founder bio: "DOSSIER · 001", "ALMOBADIR / FOUNDER FILE", and the date "26.04.2026". This is the *Wes-Anderson* problem — every prop is in frame, none earns its place. NYT contributor pages have ONE element: the contributor's name. | `index.html:1003–1014`. | Keep ONE: just `ALMOBADIR · المؤسس`. Remove "DOSSIER · 001" (numbered dossier implies a series — there is none) and the date (this isn't a publication date; it's the day the page was scaffolded). |
| 4 | F-VANITY-FIG01 | HIGH | Editorial dressup | "FIG. 01" caption assumes a figure-numbered article exists around it. There is no FIG. 02. A lone "FIG. 01" reads as costume, not editorial. The Atlantic uses figure numbers when there are multiple figures in long-form. | `index.html:1028`. | Drop "FIG. 01". Caption should be: `بدر شاكر — الرياض، 2025`. The dot/dash and date are enough. |
| 5 | F-VANITY-RAIL | HIGH | Decorative motion | Vertical marquee rail repeats `PORTRAIT · BADR SHAKER · FOUNDER · ALMOBADIR · #6 FAVIKON SAUDI B2B 2025` next to a portrait that already says all this. It is illegible at 10px tracking 0.32em on a moving track, decorative only, and adds a third concurrent animation (rail + verified spin + photo zoom). | `index.html:1030`, `v3.css:716–718` (28s linear). | Remove the rail OR replace it with a *static* rotated label that says only "FIG. 01 · 2025" — adds editorial texture without claim-stacking. Reclaim the 28px column for portrait width. |
| 6 | F-VANITY-AUTHORITY-DUP | **CRIT** | Information redundancy | The same Favikon #6 claim appears **four times** in this section: stat #1 ("FAVIKON · 2025 #6"), authority card #1 ("FAVIKON RANKED #6"), tagline ("Top 20 B2B Saudi Arabia · 2025"), and follow card #4 ("Favikon · Profile · Authority 8,545 #6"). Authority cards repeat what the stats grid already states. Repetition signals insecurity, not authority. | `index.html:1036, 1048–1051, 1064–1068, 1108–1115`. | Pick **one** placement. The stats grid (`<dl>`) is the right home for the number. Delete the entire `.fnd3a__authority` block — it's a vestigial second draft of the stats. The Favikon link belongs in the follow grid. |
| 7 | F-VANITY-NETWORK | HIGH | Inflated claim | Authority card 3 says "NETWORK REACH 1.5M+ متابع · إدارة الشبكة" — sums all the brand handles' follower counts (mindset 478K + almobadir 363K + badrshaqer 317K + flousak 202K + business 124K + media 57.7K ≈ 1.54M). This double-counts overlapping audiences and conflates *brand reach* with *founder reach*. Same approach to a NYT bio would say "managing 200M readers." | `index.html:1073–1077`. | Either remove the card, or reframe as "AUDIENCE UNDER MANAGEMENT · 6 brands" (count of properties, not summed followers). |
| 8 | F-VANITY-AUTHSCORE | HIGH | Opaque metric | "AUTHORITY SCORE 8,545" is a Favikon proprietary metric meaningful to roughly 0% of readers. It is dressed in a star icon to imply institutional weight. | `index.html:1069–1072`. | Drop the card. If the Favikon connection matters, the link in the follow grid (#04) is enough. |
| 9 | F-VANITY-DOSSIER001 | MED | Numbered series fiction | "DOSSIER · 001" implies dossier 002, 003 exist. The section is a single founder bio. Numbering one of one is theatre. | `index.html:1009`. | Remove. |
| 10 | F-VANITY-QUOTE | MED | Self-attributed quote | The pull quote "محتوى يغنيك عن دورات كاملة" is attributed to *"الفلسفة التحريرية لـ المبادر"* — i.e. the company's own editorial philosophy quoting itself. NYT/Atlantic pull quotes are by **named third parties** or by the subject (with their name beside it). A house quoting its house philosophy reads like a coffee-shop wall. | `index.html:1041–1045`. | Either: (a) attribute to **بدر شاكر** by name with a date, or (b) cut the block and let the bio paragraph carry the message. |
| 11 | F-PHOTO-SUSPECT-AI | HIGH | Authenticity | The portrait visibly has the signature of an AI-stylization filter (smooth skin, painterly eyes, ornate symmetric mandala behind subject). Worse than a low-res photo: an AI-stylized one. Editorial credibility cannot survive AI filters on the founder's face. | Visual inspection of `assets/Badrshaqer.jpg`. | Replace with a real, unstylized photograph. This is non-negotiable for the editorial frame to work. |
| 12 | F-TYPO-NAME-MARGIN | MED | Typography | Name `بدر شاكر` uses `padding-block: 0.06em` (recent fix) on a `clamp(40px,5vw,76px)` line — the fix prevents clipping but the gradient text loses about 4–6 px of cap visibility on macOS Safari with Almarai fallback. Verify with descenders. The fix works but is a workaround for the missing brand font. | `v3.css:737`. | Once `Mostaqbali` font ships, drop padding-block. As-is it works; document as known workaround in `LESSONS.md`. |
| 13 | F-TYPO-GRADIENT-RTL | MED | Typography | Name uses `linear-gradient(180deg, #FBF7EE 0%, #F5EAD2 100%)` — top-cream → bottom-warm. Gradient goes from `#FBF7EE` to `#F5EAD2`: a 6% luminance drop. On a dark void background this gradient is invisible at glance — the two colors are too close. The "gradient text" effect is performing zero visual work. | `v3.css:737`. | Either commit to a real gradient (e.g. ink → crimson) for emphasis, or drop the gradient and use solid `var(--ink)`. Performative gradients hurt more than they help. |
| 14 | F-TYPO-BIO-JUSTIFY | MED | Arabic typography | Bio uses `text-align: justify` with `text-justify: inter-word` in Arabic. Browsers do **not** kashida-justify Arabic; they fall back to crude word-spacing, producing rivers in a 65–80ch line. Mobile correctly drops to `text-align: start` (line 1058). Desktop should match. | `v3.css:742`. | Set `text-align: start` desktop too. Justified Arabic without proper kashida support looks worse than left-aligned. |
| 15 | F-TYPO-BIO-LINE | MED | Line length | Bio paragraph at 1440 lives in the right column ≈ 6/(5+6) of grid minus 80px gap → ≈ 700px wide. At 20px Arabic body that's ~85ch — at the upper edge of the 45–90 band. Combined with `line-height 1.85` (high but defensible for Arabic), the paragraph reads heavy. | `v3.css:742`. | Set `max-width: 62ch` on `.fnd3a__bio-text`. Reduce line-height to 1.75. |
| 16 | F-TYPO-VERIFIED-7PX | LOW | Microtype | Verified ring SVG text is `7.4px` (5.6px on mobile) — well below readable threshold. The ring is decorative; this is fine *if* you remove the badge (F-VANITY-VERIFIED). If kept, the text functions as texture only. | `v3.css:727, 1049`. | Tied to F-VANITY-VERIFIED. If badge stays, text size is acceptable for ornament. |
| 17 | F-TYPO-MIXED-SCRIPT | MED | Bidi | Tagline "بزنس و تسويق ● Top 20 B2B Saudi Arabia · 2025" mixes Arabic body and Latin mono inline. The bullet between them is an em-bullet (8px crimson). Bidi resolution puts the EN segment LTR-embedded inside an RTL parent, which Chrome handles correctly but Firefox and older Safari sometimes leave the period ambiguous. | `index.html:1036`, `v3.css:738–740`. | Wrap the EN segment in `<bdi>` or set `unicode-bidi: isolate` on `.fnd3a__tagline-en`. Currently neither is set. |
| 18 | F-TYPO-STAT-RANK | LOW | Number alignment | Stat "FAVIKON · 2025" displays `# 6` with `font-size: 0.7em` on the `#`, baseline aligned. The rank glyph and digit don't share a baseline cleanly because the `#` sits at the cap-height while the digit fills the body. | `v3.css:758`. | Use `align-self: baseline` and `font-size: 0.62em` on `.fnd3a__stat-rank`, or wrap `#6` together as a single span without size split. |
| 19 | F-RTL-MASTHEAD | LOW | RTL | Masthead-left has rule + eyebrow + sep + DOSSIER · 001 (line 1004–1009). In RTL `flex-direction: row` reverses, so the crimson rule lands on the right edge. This is correct visually (mirrors NYT-style underline-in front of the eyebrow) but the *content order* is "rule → label → sep → mono-stamp" which inverts to "stamp · sep · label rule" — readable but ambiguous about what the rule is annotating. | Visual at fnd3a-1440.png. | Either remove the rule (simplify), or make it read as a clear element separator (full-width 1px under the eyebrow). |
| 20 | F-RTL-CAP | LOW | RTL | Caption uses `inset-inline-start: 16px` so in RTL the FIG. 01 number is right-anchored. Order of "FIG. 01 · text" with RTL flex-direction reverses to "text · FIG. 01" visually — fine, intentional. WORKS. | `v3.css:730`. | Keep. |
| 21 | F-A11Y-VERIFIED-LABEL | HIGH | Accessibility | `<div class="fnd3a__verified" aria-label="حساب موثّق">` — div with aria-label is announced as a region by some screen readers, but div has no role. Also: aria-label asserts a falsehood (account is not platform-verified). | `index.html:1021`. | Tied to F-VANITY-VERIFIED — remove element. If kept, change to `role="img" aria-label="شارة التحقق"` (badge of verification, not "verified account") and remove the misleading semantic. |
| 22 | F-A11Y-DL | WORKS | Accessibility | Stats correctly uses `<dl><dt><dd>` semantics with `aria-label="إحصاءات المؤسس"`. Excellent. | `index.html:1046–1062`. | Keep. |
| 23 | F-A11Y-BLOCKQUOTE | WORKS | Accessibility | Quote uses real `<blockquote>` with `<footer>` for attribution. Correct. | `index.html:1041–1045`. | Keep. |
| 24 | F-A11Y-H2 | WORKS | Accessibility | Name is `<h2>` with explicit `id` referenced by section's `aria-labelledby`. Correct heading model. | `index.html:1001, 1035`. | Keep. |
| 25 | F-A11Y-ALT | MED | Accessibility | Photo alt is `"بدر شاكر، مؤسس المبادر"` — adequate but minimal. Editorial photos benefit from descriptive alt: setting, attire, mood for non-sighted users to share the editorial gesture. | `index.html:1019`. | Expand to e.g. `"صورة شخصية لبدر شاكر، مؤسس المبادر، الرياض 2025"`. |
| 26 | F-A11Y-FOLLOW-LABEL | LOW | Accessibility | Follow cards use platform name + handle but no `aria-label` on the link. Screen reader reads `01 [icon] Instagram @badrshaqer 317K ↗` — verbose. | `index.html:1087–1115`. | Add `aria-label="تابع على إنستغرام @badrshaqer (317 ألف متابع)"` etc. |
| 27 | F-MOTION-CONCURRENT | HIGH | Motion | At rest the section runs **3 concurrent infinite animations**: verified spin (24s), tick counter-spin (24s), rail marquee (28s). All visible above the fold once scrolled in. None contributes information. Concurrent infinite animation in editorial layouts breaks the "still page" gestalt that NYT/Atlantic rely on. | `v3.css:716–718, 724–728`. | Cut all three at the source. They survive the reduced-motion guard (line 1144) but should not exist for full-motion users either. The photo zoom on `is-inview` (1.02→1.06) is enough kinetic interest. |
| 28 | F-MOTION-PHOTO-ZOOM | MED | Motion | Photo `transform: scale(1.06)` on inview + `scale(1.09)` on hover compounds nicely on a real photo. On a 150×150 source upscaled, the zoom magnifies compression artifacts. | `v3.css:720–722`. | Tied to F-VANITY-PHOTO. With a high-res replacement, zoom is fine. With current source, zoom makes the photo worse. |
| 29 | F-MOBILE-PORTRAIT | WORKS | Responsive | At ≤720 the rail is hidden, frame becomes max 420px centered, verified shrinks to 64px (56px at 480). Layout works at 390 and 375. The recent grid-collapse fix is correct. | `v3.css:1044–1051, 1126–1130`. | Keep. |
| 30 | F-MOBILE-FOLLOW | MED | Responsive | Follow grid moves to 2 columns at ≤960. The CTA card (`#03 Calendly · BOOK`) is no longer visually distinct from the others on mobile because the gradient looks similar at small sizes and order shuffles. | `v3.css:813`. | Pull the CTA card out of the grid on mobile and stack it full-width above the grid. It's the action card; treat it like one. |
| 31 | F-COLOR-CRIMSON-LOAD | MED | Color | Crimson `#E6213B` is used in: rule, eyebrow first child, bullet, bio strong, quote bar+gradient+mark, stat rank/plus, auth-card-rank gradient, auth-icon, follow-stat, follow-card-cta gradient, follow-card-cta border, follow-arrow hover, colophon n/a. Approximately **13 distinct uses** in one section. Loses meaning. | `v3.css:704–798`. | Reduce crimson to: name accent (none currently), bio strong (#6), CTA card. Use `--ink-mute` and `--hairline` for everything else. |
| 32 | F-FOLLOW-CTA-WEIGHT | MED | Hierarchy | The 4-card follow grid puts Calendly (the actual conversion goal) at position 3 of 4, sandwiched between Threads and Favikon. The eye lands on #1 Instagram. | `index.html:1086–1116`. | Re-order to `Calendly → Instagram → Threads → Favikon` OR break the CTA out as the primary action and demote the rest. |
| 33 | F-COLOPHON | LOW | Polish | The `.fnd3a__colophon` footer (line 1117–1121) is `aria-hidden`, mono, "● 26.04.2026 ALMOBADIR / FOUNDER · DOSSIER 001 ●" or similar. It exists below the follow grid. Editorially nice idea, but on a single-page site this colophon below ONE section reads as overdressing. | `index.html:1117`, `v3.css:797–798`. | Cut. Save the colophon device for the page-end footer. |
| 34 | F-1998-RISK | MED | Cross-site consistency | (External-context flag, not in this section's code.) Other context indicates a "Fundado 1998" claim used elsewhere on group properties. If the founder bio cites the brand as the founder's, the bio currently says "9-month-old store / brand" implicitly. Verify the founder's bio doesn't backstop a 1998 claim that contradicts. | Out-of-scope cross-check. | Audit any "since 1998" / "founded 1998" copy elsewhere on `almobadir.com` against the founder's stated educational + career timeline. |
| 35 | F-VANITY-FOLLOW-GRID | HIGH | Vanity / scope | The 4-card follow grid + colophon at the bottom of the founder section is a *second hero* — it doubles the section's height (≈600px below the bio) and turns an editorial profile into a personal-brand portal. Stripe's people page does not link to executives' Instagrams. | `index.html:1080–1116`. | Cut to **2** cards (Instagram + Calendly) inline as small chips below the bio, OR move the entire follow block to a separate section. The current placement makes the founder section the page's *exit*. |
| 36 | F-COPY-BIO-LENGTH | LOW | Copy | Bio is 47 Arabic words. Tight. Does the job. WORKS. | `index.html:1039`. | Keep. |
| 37 | F-COPY-EYEBROW-DUP | LOW | Copy | "FOUNDER · المؤسس" (sub-eyebrow line 1034) repeats "المؤسس · DOSSIER · 001 / FOUNDER FILE" (masthead line 1006–1011). Reader is told the section is about the founder four times before the name appears. | `index.html:1003–1014, 1034`. | Drop the sub-eyebrow line — masthead already establishes context. |
| 38 | F-PERF-PHOTO-LOADING | LOW | Perf | Photo is `loading="lazy" decoding="async"` — appropriate for below-fold. WORKS. | `index.html:1019`. | Keep. (Replacement should keep these attrs.) |

---

## Vanity Engineering Assessment

### Requirement-to-Complexity Ratio

**Requirement:** "Show the reader who runs this publication and why his judgment is worth trusting."

**Minimum viable solution:** name + headshot + 2 sentence bio + one external proof point + one follow link. ~12 DOM nodes, no animation.

**Current implementation:** 14 typographic gestures, 3 concurrent animations, 4 follow targets, 6 numeric proof points, ~140 DOM nodes. **RCR = 7/10.** Should be 3/10.

### Top 7 Vanity Findings (ordered by severity)

1. **Self-applied "VERIFIED" badge** (F-VANITY-VERIFIED) — Kill cost: 5 min. Remove element + spin keyframes. V3.
2. **Photo asset (150×150 AI-stylized)** (F-VANITY-PHOTO + F-PHOTO-SUSPECT-AI) — Kill cost: 1–2 hours (actual photo session) + 15 min (srcset). V3.
3. **Quadruple Favikon claim** (F-VANITY-AUTHORITY-DUP) — Kill cost: 30 min. Delete `.fnd3a__authority` block, leave stats grid. V2.
4. **Triple masthead stamp (DOSSIER 001 + FOUNDER FILE + date)** (F-VANITY-DOSSIER + F-VANITY-DOSSIER001) — Kill cost: 10 min. V2.
5. **Vertical marquee rail** (F-VANITY-RAIL) — Kill cost: 10 min. Remove element + keyframes. V1.
6. **"NETWORK REACH 1.5M+" double-count** (F-VANITY-NETWORK) — Kill cost: 5 min. V2.
7. **FIG. 01 caption** (F-VANITY-FIG01) — Kill cost: 2 min. V1.

### Vanity Debt Estimate

**~5 person-hours/month** of cognitive overhead across designers/devs revisiting this section to "make it more editorial" — every iteration adds another stamp/badge/number rather than removing one. Plus a credibility tax on every reader who recognizes the templates being borrowed.

### The Hard Question

> **If a reader saw this section blank — just `بدر شاكر`, two sentences, one photo, one Favikon link, and one "احجز جلسة" — would they be more or less likely to trust him?**
>
> Answer honestly: more likely. The current implementation is louder than the founder needs to be. Quiet is the last luxury.

---

## Kill Criteria for the Founder Section

**Tier 1 — Hard Kill (remove on sight):**
- Spinning verified badge (F-VANITY-VERIFIED)
- DOSSIER · 001 stamp (F-VANITY-DOSSIER001)
- Vertical marquee rail (F-VANITY-RAIL)
- Authority-cards block (F-VANITY-AUTHORITY-DUP)
- Sub-eyebrow line (F-COPY-EYEBROW-DUP)

**Tier 2 — Review trigger (must justify or cut):**
- Network reach 1.5M card (F-VANITY-NETWORK)
- "FIG. 01" caption (F-VANITY-FIG01)
- Self-attributed pull quote (F-VANITY-QUOTE)
- Section colophon (F-COLOPHON)

**Tier 3 — Soft-go (must earn its place):**
- Real portrait photo at ≥1200×1500 (F-VANITY-PHOTO + F-PHOTO-SUSPECT-AI)
- Bio max-width capped + Arabic text-align start (F-TYPO-BIO-JUSTIFY + F-TYPO-BIO-LINE)
- Follow grid reduced to 2 cards (F-VANITY-FOLLOW-GRID)
- Crimson uses reduced from 13 to 3 (F-COLOR-CRIMSON-LOAD)

---

## What Works (Keep)

- Semantic HTML: `<h2>`, `<dl><dt><dd>`, `<blockquote><footer>`, `aria-labelledby`. (F-A11Y-DL, F-A11Y-BLOCKQUOTE, F-A11Y-H2)
- Mobile rebuild: rail-hidden, frame-centered max 420, verified scaled to 64/56. (F-MOBILE-PORTRAIT)
- Reduced-motion guard covers all four animated targets in this section. (`v3.css:1144`)
- Tokenized colors and fonts — no hardcoded values found in `.fnd3a__*` rules.
- Stats grid uses `font-variant-numeric: tabular-nums`-equivalent via `--font-ar-display 800` — clean numerics.
- Bio strong tag highlights `#6` in crimson — single-color emphasis is correct.

---

## Phased Plan

### Phase 1 — Critical (kill the vanity)
Estimated effort: **2 hours**. Cut: spinning verified badge, DOSSIER 001, rail marquee, authority-cards block, sub-eyebrow, FIG. 01, network-reach card, AUTH SCORE card, colophon. **Impact:** removes ~50% of section markup and 3 animations. The section gets **shorter, quieter, more credible**.

### Phase 2 — Refinement (typography + photo)
Estimated effort: **1 day** (waiting on photo). Replace 150×150 photo with real 1600×2000 portrait + srcset. Set bio text-align start, max-width 62ch, line-height 1.75. Wrap mixed-script tagline EN segment in `<bdi>`. Drop the gradient on the name (or commit to ink→crimson). Re-order follow grid: Calendly first.

### Phase 3 — Polish
Estimated effort: **2 hours**. Better photo alt. Aria-labels on follow cards. Reduce crimson uses to 3. CTA-out-of-grid on mobile. Test all changes against reduced-motion + 480 + 768 + 1440.

---

## 200-word Summary

The FOUNDER section has a competent editorial chassis — masthead, two-column 5/6 grid, captioned portrait, pull quote, stats, follow grid — and the recent fixes (name unclipped, dropcap removed, mobile rebuild) hold. But the section over-performs the role it borrows: a NYT contributor page does not stamp itself "DOSSIER 001," does not spin a self-applied "VERIFIED" badge, does not run a vertical marquee next to the headshot, and does not state the same Favikon ranking four times in 1,663 px. The most damaging issue is the photo: 150×150 px (7.7 KB) AI-stylized illustration upscaled into an editorial frame, which directly contradicts the editorial gesture the layout is making. The second most damaging issue is the self-applied verified badge, which inverts trust — institutional credibility is conferred, never claimed. The third is repetition: stats card, authority card, tagline, and follow card all surface the same Favikon ranking, which drains it. Cut the verified badge, DOSSIER 001, rail marquee, authority-cards block, and FIG. 01; replace the photo with a real one ≥1200×1500; cap bio width; reduce crimson uses from 13 to 3. After Phase 1+2 the section is shorter, quieter, and credible at the bar of *The Atlantic* staff bios. RCR drops from 7 to 3.
