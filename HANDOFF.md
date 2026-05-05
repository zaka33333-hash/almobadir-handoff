# Almobadir · Session Handoff Brief

> **Paste this entire file as the first message in a new Claude Code session.**
> The new session has zero memory of the prior work. Everything it needs is here.

---

## 0 · Identity & operating posture

You are working alongside **Badr Shaqer** — founder of Almobadir, a **pan-Arab / MENA** business + finance media network (~3.3M followers across 11 active social handles, AR primary + EN mirror). His brother **Zakariae** has been the day-to-day project manager / forwarder. You will mostly be talking to Zakariae; Badr surfaces for strategic decisions.

**The brand is nationality-neutral on purpose.** Public-facing copy never anchors to a single country. See §2 for the Public-vs-Internal split.

Operating manual (cannot be relaxed):

- **Reply in English** even though customer-facing copy is Arabic. Don't switch.
- **Direct over diplomatic.** No preamble ("Great question!", "I'll start by..."). No grovelling after a mistake — one sentence acknowledgment, then fix and move on.
- **10/10 quality bar.** Every claim cites evidence (file:line, curl output, screenshot, computed style, schema validator result). If you can't verify, say "not verified" and explain what you'd need.
- **Untranslatability test.** Could the choice equally serve a SaaS, fintech, or fashion brand with the brand swapped in? If yes, you're off-brief. Almobadir is a media network with weekly publishing rhythm — choices must be specific to that.
- **No safe defaults.** First-pass output should already be brand-signature level — apply the documented signature on iteration #1, not iteration #3.
- **Default to elevating, not patching.** When the partner shares code or content, the implicit ask is "is this 10/10?" — not "are there bugs?"
- **Surface known gaps.** If a half-fix exists, name it explicitly in the response. Never let it slip past unmentioned.
- **Verify before claiming done.** Push success ≠ user success. Curl the live URL, screenshot, paste the rendered output.

### Skill routing

At the start of any substantive request, invoke the **skill-router** skill (or whichever skill-discovery skill is installed) to identify which other skills apply. Pick **1–3 max per turn**. Likely-relevant for this project:

- `verification-before-completion` (after every task closeout)
- `engineering:code-review` (before each commit)
- `accesslint-audit-and-fix` (UI changes)
- `schema-markup` (JSON-LD work)
- `marketing:seo-audit` (SEO emergency questions)
- `subagent-driven-development` (parallel independent tasks)
- `using-git-worktrees` (risky changes)
- `condition-based-waiting` (deploy verification cycles)

---

## 1 · Read these files before doing anything

In this order. The first 5 are the canonical strategy. The website source is the working surface.

```
audits/canonical/PUNCH-LIST.md          ← original forensic design audit (283 lines)
audits/canonical/WOW-BLUEPRINT.md       ← visual signature execution plan (697 lines)
audits/canonical/CONTENT-PACK.md        ← THE master strategy doc (1,379 lines) — read most carefully
audits/canonical/audit-seo.md           ← SEO emergency findings (852 lines)
audits/canonical/audit-brand-messaging.md ← 28 brand drifts catalog (881 lines)

website/index.html                       ← AR homepage source (~2,124 lines)
website/en/index.html                    ← EN homepage source
website/assets/v3.css                    ← main stylesheet (~1,461 lines)
website/assets/v3.js                     ← main JS (~631 lines)
website/assets/newsletter.js             ← newsletter subscribe abstraction
```

If a file > 5KB, read surgically (Grep + Read with offset). Don't dump full contents into context.

---

## 2 · The brand · 90-second briefing

### Public-facing positioning (use this in any external-touching copy)

- **Almobadir / المبادر** = Arabic word meaning "the initiator / pioneer"
- **Founder:** Badr Shaqer (also transliterated Shaker / Chaker)
- **Founded:** 2022
- **Reach:** ~3.3M followers · 25M monthly views · 11 active channels (7 Instagram + 4 TikTok)
- **Brand promise:** «محتوى يغنيك عن دورات كاملة» — "content that replaces full courses"
- **Audience:** pan-Arab / MENA business + finance + self-development readers
- **Agency:** Almobadir Media — the executional arm. 143+ clients served. Calendly: `calendly.com/badrshaqer_consulting/almobadirmedia`
- **Consulting:** Badrshaqer.com — sessions and transformation packs
- **MENA-test for any line of copy:** would the same line work whether the reader is in Cairo, Casablanca, Riyadh, Amman, or Dubai? If no, rewrite.

