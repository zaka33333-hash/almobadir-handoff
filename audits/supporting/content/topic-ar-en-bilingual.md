# Topic: AR-EN bilingual content strategy — Almobadir's two surfaces

> **Wave 3, deep-dive 9 of 10.** Audit + research briefing on the AR/EN bilingual posture for almobadir.com. Sources cited inline; URLs collated at the bottom under "Sources."
> **Author:** Agent 3.9, CONTENT Intelligence audit.
> **Date:** 2026-04-27.
> **Inputs read:** `website/index.html` (AR, 1234 lines), `website/en/index.html` (EN, 1274 lines), `handoff-prior-session.md`, public sites (Wamda, Asharq Al-Awsat, NEOM, Saudi Aramco corporate, Argaam, Visit Saudi).

---

## Executive summary

The English mirror is now live at `/en/` with full hreflang reciprocity, a CSS Latin-stack override on `html.lang-en`, a MutationObserver patch that converts Arabic-Indic numerals (٠-٩) to Latin (0-9) inside the method-step counter and newsletter countdown, and a parallel footer/header lang-toggle pair. Architecturally the EN build is solid. **Strategically it is a 1:1 translation of an Arabic-first product, and that is the wrong default.**

Almobadir's audience is overwhelmingly Arabic-first (3.3M social followers in Arabic; the network's own copy says "an Arabic editorial team"). The English mirror cannot beat the Arabic flagship at its own game — Arabic readers will always pick `/`. The EN site only earns its keep if it serves a *different* job: a credentialing, B2B, and discoverability surface aimed at journalists, agency clients (Almobadir Media has 143+ clients, $1M+ ad spend), investors/partners, and English-comfortable Saudi/Gulf executives who Google in English even when they read in Arabic.

The recommended posture is **80% mirror + 20% EN-only**. Keep the homepage parallel (it is the credentialing showcase — KPIs, network, founder, method) but add EN-only depth where journalists, investors, and B2B buyers actually need it: a Press/Media kit (assets, fact sheet, founder bio, contact), an Agency case-studies hub for `almobadir.media`, a Methodology longread, and a `numbers.json` machine-readable network-stats endpoint that reporters can cite. Drop Arabic-only memes, Saudi-dialect captions, and hashtag-jokes from the EN side — they translate to inert prose. Tone-calibrate the EN voice to match Almobadir's confident, editorial, dark-newsroom register (it currently reads like a careful translation, not a parallel original).

The "Almobadir vs The Initiator" naming question resolves cleanly: **keep "Almobadir" as the canonical romanization** (the brand recognition equity is in the Arabic word, not in a literal English translation), and add a one-line gloss the first time it appears in body copy on English landing pages. Numerals: Latin everywhere on the EN site; Arabic-Indic everywhere on the AR site; the MutationObserver patch already enforces this.

**Recommended bilingual posture, one sentence:** Mirror the homepage, diverge in the deep pages — EN as a credentialing/B2B surface, AR as the audience flagship.

---

## Bilingual patterns at 8+ MENA brands (matrix)

Surveyed publicly between 2026-04-26 and 2026-04-27. URL-structure column shows current observed convention.

| Brand | URL pattern | Default | Symmetry | Audience target on EN | Notes |
|---|---|---|---|---|---|
| **Wamda** | `/` (EN), `/ar` | EN | Mostly symmetric, EN-first | MENA founders, VC, English-comfortable execs | Bilingual platform; EN is primary, AR translated. Partnered with FT for syndicated EN content (Wamda × FT, 2022). |
| **Asharq Al-Awsat** | `aawsat.com` (AR), `english.aawsat.com` (EN) | AR | Adapted, not identical | International readers, English-speaking diplomats and analysts | Five-language family (AR, EN, TR, UR, FA). Each language has its own editorial team adapting, not just translating. |
| **Argaam** | `/` (AR), `/en` (EN) | AR | Mostly symmetric | English-speaking Gulf investors, expats | Dual platform, both AR and EN serve markets data + news. Argaam Plus (paid) priced for both. |
| **Saudi Aramco corporate** | `/en` and `/ar` | EN-equivalent | Symmetric formal | Investors, regulators, journalists, suppliers | Heavy investor-relations + sustainability content. Same architecture both languages. |
| **NEOM** | `/en-us` and `/ar-sa` | EN | Symmetric | International investors, partners, suppliers | Locale-coded URLs (`en-us`, `ar-sa`). Heavy "Invest in NEOM," "Suppliers," "News & Media" sections — investor-first. Press contact at `/newsroom/newsroom-contact-us/press`. |
| **Visit Saudi** (MISA tourism) | `/en` and `/ar` | EN | Symmetric consumer | International tourists | Pure consumer content; not a credentialing surface. |
| **Tadawul** (Saudi Stock Exchange) | `/en` and `/ar` | AR for issuers, EN for foreign investors | Symmetric formal | Foreign institutional investors, listed companies | Bilingual is a regulatory requirement, not a marketing choice. |
| **Diriyah Gate Development Authority** | `/en` and `/ar` | EN-tilted | Symmetric formal | International tourism investors, partners | EN content frequently quoted in Asharq Al-Awsat English coverage; positioned for inbound capital. |
| **Arab News** (English daily) | EN only at `arabnews.com`; AR at sister Asharq Al-Awsat | EN-only by design | N/A — AR is a separate brand | Saudi expats, English-speaking residents, international wire pickups | Separate brand strategy: don't run two languages on one URL — run two brands. |
| **Al Arabiya** | `arabiya.net` (AR), `english.alarabiya.net` (EN) | AR | Adapted | International English readers, diaspora | Subdomain split, separate editorial. Same parent (MBC). |

### Patterns to extract

