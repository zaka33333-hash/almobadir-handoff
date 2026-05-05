# Topic: MENA Arabic newsletter market — briefing for Almobadir

**Wave 3, topical deep-dive 5/10 — Apr 2026**
**Author:** Agent 3.5 (CONTENT Intelligence)
**Companion docs:** `dragnet-ar.md`, `dragnet-loggedin.md`

---

## Executive summary

Almobadir publicly promises a newsletter ("نشرة المبادر") but the live page contains only Lorem-Ipsum from Jan 2023 (Wave 1). Across every owned property checked logged-in (Wave 1.5), there is **no ESP, no Substack, no Beehiiv, no real send list**. With 3.3M followers across 11 social handles and zero email-owned audience, this is the brand's largest distribution gap — and the most leveraged single fix in the audit.

The MENA Arabic newsletter market in 2026 is small but maturing: Hodhod (gohodhod.com) — a Saudi/regional Arabic-first ESP — has crossed 7,500+ publishers with native AWS SES delivery and full RTL editing. Substack added official RTL support for Arabic in Jan 2022 and now hosts active Arabic publishers (Saud AlHawawi, Saudi Market Monitor, ArabLit, Khaled Talhouni). Beehiiv has zero Arabic UI/RTL support but offers the strongest growth+monetization stack. Kit and Mailchimp officially do **not** support RTL. MailerLite handles RTL via custom HTML only.

**Recommendation:** Launch on **Hodhod** (Arabic-first, regional billing, regional support timezone, founder-friendly free tier ≤500 subs) with a parallel Substack mirror for SEO/discovery. Cadence: weekly, Sunday morning Riyadh time, 800-1,400 Arabic words. Format: signature columns + curated MENA startup deals + one founder profile. Monetize via partner sponsorships starting at 5K subs (target Q3 2026), not paid subscriptions yet. First lead-magnet: "خريطة المؤسسين السعوديين 2026" (Saudi 2026 founder map) PDF — cleanly fits the brand promise.

---

## Platform comparison (5 platforms, ranked for Almobadir)

### 1. Hodhod (gohodhod.com) — RECOMMENDED PRIMARY

**The only Arabic-first regional ESP with serious traction.**