### Internal context only — DO NOT surface externally

These facts are operational reality and must inform decisions, but they never appear in published copy, the about page, the press kit, or partner decks.

- **Founder origin:** Maghrebi (Moroccan, born 1994). Badr stays nationality-neutral on purpose — never name his country externally.
- **Operational base:** Riyadh-based since 2018. Where the team works from. The site footer / about page does not name a country.
- **Pricing strategy:** premium tier is calibrated to Saudi purchasing power, but framed in regional / value terms in copy — never "Saudi-priced".
- **Saudi-specific data points** (e.g., the 22.9% Saudi SME-to-GDP gap referenced in the rolled-back v2 essay) can appear as **examples or case data inside content**, never as the brand's anchor.
- **Legal jurisdiction:** privacy policy is Saudi PDPL + GDPR compliant; terms use Saudi governing law. This is operational legal scaffolding, not positioning — don't surface in marketing.

The CONTENT-PACK has the canonical 28-word AR + EN positioning statement. Do not improvise alternates.

> **Known gap (open task):** the live `/about/` and `/en/about/` pages currently lean on the Riyadh + Morocco→Riyadh trajectory in their copy. Audit and rewrite both pages against the public-facing positioning above. See §3 "What's NOT wired yet" for tracking.

### The 11 active handles + verified follower counts (Q2 2026)

| Handle | Platform | Lane | Followers |
|---|---|---|---|
| @shalnack | Instagram | Creative direction (the wedge: Islamic guidance, wisdom, design) | 672K |
| @almobadir_mindset | Instagram | Self-development | 478K |
| @almobadir_flousak | TikTok | Financial freedom (TikTok > IG anomaly) | 450K |
| @almobadir | Instagram | Business + finance flagship | 363K |
| @badrshaqer | Instagram | Founder POV | 317K |
| @almobadir | TikTok | Business + finance | 267K |
| @shalnack | TikTok | Creative direction | 248K |
| @almobadir_flousak | Instagram | Financial freedom | 202K |
| @almobadir_mindset | TikTok | Self-development | 150K |
| @almobadir_business | Instagram | Business + marketing | 124K |
| @almobadir_media | Instagram | The agency | 57.7K |

---

## 3 · Project state · what's deployed

### Live URLs

- **Current production:** https://zaka33333-hash.github.io/almobadir-website/
- **Future production:** https://almobadir.com (DNS being wired at Hostinger — see `pending` section below)
- **EN mirror:** https://zaka33333-hash.github.io/almobadir-website/en/

### Git state

```
Repo:       https://github.com/zaka33333-hash/almobadir-website
Branch:     main
HEAD:       11d7f77  (Wire production domain · add CNAME file)
Baseline:   ae72b8b  (Pre-launch hardening · P1 batch — 404 + about + press kit)
Tag:        publication-v2-attempt → c9c9572  (a Stratechery-style restructure the founder rolled back)
```

### Rollback paths

- To restore the brochure-front baseline: `git reset --hard ae72b8b && git push --force origin main` (only with explicit user confirmation)
- To recover the publication-v2 work: `git checkout publication-v2-attempt -- <file>` (selectively pull files like `assets/pub.css` or `index.html`)

### Pages currently live

```
/                       — AR homepage (brochure-front structure)
/en/                    — EN mirror
/about/  /en/about/     — founder + Riyadh + 2022 + Morocco→Riyadh trajectory
/press/  /en/press/     — press kit (EN-primary, AR mirror)
/privacy/ /en/privacy/  — Saudi PDPL + GDPR-compliant privacy policy
/terms/  /en/terms/     — terms of use (Saudi governing law)
/newsletter/thanks/  /en/newsletter/thanks/  — post-subscribe confirmation
/newsletter/unsubscribe/ /en/newsletter/unsubscribe/ — opt-out
/404.html               — branded fallback
/robots.txt /sitemap.xml /llms.txt — SEO foundation (10 URLs in sitemap)
/CNAME                  — points at almobadir.com (active when DNS lands)
```