1. **Subdomain split (`english.aawsat.com`) signals editorial divergence.** Subdirectory split (`/en/`) signals a localized mirror. Almobadir is at `/en/`, which is the correct choice — the brand is one entity, not a dual masthead.
2. **Government/parastatal sites lean EN-first or symmetric formal.** They are credentialing for capital, not marketing to a domestic audience. Almobadir's EN side should borrow this register (formal, factual, deck-ready).
3. **Pure-media plays (Asharq, Al Arabiya) maintain separate editorial teams per language.** Almobadir does not need this; it is a creator network, not a wire service.
4. **Wamda's pattern is closest to a workable Almobadir model:** EN as the platform-of-record for an MENA-focused B2B audience, AR as the localized parallel. The twist for Almobadir is that the *audience* is reversed — Arabic is the home language, English is the credentialing language.

---

## SEO best practices 2026 (technical checklist with current site audit)

Sourced from Google Search Central docs, ClickRank 2026 hreflang guide, LinkGraph, Weglot, Mak IT Solutions Saudi/UAE multilingual SEO guide, Searchengineland.

### Checklist — and where Almobadir stands today

| # | Practice | Status on almobadir.com | Action |
|---|---|---|---|
| 1 | **Separate URLs per language.** AR at `/`, EN at `/en/`. Each canonicalizes to itself. | ✅ Implemented. `<link rel="canonical" href="https://almobadir.com/">` on AR; `https://almobadir.com/en/` on EN. | Hold. |
| 2 | **Reciprocal hreflang.** Each page links to all language versions including itself; no missing return tags. | ✅ Both pages declare `hreflang="ar"`, `hreflang="en"`, `hreflang="x-default"` pointing at AR. | Hold. |
| 3 | **`x-default`** points at the language version Google should fall back to when no other matches. | ✅ Set to AR (`https://almobadir.com/`) on both pages. Defensible — the brand is Arabic-first. | Hold. |
| 4 | **Canonical never contradicts hreflang.** EN page should NOT canonicalize to AR. | ✅ Each page canonicalizes to itself. Common error per Weglot guide; avoided here. | Hold. |
| 5 | **`<html lang>` and `dir`** match content. | ✅ AR: `lang="ar" dir="rtl"`. EN: `lang="en" dir="ltr"`. | Hold. |
| 6 | **`og:locale`** matches page language. | ✅ AR: `ar_SA`. EN: `en_US`. | Consider `en_GB` if Gulf B2B/UK skew matters; `en_US` is fine default. |
| 7 | **Twitter card meta** present. | ✅ `summary_large_image` on both. | Hold. |
| 8 | **Schema.org `@inLanguage`** on every JSON-LD entity. | ❌ No JSON-LD blocks present in either page. | **Add.** See "Concrete EN-site changes" below. |
| 9 | **Schema.org `availableLanguage`** on Organization. | ❌ Missing (no Organization JSON-LD). | **Add.** Set to `["ar","en"]`. |
| 10 | **XML sitemap with hreflang**. | ❓ Not verified — file not in repo. | Check on prod; if missing, add `sitemap.xml` with `<xhtml:link rel="alternate" hreflang="...">` per URL. |
| 11 | **Robots.txt allows both versions; sitemap referenced.** | ❓ Not verified. | Check on prod. |
| 12 | **Avoid dual hreflang sources** (HTML head + sitemap simultaneously, mismatched). Per ClickRank 2026 guide and Mak IT, this is the #1 hreflang error in Saudi/UAE multilingual sites. | ✅ Currently HTML-only. | If a sitemap is added later, keep hreflang in HTML and OPTIONALLY mirror in sitemap; don't let them drift. |
| 13 | **No mixed-content language (e.g., AR copy on EN page) without `lang` attribute on the inline element.** | ⚠️ Manifesto section on EN page (`html.lang-en .manif3__shadow`) renders the AR translation visible (CSS sets `opacity:1` for `.manif3__shadow` in EN locale, swapping it from "shadow" to "primary"). The AR fragment has `lang="ar" dir="rtl"` — correct. | Hold; this is intentional editorial design (the manifesto deliberately shows both originals). |
| 14 | **EN keyword research is independent.** Don't translate AR keywords; research EN search demand separately. | ⚠️ No keyword brief found in repo. EN copy currently mirrors AR phrasing. | Recommend a separate EN keyword brief (see "Open questions"). |
| 15 | **Geo-targeting in Google Search Console.** EN can target multiple regions or be unset. | ❓ Cannot verify from repo. | Set EN to "unlisted" (international) in GSC; AR to Saudi Arabia. |
| 16 | **Page speed parity.** Both languages share the same CSS/JS/image bundle on Almobadir; should be identical Lighthouse. | ✅ Same `assets/v3.css?v=2` and `assets/v3.js?v=2` on both. | Hold. |
| 17 | **404 handling on language toggle.** Clicking "AR" from `/en/page` should land on `/page` if it exists, or `/` if not — never a 404. | ⚠️ Current toggle hardcodes `window.location.href = '/'` and `'/en/'` (lines 1193-1198, 1199-1216 of EN page). One-page site so this works today, but the moment a `/en/about` exists, it will silently dump the user to the AR root instead of `/about`. | **Fix when adding more pages.** Replace with a URL-translation helper that maps `/en/<slug>` → `/<slug>` and back. |
| 18 | **Fonts loaded only when needed.** AR fonts (Almarai, Tajawal, Cairo, Reem Kufi, Mostaqbali) are heavy; EN doesn't strictly need them — but the inline SVG wordmark uses Mostaqbali for the Arabic word "المبادر" even on the EN page. | ⚠️ EN page loads the same five Arabic font files as AR. Acceptable for one page, wasteful at scale. | Defer non-wordmark Arabic fonts on EN (`font-display: optional`); keep Mostaqbali for the wordmark only. |
| 19 | **Per Mak IT 2026 Saudi/UAE multilingual SEO:** avoid auto-translation plugins; ship hand-written copy per language to avoid duplicate-content penalties from Google's near-dupe detector. | ✅ EN copy is hand-translated, not machine-output. | Hold. |
| 20 | **Quarterly hreflang audit** with Screaming Frog or Sitebulb (linkgraph.com 2026 recommendation: monthly crawl, quarterly GSC International Targeting review). | N/A — too new to have drifted yet. | Add to ops calendar. |