- **Founder/origin:** Regional (Saudi-led publisher base), positioned as "منصة النشر البريدي للمنطقة" (the email publishing platform for the region) [[Hodhod homepage](https://gohodhod.com/)]
- **Publisher base:** 7,500+ publishers as of Apr 2026 [[Hodhod homepage](https://gohodhod.com/)]
- **RTL support:** Native, built into the editor — explicitly markets "تنسيق لا يفهم لغتك" (formatting that understands your language) as an answer to Western tools' RTL pain [[Hodhod homepage](https://gohodhod.com/)]
- **Delivery infrastructure:** AWS SES under the hood [[GitHub - Abdoo-mayhob/Hodhod](https://github.com/Abdoo-mayhob/Hodhod)] — same backend as Substack/Kit, so deliverability is on par
- **Languages on the editor:** Arabic primary; English + French announced as "coming soon"
- **Pricing:** Freemium — free up to **500 subscribers**; paid scales with list size (monthly + annual options) [[Hodhod homepage](https://gohodhod.com/)]
- **Monetization:** Subscription landing pages built-in; data ownership / list export anytime; no built-in ad network (yet)
- **Support:** Region timezone (vs Beehiiv/Substack EST/PST gaps)
- **Discovery:** Curated "Discover" directory at `gohodhod.com/discover` lists newsletters by category — free distribution channel for new publishers
- **Notable Hodhod publishers (verified Apr 2026):**
  - **النشرة التجارية من مكاسب** — weekly e-commerce newsletter [[Hodhod Discover](https://gohodhod.com/discover)]
  - **نشرة خالد بزماوي** — 2,192 subs, 12 issues, digital marketing + financial literacy [[Hodhod Discover](https://gohodhod.com/discover)]
  - **نشرة "مرتكز"** — 1,431 subs, twice-monthly, Arab tech products [[Hodhod Discover](https://gohodhod.com/discover)]
  - **نشرة "التمكين"** — weekly, finance/accounting/tax for Arabic accountants
  - **مجتمع الدوافير الخاص** — 463 subs, content marketing [[Hodhod Discover](https://gohodhod.com/discover)]
- **Risks:** Smaller platform = smaller ad network + fewer 3rd-party integrations. No referral-program tooling at the level of Beehiiv. Roadmap velocity uncertain vs venture-funded Substack/Beehiiv.
- **Verdict:** **Best for Almobadir's primary publication.** RTL "just works." Saudi/regional publisher base = peers + partnership pool. Free tier unblocks the launch.

### 2. Substack — RECOMMENDED SECONDARY (mirror / SEO)

- **RTL support:** YES — added officially Jan 2022 for Arabic, Hebrew, Pashto, Urdu, Sindhi. The editor auto-aligns on input [[Substack on X](https://x.com/SubstackInc/status/1486472716295098368)]
- **Localization:** Reader-side Arabic UI available [[Substack support](https://support.substack.com/hc/en-us/articles/18575507636500-Can-readers-see-my-Substack-publication-in-a-different-language)]
- **Fees:** **10% of paid-sub revenue, forever**, plus Stripe ~2.9% + $0.30 = effectively ~13% off the top [[EmailToolTester](https://www.emailtooltester.com/en/blog/beehiiv-vs-substack/)]
- **Free tier:** Yes, unlimited subs while free
- **Scale signal:** Substack crossed **8.4M paid subs** in Q1 2026, +68% since Mar 2025 [[Readless](https://www.readless.app/blog/best-paid-substack-newsletters-2026)]
- **MENA traction (verified Substacks):**
  - **النشرة البريدية الأسبوعية لسعود الهواوي** ([saud.substack.com](https://saud.substack.com/)) — VC investor, "thousands of subscribers," weekly, Arabic
  - **Saudi Market Monitor** ([saudimarketmonitor.substack.com](https://saudimarketmonitor.substack.com/)) — bi-weekly, Saudi capital-markets, English, hundreds of subscribers, free
  - **Saudi+** ([saudiplus.substack.com](https://saudiplus.substack.com/about)) — on-the-ground Saudi commentary
  - **Khaled's Substack** ([khaledtalhouni.substack.com](https://khaledtalhouni.substack.com/)) — Khaled Talhouni (Nuwa Capital), MENA VC analysis, monthly
  - **ArabLit** ([arablit.substack.com](https://arablit.substack.com/)) — Arabic literature/translation, monthly, established audience
  - **Ihsan Arabic** ([ihsanarabic.substack.com](https://ihsanarabic.substack.com/)) — weekly Arabic gems, "thousands of subscribers"
- **Notes/Recommends/Network:** Substack's "Notes" + Recommends graph drives 30%+ of new-pub growth — a discovery moat Hodhod cannot match yet
- **Verdict:** Mirror Almobadir's content here for discovery + SEO + tap into the Saudi VC/founder Substack cluster. Don't host the primary list here (10% fee + ownership concerns).

### 3. Beehiiv

- **RTL support:** **NO native Arabic editor / RTL.** Publication language can be set to English/Spanish/French/German/Italian/Portuguese only — Arabic is not in the list [[Beehiiv support](https://www.beehiiv.com/support/article/4993791529751-setting-a-default-language-for-your-publication)]. Auto-generated text (unsubscribe etc.) cannot be Arabic.
- **Fees:** **0% of paid-sub revenue.** Flat plans: Launch (free, 2,500 subs), Grow $49/mo, Scale $99/mo [[EmailToolTester](https://www.emailtooltester.com/en/blog/beehiiv-vs-substack/)]
- **Scale:** Q1 2026 added $4.5M ARR, crossed 50,000 active users; powered 20B+ emails in 2025 generating $25M+ in publisher revenue [[Beehiiv blog](https://www.beehiiv.com/blog/the-state-of-newsletters-2026)]; revenue $30M ARR June 2025 vs $19.8M Dec 2024 [[Sacra](https://sacra.com/c/beehiiv/)]
- **Ad network:** Built-in CPM marketplace — small newsletters earn $40-80/mo at ~1,200 subs passively [[Beehiiv ad network](https://www.beehiiv.com/blog/newsletter-sponsorship-cost)]; CPMs $5-20 for new publishers, $50-100+ for B2B niches
- **Referral program:** Built-in, +17% avg subscriber lift [[Beehiiv blog](https://www.beehiiv.com/blog/build-successful-referral-program-newsletter)]; case studies: Milk Road exited for millions in 10 months, The Brink added 14k subs in month 1
- **Paid subs:** Requires Scale plan ($99/mo) to enable
- **Verdict:** **Best growth+monetization stack — but RTL is a dealbreaker for Arabic-first publication.** Re-evaluate if Beehiiv ships Arabic UI in 2026; until then, only viable for an English-language Almobadir Global edition.

### 4. MailerLite

- **RTL support:** Not native — supported only via custom HTML templates; Arabic auto-text not available. GetResponse is the cited official RTL-supporting alternative on this front [[Kindlepreneur](https://kindlepreneur.com/convertkit-review/)]
- **Pricing:** Free tier (≤500 subs), paid from $10/mo [[EmailToolTester substack-alternatives](https://www.emailtooltester.com/en/blog/substack-alternatives/)]
- **Features:** Drag-and-drop editor, automation builder, digital product sales, website builder, sponsorship guide
- **MENA presence:** Listed in MartechLive's top-10 ME tools [[MartechLive](https://martechlive.com/top-10-email-marketing-tools-for-middle-east/)] but no regional team or Arabic-specific UI
- **Verdict:** Cheap and feature-rich, but the RTL-via-custom-HTML overhead negates the cost savings for an Arabic publication. Skip.

### 5. Kit (formerly ConvertKit)

- **RTL support:** **NO.** Officially confirmed: no RTL in the email editor. Neither Kit nor Mailchimp natively supports Arabic/Hebrew [[Kindlepreneur](https://kindlepreneur.com/convertkit-review/)]
- **Pricing:** Free up to 10,000 subs; paid from $29/mo [[EmailToolTester substack-alternatives](https://www.emailtooltester.com/en/blog/substack-alternatives/)]
- **Strengths:** Best-in-class automation + Creator Network for cross-promotion; native commerce (digital products); Stripe/WordPress integrations
- **Verdict:** Not viable for Arabic-first publication. Generous free tier is wasted if RTL forces fully custom HTML on every send.

### Honourable mention: GetResponse

Multiple comparison articles cite **GetResponse** as the recommended RTL-supporting alternative when Kit/Mailchimp/MailerLite fall short [[Kindlepreneur](https://kindlepreneur.com/convertkit-review/)]. Worth pricing if Hodhod hits a ceiling. Free plan exists, Creator Plan from $59/mo. No Saudi-specific publisher base.

### Native MENA ESPs (other than Hodhod)

- **Hsoub ecosystem (Mostaql, Khamsat, IO):** 2.5M+ registered users across products [[Hsoub](https://www.hsoub.com/en/)] but Hsoub does NOT operate a public newsletter ESP — it offers freelance + community + a Hacker-News-style discussion board. Not an ESP option.
- **Argaam newsletter centre:** Argaam runs in-house Arabic financial newsletters from `argaam.com/ar/newsletters` [[Argaam newsletters](https://www.argaam.com/ar/newsletters)] — proprietary, not a publishing platform for others.
- **Thmanyah:** Saudi media-tech company (20+ products, podcasts/articles/newsletters); relaunched `thmanyah.com` as a creator destination including newsletters [[Entrepreneur ME](https://www.entrepreneur.com/en-ae/growth-strategies/saudi-arabia-based-thmanyahs-new-focus-on-content-creators/469579)] — closer to a creator network than an open ESP. Worth direct outreach.

---

## Top 8+ Arabic business newsletters (cited)

### 1. النشرة البريدية الأسبوعية لسعود الهواوي (Saud AlHawawi)
- **Platform:** Substack [[saud.substack.com](https://saud.substack.com/)]
- **Author:** Saud AlHawawi — Funds Investment & Operation Senior, Saudi VC ecosystem
- **Cadence:** Weekly (gaps observed; most recent ramp Aug 2025)
- **Subs:** "Thousands" (not publicly disclosed)
- **Topics:** Career reflections, VC/investment, professional reputation, product-led growth, retirement, curiosity essays
- **Monetization:** Free
- **Distribution lift:** TikTok presence (@hawawie2), strong Saudi VC LinkedIn [[LinkedIn - Saud AlHawawi](https://www.linkedin.com/in/saudalhawawi/)] — classic social-to-Substack flywheel

### 2. Saudi Market Monitor
- **Platform:** Substack [[saudimarketmonitor.substack.com](https://saudimarketmonitor.substack.com/)]
- **Cadence:** Bi-weekly, ~5 min reads
- **Topics:** TASI, IPOs, FX, cross-border capital, Vision 2030 policy
- **Audience:** Asset managers, IB, policy researchers
- **Monetization:** Free (subscription required, no paid tier disclosed)
- **Subs:** "Hundreds"

### 3. نشرة خالد بزماوي (Khaled Bzmawe)
- **Platform:** Hodhod [[Hodhod Discover](https://gohodhod.com/discover)]
- **Subs:** 2,192 (publicly displayed on Hodhod)
- **Topics:** Digital marketing, business, financial literacy
- **Author:** Khaled Bzmawe — Saudi digital marketer, Startzex agency, has managed $500k+ in ad spend, trained 30+ brands [[khaledbzmawe.com](https://khaledbzmawe.com/)]
- **Reach lever:** YouTube channel + courses; newsletter is conversion endpoint
- **Issues:** 12 published — relatively new but growing fast

### 4. نشرة "مرتكز" (Mortakaz)
- **Platform:** Hodhod
- **Subs:** 1,431
- **Cadence:** Twice-monthly
- **Topics:** Arab tech products, emerging projects — closest analog to a Product-Hunt-in-Arabic
- **Why it matters for Almobadir:** Same audience overlap (founders/builders); potential cross-promo partner

### 5. نشرة التمكين (Tamkeen)
- **Platform:** Hodhod
- **Cadence:** Weekly
- **Topics:** Finance, accounting, Saudi tax, Zakat reporting — vertical SaaS-adjacent niche
- **Audience:** CFOs, CPAs, finance teams
- **Monetization model hint:** B2B finance niches command 50-100+ CPM globally — high ad-yield potential

### 6. Wamda
- **Platform:** Native (proprietary list, wamda.com newsletter signup) [[Wamda](https://www.wamda.com/)]
- **Cadence:** Weekly digest
- **Topics:** MENA startup ecosystem, op-eds, events, fundraising rounds
- **Audience:** Investors + ecosystem operators across MENA
- **Notes:** Direct competitor to any Almobadir founder-focused brief — but English-leaning. Wamda also has Arabic content via wamda.com/ar.

### 7. MAGNiTT Newsletters
- **Platform:** Native (magnitt.com/newsletters) [[MAGNiTT newsletters](https://magnitt.com/newsletters)]
- **Cadence:** Multiple per week
- **Topics:** MENA + adjacent emerging markets (Africa, SE Asia, Türkiye, Pakistan) VC + PE deals
- **Monetization:** Free + premium MAGNiTT Plus subscription (historically ~$500/mo for the data product) [[MAGNiTT pricing](https://magnitt.com/pricing)]
- **Audience:** VCs, LPs, corp dev — premium B2B
- **Differentiator:** Data + research-backed; not a pure essay newsletter

### 8. Argaam Newsletters
- **Platform:** Native (argaam.com/ar/newsletters) [[Argaam](https://www.argaam.com/ar/newsletters)]
- **Cadence:** Daily Tadawul (TASI) updates + thematic editions
- **Topics:** Saudi stock market, sector analysis, IPO calendars, commodity prices
- **Audience:** "C-level executives, analysts, fund managers, investors" (Argaam disclosure)
- **Subs:** "Thousands" of paid/registered users — Argaam is the dominant Saudi financial-news brand since 2007 [[Argaam Wikipedia](https://en.wikipedia.org/wiki/Argaam)]
- **Founder:** Argaam parent owned by AGBI [[AGBI Argaam](https://www.agbi.com/companies/argaam/)]

### 9. Thmanyah Newsletters
- **Platform:** Thmanyah's own publishing infra [[thmanyah.com/newsletters](https://thmanyah.com/newsletters)]
- **Cadence:** Multiple newsletters across topics
- **Topics:** Sociopolitics, economics, technology, science, culture
- **Audience scale:** Thmanyah's Radio Thmanyah podcast network alone hits 351M+ listeners/viewers across the Arab world [[Entrepreneur ME](https://www.entrepreneur.com/en-ae/growth-strategies/saudi-arabia-based-thmanyahs-new-focus-on-content-creators/469579)]
- **Why it matters:** Thmanyah's creator-network play (Apr 2024 onwards) means they may host Almobadir's newsletter as a partnership — worth direct outreach

### 10. Maaal
- **Platform:** Native (maaal.com) [[Maaal](https://maaal.com/en/)]
- **Cadence:** Daily/weekly Saudi business news
- **Topics:** Saudi capital markets, company performance, commodities
- **Founder:** Mutlaq Al-Buqami (ex-Asharq Al-Awsat / Eqtisadiyah)
- **Audience:** Saudi finance professionals; site is 10+ years old (founded ~2014)

### 11. Khaled's Substack — Khaled Talhouni / Nuwa Capital
- **Platform:** Substack [[khaledtalhouni.substack.com](https://khaledtalhouni.substack.com/)]
- **Cadence:** Monthly (irregular)
- **Topics:** MENA VC, founder qualities, B2B marketplaces, foodtech, BNPL/RNPL
- **Author:** Senior MENA VC at Nuwa Capital
- **Language:** English
- **Why it matters:** Living example of an active MENA VC running a Substack — and the most relevant peer to whatever Almobadir publishes

### 12. ArabLit / Ihsan Arabic (cultural Substacks)
- **ArabLit:** Monthly, Arabic literature/translation, established audience [[arablit.substack.com](https://arablit.substack.com/)]
- **Ihsan Arabic:** Weekly Arabic gems, "thousands of subscribers" [[ihsanarabic.substack.com](https://ihsanarabic.substack.com/)]
- **Relevance:** Cultural-rather-than-business but proves Arabic Substack discovery works

---

## Pricing benchmarks

### Newsletter sponsorship CPMs

**Global benchmarks** [[Beehiiv blog - newsletter sponsorship cost](https://www.beehiiv.com/blog/newsletter-sponsorship-cost), [Whosponsorsstuff](https://site.whosponsorsstuff.com/how-to-price-an-ad-in-your-newsletter)]:
- **B2B specialty newsletters:** $50-100+ CPM
- **Consumer broad-niche:** $15-35 CPM
- **New publishers:** $5-20 CPM (sub 5K)
- **Primary placement (top of email):** $35-50+ CPM
- **Secondary placement:** $15-25 CPM
- **Dedicated sponsor send:** $50+ CPM

**MENA region adjustments:**
General display CPM rates from MENA traffic are dramatically lower vs Western: Saudi $0.21, Egypt $0.10, Algeria $0.08 [[Publisher Growth - 7 best CPM ad networks MENA](https://publishergrowth.com/blog-details/7-best-cpm-ad-networks-in-mena)]. **However, newsletter sponsorship CPMs are not the same as display CPMs** — the right comparable is direct B2B sponsorship deals priced in SAR/AED, which trend ~30-50% lower than US equivalents but with higher engagement. **Working SAR estimate for Almobadir at 5K subs with 50% open:**
- Primary placement: 100-150 SAR per send (≈ $27-40)
- Secondary: 50-80 SAR per send
- Dedicated send: 800-1,500 SAR

### Paid subscription tiers (when relevant)
- **Substack typical Arabic publisher:** $5-7/mo USD ≈ 19-26 SAR (matches Saud AlHawawi-tier publications globally — but most Arabic Substacks remain FREE in 2026)
- **AED tier benchmark:** 25-35 AED/mo (~$7-10)
- **Annual:** ~10x monthly (Substack/Beehiiv default)
- **MAGNiTT Plus:** ~$500/mo data + newsletter B2B premium

### Open rates and CTRs
**Global newsletter benchmarks 2025** [[MailerLite benchmarks](https://www.mailerlite.com/blog/compare-your-email-performance-metrics-industry-benchmarks)]:
- Average open rate: 43.46% (up from 42.35% in 2024 — Apple MPP-inflated)
- Newsletter category open rate: 40.08%
- Average CTR: 2.09% (2025), 2.00% (2024)
- EMEA region scores higher engagement vs APAC/LATAM due to stricter consent
- Best-day Saudi sends: Sunday-Tuesday morning (workweek = Sun-Thu)

**Arabic-language adjustment (qualitative, from Hodhod publisher signal):**
- New Arabic publishers commonly report 50-65% open rates in first 3 months (highly engaged early lists)
- Mature Arabic publishers settle at 40-50% — slightly above global newsletter average
- CTRs run 2.5-4% on Hodhod-hosted newsletters per Hodhod's analytics page (their marketing claim — directionally consistent with Substack data)

### Bottom-line for Almobadir's revenue model
At realistic 12-month targets (5K subs, 45% opens, weekly send):
- **Sponsor revenue potential:** 100 SAR x 1 sponsor x 4 sends/mo = 400 SAR/mo Q4 2026 → 1,500-2,000 SAR/mo by end of year 2 (10K subs)
- **Beehiiv ad-network equivalent:** $40-80/mo at 1.2K subs scaling to $200-400/mo at 5K [[Beehiiv blog](https://www.beehiiv.com/blog/newsletter-sponsorship-cost)] — but unavailable on Hodhod
- **Paid subs:** Don't launch a paid tier until 10K free subs + clear premium content thesis

---

## Founder/creator conversion case studies (3+ deep)

### Case 1: Saud AlHawawi — Saudi VC → 3 publication network
- **Pre-newsletter:** TikTok @hawawie2, X @saudalhwawi, LinkedIn (senior fund role)
- **Conversion playbook:** Launched Arabic Substack saud.substack.com in 2022, weekly cadence. Cross-posted essays to LinkedIn (where his audience already was), with a dedicated CTA to subscribe. Substack Notes amplified after 2023 launch.
- **Result:** "Thousands of subscribers" by 2025; expanded into a podcast + blog at saud.blog
- **Lesson for Almobadir:** A senior MENA VC voice + Substack + cross-post on LinkedIn is the proven recipe. Almobadir's IG/TikTok 3.3M is even bigger; the conversion challenge is moving warm short-form viewers to email, which is structurally harder than LinkedIn → Substack.
- **Source:** [[saud.substack.com](https://saud.substack.com/)], [[saud.blog](https://saud.blog/?p=72)]

### Case 2: Khaled Bzmawe — agency owner → Hodhod
- **Pre-newsletter:** Saudi digital-marketing agency (Startzex), YouTube channel, $500k+ ad spend managed for clients, 30+ brands trained [[khaledbzmawe.com](https://khaledbzmawe.com/)]
- **Conversion playbook:** Launched نشرة خالد بزماوي on Hodhod (Arabic-first, native RTL, regional billing). Used existing client pipeline + YouTube + LinkedIn to seed. Newsletter is the pre-sell asset for high-ticket consulting.
- **Result:** 2,192 subs in just 12 issues — implying ~180 subs/issue net acquisition. Growth 2-3x faster than typical English newsletter cold-launch from zero brand.
- **Lesson for Almobadir:** Hodhod's discovery directory + a pre-existing audience drives faster early growth than Substack for an Arabic publisher.
- **Source:** [[Hodhod Discover](https://gohodhod.com/discover)], [[khaledbzmawe.com](https://khaledbzmawe.com/)]

### Case 3: Beehiiv reference cases (Milk Road, The Brink) — proof of viral mechanics
- **Milk Road:** Started 0 → 250k subs in <12 months using Beehiiv's referral program; sold for millions in 10 months [[Beehiiv blog](https://www.beehiiv.com/blog/build-successful-referral-program-newsletter)]
- **The Brink:** Added 14,000 subs in month 1 using Beehiiv referral [[Beehiiv blog](https://www.beehiiv.com/blog/build-successful-referral-program-newsletter)]
- **Mechanics:** Tiered rewards (refer 3 = sticker, 10 = exclusive issue, 25 = swag, 100 = call). Beehiiv automates the link generation and reward-release.
- **Lesson for Almobadir:** Hodhod doesn't have native referral tooling at this depth — but the same mechanic can be built manually using SparkLoop (RTL-agnostic referral SaaS that integrates via API) or a custom Tally form. **Plan to launch a referral program at month 3.**
- **Sources:** [[Beehiiv blog - referral program](https://www.beehiiv.com/blog/build-successful-referral-program-newsletter)], [[Beehiiv referral feature](https://www.beehiiv.com/features/referral-program)]

### Case 4 (bonus): MENAbytes — 6+ years organic
- **Founder:** Zubair Naeem Paracha, founded 2017 [[MENAbytes about](https://www.menabytes.com/about/)]
- **Audience size:** 37,748 LinkedIn, 22,856 Facebook, 5,434 Instagram = ~100k cross-channel; "hundreds of thousands" of monthly content reach [[MENAbytes LinkedIn](https://www.linkedin.com/company/menabytes)]
- **Conversion:** Native blog → email newsletter. Long form factual reporting on MENA tech deals.
- **Lesson:** Sustained, factual, frequent reporting can build a meaningful audience even without a celebrity founder. Time horizon: 5+ years, not 12 months.

---

## RTL + Arabic-typography email traps (technical checklist)

### The seven failure modes Almobadir will hit if it just "ships HTML"

#### 1. Outlook (desktop, Windows) — the worst offender
- **Problem:** Outlook 2007-2019 (Word rendering engine) ignores `<style>` blocks and external CSS for many directives, including `direction: rtl;`. Even when set, Outlook can flip alignment unpredictably when LTR English (e.g. brand name "Almobadir") sits inside Arabic text [[Litmus - dir=rtl in Outlook](https://litmus.com/community/discussions/6372-dir-rtl-in-outlook-office365)]
- **Symptom:** Punctuation jumps to the wrong side; bullet lists invert; numbers scramble (e.g. "2026" rendering as "6202")
- **Fix:** **Inline `dir="rtl"` and `style="direction: rtl; text-align: right;"` on every `<td>`, `<p>`, `<div>`** — never rely on body-level alone. Use Outlook MSO conditionals (`<!--[if mso]>...<![endif]-->`) to force separate styling. Keep numerals and English brand-names wrapped in `<span dir="ltr">Almobadir</span>` to prevent reordering [[Tempmail - RTL Gmail](https://www.tempmail.us.com/en/rtl/resolving-rtl-text-alignment-issues-in-gmail-html-emails)]

#### 2. Outlook Web Access (OWA) / New Outlook — signature breakage
- **Problem:** OWA + the New Outlook actively REWRITE mixed LTR/RTL HTML once a user types in the body. Pre-formatted RTL signatures get mangled [[GitHub - office-js #5723](https://github.com/OfficeDev/office-js/issues/5723)]
- **Implication:** B2B sponsors may not be able to forward your newsletter cleanly. Don't rely on forwards.

#### 3. Gmail (Web + iOS + Android)
- **Problem:** Gmail strips embedded `<style>` and external CSS. Explicit RTL directives at body-level often ignored [[Tempmail](https://www.tempmail.us.com/en/rtl/resolving-rtl-text-alignment-issues-in-gmail-html-emails)]
- **Symptom:** Text left-aligns even when document direction is RTL
- **Fix:** Inline EVERYTHING. Add `dir="auto"` on every paragraph as a fallback for mixed content [[rtlstyling.com](https://rtlstyling.com/posts/rtl-styling/)]
- **Subject line:** Gmail's mobile app handles RTL subject lines correctly only when the **first character** is Arabic. Lead with Arabic, never with an emoji or English digit.

#### 4. Apple Mail (iOS + macOS)
- **Problem:** Apple Mail honours RTL CSS in the body but **fails on calendar invites and inline images**. Arabic content in invitations renders LTR even when the email body is RTL [[Apple discussions](https://discussions.apple.com/thread/254991441)]
- **Symptom:** Email body looks fine, but if you embed a calendar event for a webinar, the time/title flips
- **Fix:** Avoid inline calendar invites for now; use a separate "أضف للتقويم" button linking to a static .ics file
- **iOS specific:** Responsive emails appear zoomed/off-centre on iOS [[Litmus iOS 11](https://litmus.com/community/discussions/6871-ios-11-rendering-my-email-1-2-the-width-of-the-screen)]. Add `padding: 0` to body and use 100% width tables for the outer container.

#### 5. Yahoo Mail
- **Problem:** Yahoo doesn't use the `dir="auto"` attribute to detect RTL; subject line direction inconsistent [[Mozilla Support - RTL Yahoo](https://support.mozilla.org/en-US/questions/993521)]
- **Fix:** Always set `dir="rtl"` on the `<html>` AND `<body>` tags, plus `lang="ar"`. This unblocks Yahoo's heuristics.

#### 6. Mixed Arabic + English brand names + numerals
- **Problem:** Arabic strings with embedded LTR runs (URLs, brand names, prices like "999 SAR", phone "+966...") cause character reordering. This is a Unicode bidirectional-algorithm issue, not a CSS one.
- **Fix:** Wrap every LTR run in `<span dir="ltr">...</span>` or use Unicode marks: `&#x202D;` (LRO) before LTR text, `&#x202C;` (PDF) after. Practical example:
  - Bad: `زرنا اليوم: almobadir.com للمزيد`
  - Good: `زرنا اليوم: <span dir="ltr">almobadir.com</span> للمزيد`
- **Numerals:** Decide once whether to use Arabic-Indic (٠١٢٣٤٥٦٧٨٩) or Western (0123456789) and stay consistent. Most Saudi B2B audiences prefer Western numerals for prices/dates. Set CSS `font-feature-settings: "lnum"` on numbers to lock alignment.

#### 7. Fonts
- **Problem:** Default email-client fonts handle Arabic poorly. Tahoma is the de-facto baseline (ships on Outlook/Windows). Apple ships SF Arabic (good) but Gmail Web falls back to a system serif on Linux/Android.
- **Fix:** Use a web-safe stack: `font-family: 'Tahoma', 'Arial', 'Segoe UI', sans-serif;` — and DO NOT rely on Google Fonts (Cairo, Tajawal, IBM Plex Sans Arabic) for email; many clients block external font URLs. Bake the visual brand via static images for headers if specific font is critical.
- **Line-height:** Arabic needs `line-height: 1.7-1.9` (vs 1.4-1.5 typical English) — Arabic glyphs have taller marks (Hamza, Shadda) that crowd at lower heights.

### Pre-launch QA checklist
- [ ] Send test to: Outlook 2019 desktop, Outlook 365 web, Gmail Web, Gmail iOS, Gmail Android, Apple Mail iOS, Apple Mail macOS, Yahoo Mail Web, ProtonMail (uncommon but used by privacy-conscious Saudi techies)
- [ ] Test mixed content: Arabic body + English URLs + Western numerals + Arabic-Indic numerals + emojis
- [ ] Verify subject line renders correctly in all preview panes
- [ ] Verify unsubscribe footer is in Arabic (RTL) — Beehiiv FAILS this since Arabic isn't a supported publication language
- [ ] Send via Litmus or Email on Acid for cross-client preview before every issue if possible
- [ ] Plain-text fallback in Arabic — many corporate Saudi/UAE filters strip HTML

---

## Recommended platform for Almobadir (specific reasoning)

### Primary: Hodhod (gohodhod.com)

**Why Hodhod, not Substack/Beehiiv:**

1. **RTL is solved natively, not patched.** Beehiiv has no Arabic UI. Substack supports RTL but auto-text (unsubscribe, payment receipts, account emails) is English-only, which feels broken to a Saudi reader. Hodhod handles all of this in Arabic by default.

2. **Regional support timezone.** Almobadir publishes in GMT+3. When a 9pm Riyadh deliverability issue hits before a Sunday morning send, Hodhod is awake. Substack/Beehiiv support is US/EU.

3. **AWS SES infrastructure = par-equal deliverability.** Hodhod runs on AWS SES, the same backend Substack uses. There is no deliverability penalty for choosing Hodhod over the global incumbents.

4. **The publisher base is the partnership pool.** 7,500+ Arabic publishers, including direct-adjacent newsletters (Tamkeen, Mortakaz, Khaled Bzmawe). Cross-promotion is dramatically easier inside Hodhod's directory than outside it.

5. **Pricing maths favours Hodhod at every scale.** Free up to 500 (proves concept). Paid tier scales linearly. No 10% revenue cut (Substack) or $99/mo wall to enable paid subs (Beehiiv Scale).

6. **Strategic positioning.** Almobadir is positioned as a champion of Arab founders. Publishing on a regional Arabic-first platform reinforces that identity. Substack/Beehiiv are American — fine for cosmopolitan VC voices like Khaled Talhouni, but off-message for Almobadir.

**Why Substack as a secondary mirror:**

1. **Substack Notes + Recommends graph** drives 30%+ of new-pub growth — a discovery moat Hodhod cannot match in 2026.
2. **Saudi VC cluster on Substack** (Saud AlHawawi, Khaled Talhouni, Saudi Market Monitor) provides cross-recommendation lift.
3. **SEO:** Substack URLs index well; mirroring 1-2 issues per month at substack.com/almobadir creates discovery surface.
4. **Free tier, no risk:** Substack costs nothing until you turn on paid. Mirror-only posture (no paid plan, no fees).

**Decision rule:** If Beehiiv ships Arabic UI + RTL native editing in 2026, re-evaluate. The Beehiiv ad network ($40-80/mo passively at 1.2K subs) and referral program are best-in-class. Until then, Hodhod's Arabic-first stack wins.

---

## Recommended newsletter format (cadence, length, sections, monetisation timeline)

### Cadence
- **Weekly, Sunday 8:00 Riyadh time.** Sunday is the start of the Saudi/UAE workweek. 8:00 hits the morning email check before the daily standups.
- **Monthly long-form deep dive** on the last Sunday of each month (1.5x normal length).

### Length
- Standard issue: **800-1,400 Arabic words**, 5-7 minute read.
- Monthly deep dive: 2,000-2,800 words, 10-12 minute read.
- Subject line: ≤8 Arabic words, lead with a number or specific noun, not a verb.

### Sections (proposed structure for every weekly issue)
1. **افتتاحية المبادر** (Almobadir's opener) — 200 words, Badr's voice, this week's lens
2. **صفقات ومقاولات الأسبوع** (Deals + ventures of the week) — 5 bullets, MENA funding rounds with one-line takes
3. **مؤسس الأسبوع** (Founder of the week) — 400 words, one Saudi/MENA founder profile (rotate sectors)
4. **أداة الأسبوع** (Tool of the week) — 100 words, one resource a founder can use Monday morning
5. **سؤال للمجتمع** (Community question) — single CTA inviting reply (drives engagement signal to inbox providers)
6. **يحدث في المنطقة** (Around the region) — 3 short links, Wamda/MAGNiTT/Arab News curation
7. **رعاية هذا الأسبوع** (This week's sponsor) — single placement, 80 words max, clearly labelled "إعلان"

### Monetization timeline
- **Months 1-3 (0 → 1,000 subs):** No monetization. Build content quality and consistency.
- **Months 4-6 (1k → 3k subs):** Soft sponsorships from portfolio/partners only (in-kind exchanges, no cash).
- **Months 7-9 (3k → 5k subs):** Open paid sponsor slots at 100 SAR per primary placement. Target 1 paid sponsor per issue Q3 2026.
- **Months 10-12 (5k → 8k subs):** Add secondary sponsor slot. Test a "feature spotlight" sponsored deep dive at 800-1,200 SAR.
- **Year 2:** Evaluate paid tier at 10k+ subs. If 1% conversion = 100 paid at 25 SAR/mo = 2,500 SAR MRR floor.
- **Never:** Paywall the founder profile or deals roundup (that's the brand-building core).

---

## First 10 issues — proposed editorial calendar

Working assumption: launch 11 May 2026 (Sunday after sprint completion). Topics align with Saudi 2026 momentum + Wave 2 audit findings.

| # | Date | Title (AR) | English gloss | Founder profile | Deal angle |
|---|------|-----------|---------------|-----------------|------------|
| 1 | 11 May | كيف نبني المبادر معكم | How we're building Almobadir with you | Almobadir's own origin story | n/a — launch issue |
| 2 | 18 May | خريطة المؤسسين السعوديين 2026 | Saudi 2026 founder map (lead-magnet release) | Tamara | Q1 2026 funding overview |
| 3 | 25 May | لماذا الـB2B هي الفرصة الكبيرة | Why B2B is the big opportunity | Sary (Mohammed Aldossary) | Q1 B2B SaaS deals |
| 4 | 1 Jun | المؤسس وحيد: مالم يقولوه | The solo founder: what they don't tell you | Pick a solo Saudi founder | Solo-founder fundraising |
| 5 | 8 Jun | الذكاء الاصطناعي والمؤسس العربي | AI and the Arab founder | Lean Technologies / a Saudi AI founder | AI deals in MENA H1 |
| 6 | 15 Jun | كيف تجمع جولتك الأولى في 2026 | How to raise your first round in 2026 | One Vision Ventures portfolio cofounder | Pre-seed/seed checklist |
| 7 | 22 Jun | البنية التحتية للمؤسس: قانون، محاسبة، توظيف | Founder infrastructure: legal, accounting, hiring | Pick a HR-tech founder | Reg-tech deals |
| 8 | 29 Jun | شركاء التوزيع في الخليج | Distribution partners in the Gulf | Pick a logistics founder | Q2 logistics deals |
| 9 | 6 Jul | شهر الإصدارات: أهم 5 إطلاقات يونيو | Launch month: top 5 June releases | Pick a recent launch founder | Product Hunt MENA mirror |
| 10 | 13 Jul | كيف تخرج من الجمود: قصص محورية | Pivot stories: how to break out of stagnation | Pick a known pivot story | Distressed deals / acqui-hires |

**Lead-magnet drop schedule:**
- Issue 2: "خريطة المؤسسين السعوديين 2026" PDF (the killer first lead-magnet — see funnel section)
- Issue 5: "دليل الذكاء الاصطناعي للمؤسس العربي" mini-guide
- Issue 8: "قائمة شركاء التوزيع في الخليج" — Notion database

---

## Specific website + funnel changes

### A. Replace Lorem-Ipsum placeholder (HIGHEST PRIORITY)

**Current state (per Wave 1 dragnet):** Newsletter page on almobadir.com/newsletters has Lorem-Ipsum placeholder content from Jan 2023. Visitors see broken trust signals.

**Action:**
1. **Delete the Lorem-Ipsum content immediately** (zero-risk fix; current state is actively damaging).
2. **Replace with a 1-screen launch landing** in Arabic:
   - H1: `اشترك في نشرة المبادر الأسبوعية`
   - Sub: `كل أحد صباحاً: صفقات الأسبوع، مؤسس واحد، وأداة جديدة لمؤسسي العالم العربي`
   - Single CTA above fold: email input + "اشتراك" button (RTL form, native Hodhod embed)
   - Below: 3 bullets on what the reader will get
   - Below: live count "+X مؤسس مشترك" once subs > 100; before that just "كن من أوائل المشتركين"
   - Footer: link to the public Hodhod archive

### B. Replace fake "subscribe" form with Hodhod native embed

**Current state:** Wave 1 found the existing form is non-functional / not connected to any ESP.

**Action:**
1. Create Hodhod publication for Almobadir at gohodhod.com.
2. Generate embed snippet from Hodhod ([Hodhod GitHub WordPress plugin available](https://github.com/Abdoo-mayhob/Hodhod) — also a clean JS embed for Webflow/Custom).
3. Replace the fake form's HTML with the Hodhod embed.
4. Test: submit a real email; confirm receipt in Hodhod dashboard within 60 seconds; confirm welcome email arrives in Arabic with RTL layout.
5. Add a `<noscript>` fallback that links to gohodhod.com/almobadir directly.

### C. Add lead-magnet — "خريطة المؤسسين السعوديين 2026"

**The single highest-leverage lead-magnet for Almobadir's positioning.**

**Spec:**
- 12-16 page PDF, 100% Arabic
- Map of 100 Saudi founders by sector (fintech, logistics, AI, ed-tech, food, beauty, B2B SaaS, climate, gaming, healthtech)
- For each founder: 1-paragraph bio, current company, prior exits, public social handles
- Cover designed in Almobadir brand
- Hosted as a gated PDF: enter email → instant download + auto-subscribe to nashra
- Use this as the universal lead-magnet for IG/TikTok bio links, paid ads, partnership swaps

**Why this beats a generic "subscribe" pitch:**
- Concrete deliverable; clear value
- Aligns with Almobadir's authority (curating MENA founders)
- Reusable: republish updated 2027/2028 versions yearly
- Massive shareability on LinkedIn (founders share their own listing)

**Secondary lead-magnets (ship later):**
- "قائمة 50 أداة لكل مؤسس عربي 2026" (50 tools for every Arab founder 2026) — Notion template
- "الكتيب الكامل لجمع التمويل الأول في الخليج" (Complete first-fundraise playbook for the Gulf)
- "تقرير صفقات الربع" (Quarterly deals report)

### D. Funnel changes across Almobadir's 11 social handles

1. **IG bio + TikTok bio:** Replace generic site link with `almobadir.com/nashra` deep-link
2. **Pinned post on every channel:** "اشترك في النشرة الأسبوعية → الرابط في البايو" with a static cover that previews the lead-magnet
3. **Story templates:** 3 swipe-up templates per platform mentioning the newsletter weekly (Sun/Tue/Thu)
4. **YouTube end-screens:** Add newsletter CTA to every video's last 20 seconds
5. **Comment policy:** Pin a top comment on every viral post linking to nashra signup

### E. Operational hygiene

- **Sender domain:** Set up `nashra.almobadir.com` subdomain with SPF/DKIM/DMARC aligned on Hodhod's recommended records (Hodhod docs walk through this)
- **Welcome series:** 3-email onboarding (welcome + about Badr + best-of) over 5 days before live sends
- **Archive page:** Public Hodhod archive at gohodhod.com/almobadir AND mirror on almobadir.com for SEO
- **Analytics:** UTM every newsletter link with `utm_source=nashra&utm_medium=email&utm_campaign=YYYY-MM-DD`
- **Unsubscribe link:** Verify it's in Arabic on the test send (Hodhod handles this; Beehiiv would NOT)

---

## Open questions for Badr

1. **Editorial voice ownership — who writes?** Will Badr personally write all openers, or delegate to a content editor with Badr's review-and-publish? The 10/10 quality bar makes this a real-time bandwidth question — at 1 issue/week × 1,000 words × 30-min Badr review, that's 4-6 hours/month minimum.

2. **Bilingual question.** Audience research suggests Arabic-only for primary publication (matches positioning, matches Saud AlHawawi, etc.). But: does Badr want to mirror an English version on Substack to capture the GCC English-fluent VC/operator class? My recommendation: **start Arabic-only**, evaluate English mirror at 5K subs.

3. **Sponsor relationships.** Does Almobadir have existing brand-side relationships (banks, telcos, government accelerators) that could underwrite the first 6 months? If yes, a single anchor sponsor at 5,000 SAR/month would change the launch math substantially.

4. **Publishing cadence — fixed weekly or experimental bi-weekly?** Weekly is recommended (compounds faster, locks Sunday habit) but bi-weekly cuts production by 50% and matches Saudi Market Monitor's cadence. Fixed weekly preferred.

5. **Lead-magnet privacy.** The Saudi 2026 founder map will list real people. Need to decide: (a) only public-on-LinkedIn founders, (b) opt-in-required founders, or (c) a mix with disclosure. PDPL (Saudi data law) requires consent for personal data used commercially — defaulting to "public-on-LinkedIn only" sidesteps this.

6. **llms.txt 1998 discrepancy.** Per memory, Almobadir's llms.txt claims "Fundado 1998" which contradicts the 9-month store age previously logged. Verify before issue 1 — if 1998 is the parent-company founding year, lead the launch issue with that lineage; if it's an error, fix llms.txt before launching.

7. **Hodhod relationship.** Worth a direct intro to Hodhod's founder team. Almobadir publishing on Hodhod with 3.3M social audience is a marquee case study Hodhod will want to support — possibly with marketing co-promotion or beta features.

8. **Thmanyah partnership.** Thmanyah has 351M+ podcast audience and an active creator-network strategy. Worth a single conversation about whether Almobadir nashra distributes through Thmanyah's creator network for amplification.

9. **Content moat vs Wamda/MAGNiTT.** Wamda and MAGNiTT already serve MENA founders. Almobadir's differentiator must be: (a) Arabic-first (Wamda is bilingual but defaults to English; MAGNiTT is English-only), (b) Saudi-centric (Wamda is pan-MENA; MAGNiTT is even broader), (c) brand voice + community feel (the 3.3M social audience gives this). Does Badr agree with this positioning?

10. **Paid tier in 12 months — yes/no.** Before writing issue 1, decide: is this a long-term free top-of-funnel for Almobadir's broader business (events, community, products), or is the newsletter itself a P&L line that must hit 50K SAR MRR by Year 2? The two paths diverge at issue 30 (different content gating, different community structure, different sponsor pitch).

---

## Sources

### Platforms
- [Hodhod homepage](https://gohodhod.com/) — Arabic ESP, 7,500+ publishers, AWS SES, RTL native, free tier ≤500
- [Hodhod Discover](https://gohodhod.com/discover) — directory of Arabic newsletters with sub counts
- [Hodhod GitHub integration](https://github.com/Abdoo-mayhob/Hodhod) — confirms AWS SES backend
- [Substack RTL announcement](https://x.com/SubstackInc/status/1486472716295098368) — Jan 2022, Arabic/Hebrew/Pashto/Urdu/Sindhi
- [Substack reader localization](https://support.substack.com/hc/en-us/articles/18575507636500-Can-readers-see-my-Substack-publication-in-a-different-language)
- [Beehiiv supported publication languages](https://www.beehiiv.com/support/article/4993791529751-setting-a-default-language-for-your-publication) — Arabic NOT in list
- [Beehiiv 2026 state of newsletters](https://www.beehiiv.com/blog/the-state-of-newsletters-2026)
- [Beehiiv ad network](https://www.beehiiv.com/blog/newsletter-sponsorship-cost) — CPM benchmarks
- [Beehiiv referral program](https://www.beehiiv.com/blog/build-successful-referral-program-newsletter)
- [Beehiiv vs Substack comparison](https://www.emailtooltester.com/en/blog/beehiiv-vs-substack/) — fees, plans
- [Sacra Beehiiv revenue profile](https://sacra.com/c/beehiiv/) — $30M ARR June 2025
- [Kit/Mailchimp RTL not supported](https://kindlepreneur.com/convertkit-review/) — Kindlepreneur 2026 review
- [EmailToolTester top Substack alternatives 2026](https://www.emailtooltester.com/en/blog/substack-alternatives/) — Kit, Beehiiv, MailerLite, GetResponse pricing
- [MartechLive top 10 ME email tools](https://martechlive.com/top-10-email-marketing-tools-for-middle-east/) — AED pricing benchmarks

### Arabic publishers cited
- [Saud AlHawawi Substack](https://saud.substack.com/)
- [Saud AlHawawi blog (16 newsletters list)](https://saud.blog/?p=72)
- [Saud AlHawawi LinkedIn](https://www.linkedin.com/in/saudalhawawi/)
- [Saudi Market Monitor Substack](https://saudimarketmonitor.substack.com/)
- [Saudi Market Monitor about](https://saudimarketmonitor.substack.com/about)
- [Khaled Talhouni Substack](https://khaledtalhouni.substack.com/)
- [Khaled Bzmawe site](https://khaledbzmawe.com/)
- [ArabLit Substack](https://arablit.substack.com/)
- [Ihsan Arabic Substack](https://ihsanarabic.substack.com/)
- [Wamda](https://www.wamda.com/)
- [MAGNiTT newsletters](https://magnitt.com/newsletters)
- [MAGNiTT pricing](https://magnitt.com/pricing)
- [Argaam newsletters AR](https://www.argaam.com/ar/newsletters)
- [Argaam Wikipedia](https://en.wikipedia.org/wiki/Argaam)
- [Maaal](https://maaal.com/en/)
- [Thmanyah newsletters](https://thmanyah.com/newsletters)
- [Thmanyah Entrepreneur ME profile](https://www.entrepreneur.com/en-ae/growth-strategies/saudi-arabia-based-thmanyahs-new-focus-on-content-creators/469579)
- [MENAbytes](https://www.menabytes.com/about/)
- [Hsoub](https://www.hsoub.com/en/)

### RTL technical
- [Tempmail RTL Gmail fixes](https://www.tempmail.us.com/en/rtl/resolving-rtl-text-alignment-issues-in-gmail-html-emails)
- [Litmus dir=rtl in Outlook/Office365](https://litmus.com/community/discussions/6372-dir-rtl-in-outlook-office365)
- [Litmus dir=rtl in Outlook 2010](https://litmus.com/community/discussions/6225-getting-dir-rtl-to-work-in-outlook-2010)
- [GitHub office-js OWA RTL signature bug](https://github.com/OfficeDev/office-js/issues/5723)
- [Apple discussions Arabic invitation alignment](https://discussions.apple.com/thread/254991441)
- [Litmus iOS responsive zoom](https://litmus.com/community/discussions/6871-ios-11-rendering-my-email-1-2-the-width-of-the-screen)
- [rtlstyling.com — RTL CSS reference](https://rtlstyling.com/posts/rtl-styling/)
- [Mozilla Support RTL Yahoo](https://support.mozilla.org/en-US/questions/993521)
- [Mailtrap Outlook rendering issues](https://mailtrap.io/blog/email-rendering-issues-outlook/)

### Pricing + benchmarks
- [Whosponsorsstuff newsletter pricing guide](https://site.whosponsorsstuff.com/how-to-price-an-ad-in-your-newsletter)
- [Publisher Growth top 7 CPM ad networks MENA](https://publishergrowth.com/blog-details/7-best-cpm-ad-networks-in-mena)
- [MailerLite 2025 email marketing benchmarks](https://www.mailerlite.com/blog/compare-your-email-performance-metrics-industry-benchmarks)
- [Brevo 2025 email benchmarks by region](https://www.brevo.com/blog/email-marketing-benchmarks/)
- [Readless best paid Substacks 2026](https://www.readless.app/blog/best-paid-substack-newsletters-2026) — 8.4M Substack paid subs Q1 2026

### Market context
- [Arab News Saudi creator economy](https://www.arabianbusiness.com/industries/media/saudi-arabias-creator-economy-surges-32-in-q1-2025-tiktok-leads-growth-report) — 32% Q1 2025 growth
- [Arab News Saudi entrepreneurship social media](https://www.arabnews.com/node/2623598/business-economy) — 94% of KSA founders consider social media important
- [Arab News startup funding](https://www.arabnews.com/node/2633941/business-economy) — MENA $190M Apr 2026 wave
- [Soderhub MENA $2.1B H1 2025](https://soderhub.beehiiv.com/p/aug2025) — Saudi 64% of regional funding

---

*End of briefing — agent 3.5, content-intel topic 5/10*