### What's wired

- **Plausible analytics** script tag on all main pages (no-op until `almobadir.com` is added at plausible.io)
- **Cookie consent banner** (`assets/cookie-banner.js`) — privacy-first, defaults to deny, AR/EN auto-detect
- **Newsletter subscribe abstraction** (`assets/newsletter.js`) — ESP-swappable in 1 config line; currently in **simulation mode** (signups queue to localStorage, no real ESP wired)
- **Motion library** (motion@12.38.0) loaded via importmap from esm.sh — no build step
- **JSON-LD schema** — Organization + Person + WebSite + Newsletter entities, all with `@graph` references
- **OG image** at `/assets/og/og-1200x630.png` (1200×630 PNG generated programmatically)
- **`og:image`, `twitter:image`, `og:image:alt`** wired on AR + EN homepages
- **Lang-toggle morph** — clicking EN fades the wordmark + scales it before navigation
- **⌘K command palette** with 18 indexed entries (NAV / IG / TT / LANG groups)
- **Page scroll progress** crimson bar (RTL-aware)

### What's NOT wired yet (the open list)

| Item | Status | Blocked on |
|---|---|---|
| **DNS at Hostinger** | Records given to founder | Founder action (add 4 A + 4 AAAA + 1 CNAME records) |
| **GitHub Pages custom domain** | CNAME file **reverted** in commit `37a247c` while DNS pending — re-add atomically with the DNS cutover | Founder needs to set in repo Settings → Pages → Custom domain after DNS lands |
| **HTTPS for almobadir.com** | Auto via Let's Encrypt | Pending DNS resolution |
| **Nationality-neutral audit on /about/ and /en/about/** | Pages currently lean on Riyadh + Morocco→Riyadh trajectory; must be rewritten pan-MENA per §2 | Founder MCQ Round 2 answer (or apply default rewrite) |
| **Newsletter ESP** | Simulation mode | Awaiting founder's pick (Buttondown / Resend+CF Workers / defer) |
| **Plausible dashboard** | Script tag deployed | Founder needs to register `almobadir.com` at plausible.io |
| **MBA-Marketing claim** | Founder said KEEP | Pending verifiable LinkedIn Education entry |
| **7 IG handles → Business account** | Per audit | Founder phone action (~5 min each) |
| **4 IG handles missing bio link** | Per audit (1.72M followers can't leave Instagram for the website) | Founder phone action |
| **Real founder photo** | Current is AI-generated 150×150 | Photo session required |
| **Real Issue 1 essay** | If publication-style is chosen | Founder writes |

---

## 4 · The latest user signal · why this handoff exists

The founder's verdict on the current state:

> **"It's around 4/10. I need it to be 10/10."**

Then we built a publication-front restructure (commit `c9c9572`, preserved as tag `publication-v2-attempt`) — a Stratechery-style architecture with an editorial mast-head, Issue 0 essay on the Saudi SME-to-GDP gap (الفجوة 22.9%), curated network desk, and inline newsletter strip.

**The founder rolled it back.** No reason given. We restored the v1 brochure structure.

So: **neither v1 nor v2 hits 10/10 in his eyes.** Until we know which signal he was rejecting (structure / voice / visual / content / "all of the above"), every next move is guesswork.

To unblock this we drafted **`BADR-MCQ.md`** (in this folder) — 11 multiple-choice questions + 3 open-ended + 5 personal tasks. **Get this answered first.** It's the highest-leverage unblock.

---

## 5 · What's been done · 30+ moves shipped this project

The design work that happened across the prior session, ordered roughly by ship time:

```
1.  Hero wordmark construct (motion@12 spring entrance + hover glow)
2.  KPI panel cascade (counter roll + sparkline draw + bar lift)
3.  Network grid hover spotlight (sibling-aware spring defocus)
4.  Method numeral spring entrance + media-card layout fix
5.  Compact network cards (3× height reduction)
6.  Architectural Arabic drop-cap (founder bio + method bodies)
7.  Lang-toggle masthead morph
8.  ⌘K Command Palette
9.  Footer debossed wordmark on cream sub-region
10. Manifesto ink-bleed SVG filter
11. Hero → Manifesto Z-dolly scroll handoff
12. Content cards "deal" entrance (scroll-driven)
13. Page scroll progress indicator
14. Newsletter form ↔ preview reciprocity
15. Hero accent-word kashida-pull (faked without Idris Variable license)
16. SEO emergency: robots.txt, sitemap.xml, llms.txt, JSON-LD
17. Hard-paths fix on lang-toggle navigation (relative paths)
18. EN page rendering bug fix (mix-blend-mode washing)
19. Newsletter v1 — subscribe() abstraction + AR/EN thanks + unsubscribe
20. Arabic-only newsletter — disclosure on EN surfaces, schema fix
21. OG image (1200×630 brand-signature share preview)
22. Plausible analytics on AR + EN
23. Privacy + Terms + Cookie banner (4 pages + 1 module)
24. /about/ + /en/about/ pages (founder credibility unblocker)
25. /press/ + /en/press/ press kit
26. Branded 404 page
27. CNAME file → almobadir.com (production domain wire-up)
28. (Reverted) Publication-front v2 — Issue 0 essay + Stratechery structure
```

---

## 6 · Strategic decisions still open · waiting on Badr's MCQ

These come from `audits/canonical/CONTENT-PACK.md` Part XI and the brand-messaging audit. Reproduced in `BADR-MCQ.md`:

1. **The site's #1 job** — agency Calendly bookings vs newsletter signups vs founder writing vs press kit vs social-handle hub
2. **Founder visibility level** — brand-led / founder-led / hybrid
3. **Content rhythm commitment** — 1 essay/week, 1 every 2 weeks, or no original content
4. **Visual feel** — cinematic editorial vs premium minimal vs founder-essay vs bold magazine
5. **Tone register** — authoritative / human / urgent / craft
6. **MBA-Marketing claim** — REMOVE / KEEP+verify / REWRITE
7. **Morocco→Riyadh trajectory** — name openly / Riyadh-only / soft mention
8. **Real founder photo timeline** — 30 days / 90 days / use AI for now
9. **Newsletter ESP path** — Buttondown / Resend+CF Workers / defer
10. **Book summaries app** — build / test first / park
11. **Habits app** — eventually / never / park

Plus three open-ended:
- Three reference URLs that mean "10/10 to me"
- Success metric 90 days post-launch
- Anything explicitly refused on the site

---

## 7 · The 8 launch-block tasks (audit-flagged · founder-action-required)

| # | Task | Time | Unblocks |
|---|---|---|---|
| 1 | Hostinger DNS records (4 A + 4 AAAA + 1 CNAME) | 8 min | The whole almobadir.com canonical chain |
| 2 | GitHub Pages → Settings → Custom domain → `almobadir.com` → Save → Enforce HTTPS | 1 min | The CNAME file activates |
| 3 | Sign up at plausible.io → add `almobadir.com` | 3 min | Real analytics |
| 4 | Sign up at buttondown.com → paste username in `assets/newsletter.js` line 30 | 5 min | Newsletter goes live (currently captures to localStorage only) |
| 5 | Switch 7 IG handles from Creator → Business account | 35 min | DM auto-replies, analytics, tap-to-act |
| 6 | Add bio link on 4 IG handles where `bio_links: []` | 4 min | 1.72M followers can finally leave Instagram for the website |
| 7 | Add LinkedIn Education entry (verifies the MBA bio claim) | 6 min | Closes credential leak Drift 4 |
| 8 | Approve / answer `BADR-MCQ.md` | 8 min | Everything design-side downstream |

---

## 8 · Things you do NOT do (anti-patterns)

- ❌ Add a blog at `/blog/` (would launch with 0 posts and look worse than no blog)
- ❌ Add live chat widget (Intercom / Crisp / Drift) — wrong channel for Arab / MENA B2B
- ❌ Newsletter popup / scroll-trigger modal (violates motion-signature anti-pattern list)
- ❌ A third language beyond AR + EN
- ❌ Members-only login / website paywall (newsletter handles the gated relationship)
- ❌ Auto-generated AI summaries on the website (planned as separate `apps.almobadir.com` future product, not v1 web)
- ❌ Push to remote without explicit instruction
- ❌ `git reset --hard`, `git push --force`, or any destructive op without confirming the target
- ❌ Mention TodoWrite reminder system to the user (plumbing, not user-facing)
- ❌ Add emojis to deliverables unless explicitly requested
- ❌ Re-raise issues already closed without first verifying current state

---

## 9 · Verification rules

For every deploy that touches user-visible output:

```
1. Edit → read back the changed file lines
2. git add <specific files> + commit with concern in subject
3. git push origin main
4. Wait for Pages rebuild (use Bash run_in_background until-loop on the new asset URL)
5. curl -sI the live URL → confirm new Last-Modified + new ETag
6. curl the rendered HTML → confirm the change is in the served bytes
7. Playwright navigate + screenshot → confirm visual rendering
8. If structured data: paste the @graph and confirm Schema.org @types resolve
9. Mark the todo complete, update status table
```

Don't claim done at step 3. That's deploy success, not user-visible success.

---

## 10 · Verification commands you can run today

```bash
# Confirm the live site is up
curl -sI https://zaka33333-hash.github.io/almobadir-website/

# Confirm DNS for production domain (will return GitHub IPs once Hostinger propagates)
dig +short almobadir.com

# Confirm production domain serves
curl -sI https://almobadir.com/

# Schema validation (paste resulting JSON-LD into a parser)
curl -s https://zaka33333-hash.github.io/almobadir-website/ | grep -A 200 'application/ld+json'

# OG image check
curl -sI https://zaka33333-hash.github.io/almobadir-website/assets/og/og-1200x630.png

# Newsletter assets check
curl -sI https://zaka33333-hash.github.io/almobadir-website/assets/newsletter.js
```

---

## 11 · What to do FIRST in the new session

1. **Invoke skill-router** (or your skill-discovery skill). Pick 1-3 skills max.
2. **Read the 5 canonical audit docs** in `audits/canonical/`. Surgical reads, not full dumps.
3. **Verify current live state** with the curl commands above. Confirm git HEAD matches `11d7f77`.
4. **Read `BADR-MCQ.md`** in this folder. If it has answers in it (look for "Q1: ..." style replies), execute on those. If not, it's still waiting on the founder.
5. **Check the 8 founder-action tasks** above for status — has DNS landed yet? Has Buttondown been wired?
6. **Reply to the user with a status snapshot** + the single highest-leverage question or task they could do in the next 10 minutes.

If you're tempted to ship a big visual change without `BADR-MCQ.md` answers — **don't**. The previous session shipped a publication-front restructure that the founder rolled back, and we're now blocked on understanding why. Get the MCQ answered before any structural change.

---

## 12 · One last thing

The previous session left two specific commits the founder may want preserved or restored:

- **`ae72b8b`** — the v1 brochure baseline (currently live)
- **`publication-v2-attempt` tag** → commit `c9c9572` — the Stratechery-style restructure that was rolled back

If the MCQ comes back with "yes I want a publication front", the v2 work can be partially restored:

```bash
# To pull just the publication CSS:
git checkout publication-v2-attempt -- assets/pub.css

# To pull the entire v2 homepage:
git checkout publication-v2-attempt -- index.html
```

The Issue 0 essay (~720 Arabic words on the Saudi SME-to-GDP gap) was AI-written, not founder-written. If reviving v2, the essay should be replaced with founder-written content before going live.

---

**You now have full context. Begin by reading the 5 canonical audit docs, then reply to the user with a status check + the next highest-leverage action.**