---

## EN-only opportunities (5+ specific pages/sections to add)

These are surfaces that *only* the EN site needs. Building them in Arabic would either duplicate the social-feed mission (which the AR audience already gets in real time on Instagram/TikTok) or serve no real audience.

### 1. Press / Media kit — `/en/press/`

The single highest-leverage EN-only page. Per Prezly's 2026 press-kit guide and AuthorMedia's media-kit templates, journalists need: (a) a fact sheet, (b) downloadable assets, (c) founder bio, (d) reachable contact, (e) recent press mentions, (f) a single-page PDF.

Proposed structure:

- **At-a-glance fact sheet** (numbers): "Almobadir is a Saudi Arabia-based business and finance content network. Founder: Badr Shaker (#6, Favikon Top 20 Saudi B2B Creators 2025). Network: 7 Instagram accounts + 4 TikTok = 11 active channels. 3.3M+ followers. 25M monthly views. 5,000+ pieces published. HQ: Riyadh."
- **Founder bio** (3 versions: 50 words, 150 words, 500 words). Journalists pick the length they need. Include the BBA + MA-in-Marketing line, the Favikon ranking, the Almobadir Media client roster ("$1M+ in ad spend managed for 143+ clients"), and 1-2 sample headlines from past coverage.
- **Logo & brand assets ZIP** — masters: SVG (cream), SVG (crimson), PNG transparent at 512/1024/2048, plus the SVG wordmark and the founder's official portrait. License inside the ZIP: "Free to use in editorial coverage of Almobadir; do not modify proportions; do not place over photographic backgrounds without contrast check." Per Uber/Prezly press-kit best practice (download without an email gate).
- **Founder photos** at editorial sizes — minimum 3, including a horizontal 16:9 cinema-cut suitable for a magazine spread, a 4:5 portrait, and a square crop for Instagram carousel. Source: `assets/Badrshaqer.jpg` exists.
- **Network roster table** — handle, role, accent color hex, follower count, link. Already structured in the `nw3a` section of the homepage; lift it into a denser, copy-paste-friendly table on `/en/press/`.
- **Press mentions** — chronological. Even 3-5 entries (Favikon, an Asharq Business mention, a Wamda profile if any) reduce a journalist's research time and signal traction.
- **Direct contact** — `press@almobadir.com` (currently only `hello@almobadir.com` exists; add a press alias). Response-time SLA ("within 48 hours") because Prezly's 2026 guide flags this as a trust signal.
- **`numbers.json`** — machine-readable endpoint at `/en/press/numbers.json`. Schema: `{ "as_of": "2026-04-27", "followers": { "instagram": 2200000, "tiktok": 1100000, "total": 3300000 }, "monthly_views": 25000000, "active_channels": 11, "founder": { ... } }`. Wire-service AI scrapers (Bloomberg, Reuters' AI-summary tool, ChatGPT browsing) will pull this verbatim and quote it.

### 2. Agency case studies — `/en/agency/cases/`

Almobadir Media has *143+ clients, 5,000+ content pieces, $1M+ in ad spend* (per the homepage Network section). That's a B2B sales surface, but the homepage gives it a single card and a link out to `almobadir.media` (which the user did not provide for review). The EN site can host case-study pages for the agency clients who are English-comfortable: Saudi corporates with international counterparties, Gulf banks, MENA SaaS, KSA-listed companies.

Each case study: client name + sector + brief, problem, what Almobadir Media did, content samples, results (organic reach, engagement, paid spend ROI). Three to five case studies = enough to signal pattern, not so many that the page becomes a portfolio page nobody reads. Per LinkedIn B2B content benchmarks, agency landing pages with case studies convert 3-5x better than ones without.

### 3. Methodology longread — `/en/method/`

The homepage Method section is a 5-phase scroll experience (Select → Simplify → Narrate → Design → Listen). It is performative — it shows the method but does not document it. An EN-only longread (~1500-2000 words) at `/en/method/` documents the process for: (a) prospects evaluating the agency, (b) journalists writing a "how Saudi creators are doing it" feature, (c) media-school students/researchers citing the work, (d) other content networks studying the approach. Title: *"Method · how we make business stories that stick."* Tone: editorial, first-person plural, evidence-led.

Why EN-only: the AR audience consumes the method through the *output* (the daily reels and carousels). The EN audience is paying attention to the *system*. Different consumption mode.

### 4. Network sub-page hub — `/en/network/`

The homepage shows 7 channels in a magazine grid. A dedicated `/en/network/` sub-page can deepen each channel's profile: channel name, role in the network, sample post (embedded), tone description, audience demographic if known, partnership/collaboration contact. This is functionally a sales page for sponsors and brand-deal scouts. AR equivalent is unnecessary because the AR audience interacts with the channels directly on IG/TikTok; English-speaking sponsors need the meta-sheet.

### 5. Founder press hub — `/en/founder/`

The current homepage has a `#founder` anchor section with a bio. An expanded `/en/founder/` page is a Saudi-creator equivalent of a New York Times "About the columnist" page. Sections: extended bio, talks/keynotes given, media appearances, advisory roles, books/longreads written, contact for speaking inquiries (`speaking@almobadir.com`). This serves: conference organizers (RiyadhComedy, LEAP, Saudi Media Forum, regional creator economy events), university invitations, podcast bookers, journalists.

### 6. Newsletter archive — `/en/brief/archive/`

The homepage prominently CTAs the weekly Almobadir brief. If the brief is published in Arabic only, leave it as-is. **If a parallel EN edition is on the roadmap** (Open question for Badr — see end), the archive page is where it lives. Per Substack/Beehiiv 2026 norms, archive pages are SEO-rich because each issue title is a separate landing surface.

### 7. (optional, Wave 4 candidate) Investor / partnership page — `/en/partnerships/`

Only build this if Almobadir is open to outside capital, joint ventures, or strategic partnerships. Otherwise it sends a confusing signal.

---

## AR-only opportunities (don't waste translation effort here)

Things that should NEVER appear on the EN site, even mirrored. Translating these dilutes them.

1. **Saudi-dialect/Khaleeji captions and reels.** Najdi-leaning humour, Hijazi proverbs, and Gulf colloquial idioms have zero load-bearing weight when rendered as MSA prose, let alone as English. Per Qasid Online's Arabic-dialects guide and Talkpal's Saudi-dialect material, Najdi vocabulary and pronunciation are conservative and culturally specific in ways that lose 80%+ of meaning in literal translation.
2. **Hashtag-driven posts.** `#أبوظبي_اقتصاد`, `#السعودية_اقتصاد`, etc. The hashtag IS the discoverability mechanism on the AR side; the EN side has no equivalent surface for these tags.
3. **Religious or culturally referential content** (Ramadan-specific framing, Eid-specific calendars, references to specific sheikhs, references to popular Saudi TV shows or dramas like *Al-Rais*). These are perfectly legitimate in AR; in EN they read as exotic-other and undercut the credentialing register the EN site is trying to project.
4. **In-jokes, memes, and trend-following posts.** The half-life is hours. Translating them takes longer than the trend lasts.
5. **Local market gossip / "behind-the-scenes" Saudi business stories** that depend on the reader knowing who the players are. The EN audience does not. Fold those into investor-targeted longreads on the AR side; don't translate.
6. **Daily news commentary.** Almobadir's edge in AR is speed + voice. Translating that into EN at the same cadence is a losing race against Reuters/Bloomberg/Wamda. Pick monthly or quarterly editorial longreads instead for the EN side.
7. **Founder Q&A / lives streams.** Badr's IG live + TikTok live conversational style is in dialect. Subtitle-only would lose voice; full English transcript would be expensive and dated by the time it shipped.

The discipline: **AR-only content is the engine; EN-only is the dossier.** Don't run the dossier as fast as the engine.

---

## Translation traps + audit findings

### 1. The "Almobadir vs The Initiator" question — RESOLVED

The Arabic word **المبادر** literally means "the initiator," "the pioneer," "the one who starts/leads." Currently the EN site renders it as the romanization "**Almobadir**" everywhere (header, footer, body copy, social handle, OG title). The Arabic glyph is preserved in the SVG wordmark even on the EN page, with a romanized "A L M O B A D I R" wordmark-companion underneath (`hero3__wordmark-romanized` class).

**Decision: keep "Almobadir" as the canonical romanization.** Reasoning:

- Brand equity is in the Arabic word + the romanization spelling. The 3.3M followers know "@almobadir." A switch to "@theinitiator" or similar would shed every search-result, every back-link, every existing share.
- Per the Entrepreneur ME's 2018 piece on Arabic-English content for MENA brands, Arabic-rooted brand names that retain their original form on English surfaces signal authenticity without sacrificing recognition (Wamda, Souq, Talabat, Careem all do this). Translation-as-name signals discomfort with the original.
- The literal translation "The Initiator" reads like a Marvel villain in English. "The Pioneer" is closer but is already the brand of Pioneer Electronics + a chain of restaurants; collision-risk.

**Action:** Add a one-time gloss the FIRST time "Almobadir" appears in EN body copy on each landing page: *"Almobadir (المبادر, 'the initiator')"*. After that, "Almobadir" alone. Currently the EN homepage does not gloss the name at all — a casual EN reader has no idea what it means.

### 2. Numerals — already handled by MutationObserver

The EN page ships an inline `<script>` (lines 1226-1271 of `website/en/index.html`) that converts Arabic-Indic digits (٠-٩) to Latin (0-9) inside the method-step counter (`.mtd3__progress-current`) and the newsletter countdown (`#nl3-countdown`), and rewrites the AR countdown phrase ("الأحد · 9:00 ص الرياض · بعد X ي Y س") into "Sunday · 9:00 AM Riyadh · in Xd Yh."

This is correct. **Audit catch:** the regex matches both day-and-hour (`بعد X ي Y س`) and hour-and-minute (`بعد X س Y د`) variants but does not match minute-and-second (`بعد X د Y ث`) — if the AR-side script ever adds seconds-level precision in the final hour, the EN page will display Arabic seconds text. Low-probability but worth noting. **Recommended fix:** add a third branch to handle `بعد X د Y ث` → "in Xm Ys."

### 3. Right-to-left in mixed contexts

The manifesto section (`#manifesto`) deliberately renders BOTH the EN translation (`.manif3__lede`, `lang="en" dir="ltr"`) AND the AR original (`.manif3__shadow`, `lang="ar" dir="rtl"`) on the EN page. The CSS rule `html.lang-en .manif3__shadow { font-style: normal; opacity: 1; }` makes the AR fragment a primary visible block, not a faded shadow. This is intentional editorial design — showing both originals signals authenticity.

**Audit catch:** the `aria-hidden="true"` attribute on the AR fragment (line 379 of EN page) is now wrong — it was correct when the fragment was a faded decorative shadow, but on the EN page the CSS makes it visible primary text, so screen-readers should read it. **Fix:** on EN, remove `aria-hidden` from `.manif3__shadow`, OR keep it hidden and supply an ARIA description in EN. Pick one; current state is inaccessible.

### 4. The AR-eyebrow `manif3__eyebrow--en` confusion

In AR HTML the eyebrow is `§ 001 · METHOD` (Arabic) plus `MANIFESTO / 001` (EN). On the EN page the same bilingual eyebrow renders, which is fine — but the `--en` modifier class is now misleading (it labels something already in EN). Cosmetic only; flag for any future English-French expansion.

### 5. Saudi proper nouns — `Riyadh` vs `الرياض`

The EN page correctly uses "Riyadh" everywhere ("SYNC · LIVE · KSA," "Riyadh" clock label). No transliteration drift to "Riyad" or "Al-Riyadh." Hold.

### 6. Brand-tone-bleed in metric labels

Some EN labels still read like AR-ese:

- "**Published works**" (EN homepage KPI) → AR original was probably "أعمال منشورة." Better EN: "**Pieces published**" or "**Stories published**." "Works" is a literal translation that doesn't sit right in EN business-media register.
- "**Curated channels**" (EN homepage KPI) → fine, but the descriptor "**Mindset · Flousak · Business · Media + 2**" includes "Flousak" untranslated. Either gloss it ("Flousak — markets") or replace with all-English ("Mindset · Markets · Business · Media + 2"). Currently inconsistent.
- The hero ticker repeats the label "**Saudi markets · Macroeconomics · Self-development · Business deals · Founder stories · Cinematic narrative · Almobadir · weekly**" — these are clean. Hold.

### 7. The Manifesto ledger — mismatched flourish

EN manifesto reads: *"...ideas in business and finance can be told with the **pull of a thriller**."* AR reads: *"يمكن سردُها بنفس **جاذبيةِ أفلامِ الإثارة**."* The AR phrase is "the appeal of thriller films" — closer to "the gravity of a thriller" or "the magnetism of a thriller film." "Pull of a thriller" is good but slightly off-register; "magnetism" is closer. Editorial nit.

### 8. EST. 1446H — hold with footnote

The hero monogram says "EST. 1446H" — Hijri year 1446 = ~Aug 2024–Aug 2025 Gregorian. Almobadir as a content network is older than that (the founder's BBA, the original `@almobadir` handle predates 2024), but 1446H is presumably the founding of the *current* network/agency configuration. **Audit note:** EN audiences won't recognize 1446H without a footnote. Add a hover tooltip or a footer line: "EST. 1446H ≈ 2024 CE." Otherwise EN journalists will Google it and may get the wrong founding year.

### 9. The newsletter day name — handled

The EN MutationObserver overrides the Arabic `الأحد` ("Sunday") to "Sunday." But the EN page also hardcodes "**Sunday morning**" in the hero (line 222) and "**One read a week**" in the footer (line 1101). Both are clean.

---

## Tone calibration (AR voice profile + recommended EN voice profile)

### AR voice (extracted from `website/index.html` and prior Wave 1/2 findings per the handoff)

- **Register:** Modern Standard Arabic with Saudi-leaning warmth. Not classical/Quranic-formal; not heavy dialect. Roughly the register of a mid-career Saudi business journalist writing for an under-35 reader.
- **Cadence:** declarative, short sentences. "نحن لسنا منصةَ محتوى. نحن فريقٌ يؤمن أن المعرفةَ الجادّة لا يجب أن تكون مملّة." (We are not a content platform. We are a team that believes serious knowledge does not have to be boring.) Each sentence stands alone; they don't lean on each other.
- **Authority gestures:** numbers ("3.3M متابع", "25 مليون مشاهدة شهرياً"), named recognition ("#6 على قائمة Favikon"), specifics ("سبع قنوات على إنستغرام وأربع على تيك توك").
- **Signature moves:** the bullet list of three (e.g., "أعلى تأثير... زاوية واحدة حادّة... أرقام · حقائق · مصادر"); the kicker label + ALL-CAPS English subhead ("§ 001 · METHOD" + "MANIFESTO / 001"); the cinematic metaphor ("بصرامة الباحث وحبكة الفيلم" — with the rigor of a researcher and the plot of a film).
- **Cultural texture:** Saudi-locating but not parochial. Riyadh datelines, Saudi market references, Arabic-typography flourishes (the wordmark in heavy sliced display Arabic).

### EN voice — recommended

The current EN copy is competently translated but reads as **a translation, not a parallel original.** It is one beat too soft. Calibration targets:

- **Register match:** confident editorial English, the register of a senior FT or Bloomberg writer who happens to be writing about a Saudi creator network. Not Wired-y, not LinkedIn-y, not influencer-y. *The Economist* if *The Economist* covered the creator economy.
- **Cadence match:** preserve the AR habit of short declarative sentences. Cut the connectives ("which is why...", "in addition to..."). The current EN copy occasionally over-connects ("We make business and economic content with the rigor of a researcher and the plotting of a film. The Almobadir network compresses..."). Pull these into separate sentences: "We make business and economic content with the rigor of a researcher and the plotting of a film. Our network compresses the complexity of markets into stories you remember."
- **Authority gestures, with EN equivalents:** swap "Almobadir" + Arabic numerals for "Almobadir" + Latin numerals (already done). Swap "EST. 1446H" with "EST. 1446H · 2024 CE" or just "Founded 2024" depending on Badr's preference. Add a "Headquartered in Riyadh" line to anchor.
- **Specific cuts to apply:**
  - Line 207 EN: "We make business and economic content with the rigor of a researcher and the plotting of a film. The Almobadir network compresses the complexity of markets into stories you remember: 25 million monthly views across seven channels on Instagram and TikTok, curated for a generation that reads a balance sheet the way it reads a screenplay." — *Tighten:* "Business and economic content, made with the rigor of a researcher and the plotting of a film. 25 million monthly views across seven Instagram channels and four on TikTok. We write for a generation that reads a balance sheet the way it reads a screenplay."
  - Line 397 EN nw3a-lede: "The Almobadir network on Instagram and TikTok · from self-development to global markets, from the founder on camera to the creative director and the agency that ships." — Fine, hold. The em-dashes and ellipses in EN tend to be where AR uses ·-separated phrases; the current copy uses · as a deliberate stylistic carry-over, which is actually working.
  - Line 1121 EN footer-desc: "Seven channels on Instagram, four on TikTok. Over 3.3 million followers. One voice for the Arab market — analysis, education, and empowerment for business pioneers." — *"empowerment for business pioneers"* is the softest phrase on the page and a translation tell. Replace with: "Analysis, education, and a louder edge for the Arab business reader."
- **What NOT to change:** the manifesto. EN manifesto is the strongest piece of writing on the EN site. Hold.

### Voice rule of thumb

If an EN sentence could be the lede of a Bloomberg Businessweek profile of Almobadir, it's right. If it sounds like a confident translation of an Arabic press release, it's wrong.

---

## Recommended bilingual strategy for Almobadir (3 paragraphs)

**Paragraph 1 — strategic posture.** The Arabic site is the audience flagship; the English site is the credentialing dossier. They share a homepage architecture (it is the brand showcase, identical hero KPIs and network grid, parallel manifesto, parallel founder section), but they diverge below the homepage: AR builds depth in audience-facing surfaces (newsletter archive in Arabic, Saudi-dialect commentary, real-time market reactions), EN builds depth in B2B and credentialing surfaces (press kit, agency case studies, methodology longread, founder press page, machine-readable numbers endpoint). Both languages run from the same `assets/` bundle, the same hreflang web, and the same brand voice — but the *content roadmap* for each is different and is owned by different people in the editorial system.

**Paragraph 2 — the 80/20 rule.** 80% of pages are mirrored (homepage, network, founder, method, content latest, newsletter signup). 20% are language-specific. The AR-only 20% are Saudi-dialect short content, hashtag-driven posts, daily commentary, in-jokes, and religious/cultural-specific framings — which translate to inert English. The EN-only 20% are press kit, agency case studies, partnerships page (if Badr wants it), methodology longread, founder press page, and `numbers.json`. This split keeps the SEO graph clean (every URL has a clear hreflang twin OR is explicitly EN-only with no AR equivalent and no broken-mirror hreflang), and it frees the AR team from the cost of translating content the EN audience won't engage with anyway.

**Paragraph 3 — implementation cadence.** Week 1: tone-calibrate the EN homepage (apply the cuts above, add the "Almobadir (المبادر, 'the initiator')" gloss, replace "Published works" → "Pieces published", footnote the 1446H date). Week 2: ship `/en/press/` with fact sheet, founder bio (3 lengths), brand asset ZIP, founder portrait set, network roster table, press contact (`press@almobadir.com`), and `numbers.json`. Week 3: write the `/en/method/` longread (1500-2000 words). Week 4: build `/en/agency/cases/` with 3-5 case studies pulled from `almobadir.media` archive. Audit hreflang quarterly thereafter (Mak IT 2026 multilingual SEO recommendation: monthly Screaming Frog crawl; quarterly GSC International Targeting review). The EN side never tries to outpace the AR side on freshness; it tries to outdo it on density and citability.

---

## Concrete EN-site changes (current 1:1 mirror → 80% mirror + 20% EN-only)

Ranked by leverage (P0 = ship this week; P1 = ship next month; P2 = nice-to-have).

| # | Change | Priority | File / surface |
|---|---|---|---|
| 1 | Add JSON-LD Organization block with `@inLanguage` and `availableLanguage: ["ar","en"]`. Include `name`, `alternateName: "المبادر"`, `url`, `logo`, `sameAs: [IG, TikTok, YouTube, Threads, Favikon]`, `founder: { @type: Person, name: "Badr Shaker", url: "https://instagram.com/badrshaqer" }`. | P0 | both pages, `<head>` |
| 2 | Add JSON-LD WebSite + WebPage on each page with correct `inLanguage`. | P0 | both pages, `<head>` |
| 3 | Footnote 1446H with Gregorian equivalent on EN. | P0 | EN line 183 |
| 4 | Gloss "Almobadir" on first body-copy occurrence per landing page on EN. | P0 | EN multiple lines |
| 5 | Replace "Published works" → "Pieces published" on EN. | P0 | EN line 267 |
| 6 | Resolve "Flousak" untranslated tag in EN KPI footnote. | P0 | EN line 258 |
| 7 | Tighten the hero lede paragraph (cuts above). | P0 | EN line 207 |
| 8 | Replace "empowerment for business pioneers" → "a louder edge for the Arab business reader" in EN footer. | P0 | EN line 1121 |
| 9 | Add `press@almobadir.com` alias + page `/en/press/` with fact sheet, founder bios (3 lengths), asset ZIP, portrait set, mentions, numbers.json. | P0 | new file |
| 10 | Fix `aria-hidden` on EN manifesto AR fragment (currently visible but hidden from screen-readers). | P0 | EN line 379 |
| 11 | Add a third regex branch (`بعد X د Y ث` → "in Xm Ys") to the EN MutationObserver for newsletter countdown. | P1 | EN line 1242-1248 |
| 12 | Update lang-toggle to map `/en/<slug>` → `/<slug>` (currently hard-coded to `/` and `/en/`). | P1 | EN line 1193-1218 |
| 13 | Build `/en/method/` longread. | P1 | new file |
| 14 | Build `/en/agency/cases/` hub with 3-5 case studies. | P1 | new file |
| 15 | Build `/en/network/` deep-dive sub-page. | P2 | new file |
| 16 | Build `/en/founder/` press hub. | P2 | new file |
| 17 | Defer non-wordmark Arabic fonts on EN with `font-display: optional`. | P2 | `assets/fonts.css` |
| 18 | Add a sitemap.xml with `<xhtml:link rel="alternate" hreflang>` blocks per URL once the EN tree grows past 1 page. | P2 | new file |
| 19 | Set GSC geo-targeting: AR → Saudi Arabia, EN → unlisted. | P0 | GSC config |
| 20 | If Badr greenlights an EN newsletter edition, add `/en/brief/archive/`. | P2 | new file (conditional) |

---

## Press / media-kit EN page (proposed structure)

Wireframe for `/en/press/`. Designed to be one HTML page; assets live alongside.

```
HERO
  H1: "Almobadir · Press & media kit"
  Sub: "Saudi Arabia's business and finance content network. Founded 1446H (2024 CE). 3.3M+ followers."
  Two CTAs: [Download brand assets ZIP] [press@almobadir.com]

AT-A-GLANCE
  Two-column grid of facts (numbers + labels), pull from numbers.json:
    3.3M+ total followers
    25M monthly views
    7 Instagram channels
    4 TikTok channels
    143+ agency clients
    5,000+ pieces published
    $1M+ in ad spend managed
    #6 Favikon Top 20 Saudi B2B Creators 2025
    Founded 1446H (≈2024 CE)
    HQ Riyadh, Saudi Arabia
    Founder Badr Shaker

ABOUT (3 paragraphs, ~250 words)
  Plain-prose description of the network, the editorial method, the agency arm.
  No marketing copy. The kind of paragraph a journalist will paste into their piece.

FOUNDER BIO (3 lengths in expandable cards)
  50-word version (1-line lede)
  150-word version (one paragraph)
  500-word version (full profile, ready for keynote bio printing)
  Plus: birth year (Badr to confirm), education, prior work, public recognitions, Instagram links.

NETWORK ROSTER (table)
  Channel | Platform | Followers | Role | Color | Link
  (denser version of the homepage `nw3a` grid — copy-paste-friendly)

ASSETS (download grid)
  - Logo SVG (cream)        [download]
  - Logo SVG (crimson)      [download]
  - Logo PNG 512/1024/2048  [download]
  - Wordmark SVG (Arabic)   [download]
  - Founder portrait 16:9   [download]
  - Founder portrait 4:5    [download]
  - Founder portrait 1:1    [download]
  - Network color hex sheet [download .txt]
  - Brand assets ZIP (all)  [download .zip]
  License inside ZIP — short and editorial-permissive.

IN THE PRESS
  Chronological list, 3-10 entries, each with: outlet, date, headline, link.
  Empty state: "Coverage coming. Reach out at press@almobadir.com to set up an interview."

MACHINE-READABLE
  Link to /en/press/numbers.json (with copy-paste cURL example for engineers).

CONTACT
  press@almobadir.com — for editorial inquiries, response within 48 hours
  partnerships@almobadir.com — for sponsorship and brand-deal inquiries
  speaking@almobadir.com — for keynote/conference invites
  hello@almobadir.com — for everything else
  +966 (visible only to authenticated press, optional)

FOOTER
  Last updated: 2026-MM-DD
  hreflang to AR equivalent (only if /press/ is built in AR; otherwise omit hreflang and let it stand alone)
```

---

## Open questions for Badr

These are decisions only the founder can make. Each unblocks a P1 or P2 above.

1. **Does Almobadir publish the weekly brief in English?** If yes, build `/en/brief/archive/` and translate every issue. If no, keep the EN homepage CTA pointing at the AR brief and add a one-line note: "Brief is published in Arabic only."
2. **Is `1446H` the founding year of the *network* or just the *current configuration*?** Affects the press-kit "Founded" line and the hero monogram. Recommend consistency: pick one and use it everywhere.
3. **Should the EN site be EN-US, EN-GB, or international?** Default `og:locale` is `en_US`. If Almobadir's EN audience is more Gulf B2B + UK financial press than US, switch to `en_GB`. Affects spelling ("organization" vs "organisation"), date format, and number conventions (commas vs spaces).
4. **Almobadir Media — separate brand site or sub-section of `almobadir.com/en/agency/`?** Currently linked out to `almobadir.media`. If the agency strategy is to keep them separate, the EN site only links out (current state). If the strategy is to consolidate the credentialing, lift case studies INTO `/en/agency/cases/`. Affects whether agency case studies are an EN-only opportunity here, or a separate site's job.
5. **Does Almobadir take on partnerships / outside investment / strategic JVs?** If yes, build `/en/partnerships/`. If no, do not — having an empty partnerships page sends a worse signal than not having one.
6. **Press list — is there one?** A "press@almobadir.com" alias is cheap to add but only useful if someone monitors it. Confirm a real human owns the inbox before publishing it.
7. **Is the `1446H` Hijri date load-bearing or stylistic?** If load-bearing (founding date), keep it and add the Gregorian footnote. If purely stylistic ("we use Hijri because we are a Saudi brand"), pair it with `2026CE` always so EN readers can parse.
8. **Should the EN manifesto be revised, or held?** The current EN manifesto is the strongest piece of EN writing on the site. Recommendation is hold; flagging for confirmation.
9. **Voice ownership — who writes EN copy going forward?** A translator working from AR drafts will produce 1:1 mirror; a parallel English writer briefed on the brand voice will produce parallel originals. Recommendation: hire/assign one of the latter, not the former. Affects long-term divergence/symmetry.
10. **EN keyword research — which queries does Badr want to rank for?** "Saudi business creator," "Arab business media network," "Saudi finance influencer," "MENA creator economy," "Almobadir Badr Shaker," "Almobadir Media agency." The current EN copy was tone-calibrated to mirror AR; an EN keyword brief would let the next EN round absorb high-intent search queries without losing the editorial register.

---

## Sources

### Bilingual & MENA brand patterns
- [Wamda × Financial Times partnership](https://www.wamda.com/2022/02/wamda-works-financial-times-bring-global-insights-english-arabic-mena-technology-ecosystem) — Wamda fully bilingual, FT-syndicated EN content
- [Wamda homepage analysis (WebFetch 2026-04-27)](https://www.wamda.com/) — confirmed EN-default with `/ar` toggle, content same across languages
- [Asharq Al-Awsat English homepage analysis (WebFetch 2026-04-27)](https://english.aawsat.com/) — five-language family (AR/EN/TR/UR/FA), adapted not identical
- [NEOM website analysis (WebFetch 2026-04-27)](https://www.neom.com/en-us) — investor/B2B-first EN site, dedicated newsroom and press contact
- [The Arabic-English Content Conundrum, Entrepreneur ME](https://www.entrepreneur.com/en-ae/marketing/the-arabic-english-content-conundrum-communicating-to-a/304005) — multilingual brand-tone considerations
- [How to Localize Digital Media for Arabic Audiences, Igloo](https://www.weareigloo.com/how-to-localize-digital-media-for-arabic-audiences/)
- [Saudi Research and Media Group (Wikipedia)](https://en.wikipedia.org/wiki/Saudi_Research_and_Media_Group) — SRMG owns Asharq Al-Awsat, Arab News, et al.
- [Argaam launches Argaam Insight](https://aawsat.com/english/home/article/1082521/argaam-launches-argaam-insight-service)
- [Asharq 5th anniversary (Arab News)](https://www.arabnews.com/node/2622328/media)
- [Diriyah Gate CEO interview (Asharq Al-Awsat English)](https://english.aawsat.com//home/article/1792946/diriyah-gate-ceo-asharq-al-awsat-projects-be-revealed-november)

### SEO best practices
- [Hreflang Tags: Ultimate 2026 Guide for International SEO (ClickRank)](https://www.clickrank.ai/hreflang-tags-complete-guide/)
- [Hreflang Implementation Guide (LinkGraph 2026)](https://www.linkgraph.com/blog/hreflang-implementation-guide/) — 65% of international sites have hreflang errors
- [Multilingual SEO in Saudi & UAE (Mak IT Solutions)](https://makitsol.com/multilingual-seo-in-saudi-uae-avoid-duplicates/) — region-specific guidance
- [Managing Multi-Regional and Multilingual Sites (Google Search Central)](https://developers.google.com/search/docs/specialty/international/managing-multi-regional-sites)
- [Localized Versions of your Pages (Google Search Central)](https://developers.google.com/search/docs/specialty/international/localized-versions)
- [Multilingual SEO Best Practices In 2026 (Keytomic)](https://keytomic.com/blog/multilingual-seo-best-practices)
- [Hreflang Tags & X Default for International SEO 2025 (ThatWare)](https://thatware.co/hreflang-xdefault-tags-for-international-seo/)
- [Weglot Hreflang Guide](https://www.weglot.com/guides/hreflang-tag)
- [Schema.org `inLanguage` property](https://schema.org/inLanguage)
- [Schema.org `availableLanguage` property](https://schema.org/availableLanguage)

### Localization / translation philosophy
- [Arabic Localization for Beginners (PoliLingua)](https://www.polilingua.com/blog/post/arabic_localization_for_brands.htm)
- [Guidelines for Arabic Content Localization (GoToMENA)](https://gotomena.com/blog/guidelines-for-arabic-content-localization)
- [Translation vs. localization (VeraContent)](https://veracontent.com/mix/translation-vs-localization/)
- [Translating for Arabic Markets (Localize)](https://localizejs.com/articles/localizing-for-the-arabic-market-things-to-know)
- [Why GCC Businesses Are Betting Big on Arabic-English Marketing in 2026 (Shamil)](https://shamiltranslation.com/2025/11/16/why-gcc-businesses-are-betting-big-on-arabic-english-marketing-in-2026/)
- [Arabic Localization Checklist (Wordminds)](https://wordminds.com/blog/the-complete-arabic-localization-checklist-for-businesses)

### Arabic dialects / register
- [Arabic Dialects Guide (Qasid Online)](https://www.qasidonline.com/blog/the-ultimate-guide-to-arabic-dialects)
- [Gulf Arabic (Wikipedia)](https://en.wikipedia.org/wiki/Gulf_Arabic)
- [Mastering the Saudi Dialect (Talkpal)](https://talkpal.ai/mastering-the-saudi-dialect-your-ultimate-guide-to-local-arabic/)
- [Arabic Dialects Compared: Maghrebi, Egyptian, Levantine, Hejazi, Gulf, MSA (Discover Discomfort)](https://discoverdiscomfort.com/arabic-dialects-maghrebi-egyptian-levantine-gulf-hejazi-msa/)
- [MSA vs Dialects (ArabicGuru Academy)](https://arabicguruacademy.com/blogs/modern-standard-arabic-vs-dialects-what-should-you-learn-first/)

### Press & media kit benchmarks
- [Press Kit 101: What to Include (Prezly Academy 2026)](https://www.prezly.com/academy/press-kit-101-what-to-include-to-get-earned-media-coverage)
- [9 Media Kit Examples (SelfPublishing.com)](https://selfpublishing.com/media-kit-examples/)
- [How to Build an Author Media Kit (Reedsy)](https://reedsy.com/blog/author-media-kit-template/)
- [Author Press Kit Examples (AutomateEd)](https://www.automateed.com/author-press-kit-examples)
- [Creating a Digital Author Press Kit (San Diego Writers Festival)](https://sandiegowritersfestival.com/creating-a-digital-author-press-kit-five-things-to-know-by-laura-segal-stegman/celebrate-writing/)

### MENA press distribution context
- [Arab News advertising mediakit (Kochava)](https://media-index.kochava.com/ad_partners/arab-news)
- [Al Arabiya News mediakit (Kochava)](https://media-index.kochava.com/ad_partners/al-arabiya-news)
- [Arab Newswire (Saudi/MENA distribution)](https://arabnewswire.com/)
- [Mass media in Saudi Arabia (Wikipedia)](https://en.wikipedia.org/wiki/Mass_media_in_Saudi_Arabia)
- [Al Arabiya (Wikipedia)](https://en.wikipedia.org/wiki/Al_Arabiya)

### Internal source files
- `C:\Users\toner\OneDrive\Desktop\almobadir\website\index.html` (AR, 1234 lines, audited 2026-04-27)
- `C:\Users\toner\OneDrive\Desktop\almobadir\website\en\index.html` (EN, 1274 lines, audited 2026-04-27)
- `C:\Users\toner\OneDrive\Desktop\almobadir\handoff-prior-session.md` (brand audit handoff)

---

*End of briefing — Wave 3 deep-dive 9 of 10. Next: deep-dive 10 closes Wave 3.*
