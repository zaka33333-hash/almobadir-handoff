# @almobadir_business (IG) — content harvest

**Captured:** 2026-04-27 via Chrome (logged in as Badr Shaker — confirmed by `is_professional_account: true` flag in IG response and zaka_att session reused from dragnet-loggedin.md). Read-only API hits to `/api/v1/users/web_profile_info/`, `/api/v1/feed/user/{uid}/`, and `/graphql/query/?doc_id=...` (shortcode_media). No follows/likes/posts/DMs issued. 96 posts captured spanning 2024-02-14 → 2026-04-22 (full chronological tail of the 464 lifetime posts back to 26 months ago).

---

## Snapshot

| Field | Value |
|---|---|
| Handle | @almobadir_business |
| Display name | المبادر \| مال و أعمال (Almobadir \| Money & Business) |
| Category | Education *(NOT "Business" or "Marketing Agency" — telling)* |
| `is_professional_account` | **true** *(verified via API)* |
| `is_business_account` | **false** *(uses Creator account, not Business)* |
| `is_verified` | false |
| Followers | **125,233** |
| Following | **50** *(extremely tight follow list — curated)* |
| Posts | 464 lifetime |
| Highlight reel count | **0** *(no story highlights — major gap vs sibling handles)* |
| Bio external_url | **null** |
| Bio links array | **`[]`** *(zero clickable links anywhere)* |
| Threads onboarded | yes (cross-link badge from dragnet) |
| Business email/phone | null/null |
| `has_clips` | true |
| `has_channel` (broadcast) | false |

The IG API response is unambiguous: this account is a follower magnet with **zero outbound funnel infrastructure**. Compare to @badrshaqer which has `external_url = zaap.bio/badrshaqer` driving to a Calendly. @almobadir_business has nothing.

---

## Bio + funnel

**Bio (verbatim, AR):**
> نعيش في عالم يحمه : البزنس 💰
> فهم البزنس = فهم العالم 🌎
> لهذا نبسط ونكتشف معك كواليس صناعة المال
> بأسرارها، قصصها ومفاهيمها 😵‍💫

**Bio (English working translation):**
> We live in a world ruled by: business 💰
> Understanding business = understanding the world 🌎
> So we simplify and uncover with you the behind-the-scenes of money-making —
> its secrets, stories, and concepts 😵‍💫

**Note on first line:** the verb is `يحمه` — appears to be a typo for `يحكمه` ("rules it") or `يحميه` ("protects it"). Either reading, the spelling stands as published. A premium-positioning brand should clean this typo.

**Bio link (external_url):** `null`. **Bio link (`bio_links` array):** `[]` (empty). There is no Linktree, no zaap, no calendly, no website, no email. The only "link" in the IG funnel is the Threads handle `@almobadir_business` (cross-platform IG/Threads native link, not a true outbound destination).

**Contact options:**
- `business_email`: null
- `business_phone_number`: null
- DM ("Message" button) is the only contact channel exposed

**Funnel verdict:** zero. The 125K-follower channel passes traffic onto Threads (already-IG-owned) and to in-product DMs. There is **no website link, no consultation link, no newsletter capture, no lead magnet, no agency CTA**. Compared to `@almobadir_media` (57.7K, marketing agency) which at least lists service-category story highlights, `@almobadir_business` is a content-only top-of-funnel with no plumbing.

---

## Posting cadence + format mix

**Live cadence reading (newest 30 posts captured):**
- Newest post: **2026-04-22** (5 days ago, 4 in absolute days)
- Oldest of the most-recent-30: **2025-09-07**
- Span: **227 days for 30 posts → ~0.93 posts/week, ~1 post every 7.6 days**
- Posts in true rolling-30-day window (2026-03-28 → 2026-04-27): **4** *(roughly 1/week active rate currently)*
- Posts in rolling-60-day window: **8**
- Posts in rolling-90-day window: **14**

**Verdict:** the account is on a **slow-burn cadence** of ~1 post per week — far below the multi-post-per-day rate that drives growth on IG in 2026. For a 125K-follower account that hasn't grown its highlight-reel count above 0, this looks like a **maintenance mode** posture rather than an active build.

**Format mix across the 96 captured posts:**

| Format | Count | % |
|---|---|---|
| Carousel (`media_type=8`) | 52 | 54% |
| Reel/Clip (`product_type=clips`, `media_type=2`) | 39 | 41% |
| Single photo (`media_type=1`) | 5 | 5% |
| Single video feed | 0 | 0% |
| IGTV / story | 0 | 0% |

**Read:** carousel-dominant. The pinned posts are 2 carousels + 1 reel. Reels are still the engagement champions for plays (1.7M+ on the Rolex story), but the team's *cadence default* is the carousel format. That fits the "education" category tag.

---

## Top 5 last-30-days

The literal last-30-days window only contains **4 posts**, so the table below extends to last-90 days (14 posts) so it actually delivers 5 posts. Sorted by like count.

| # | URL (`/p/` or `/reel/`) | Format | Date | Likes | Comments | Plays | Hook (≤30 words, AR) |
|---|---|---|---|---|---|---|---|
| 1 | `/p/DVRLSI5DN2N/` | Carousel | 2026-02-27 | **27,949** | 1,197 | — | "🔥 أي شعلة تحترق عندك الآن ؟! 🤔💭" *(Which flame is burning in you right now? — generic motivational prompt, network cross-promo) — PINNED* |
| 2 | `/p/DW6uf1mDK6D/` | Carousel | 2026-04-09 | 7,017 | 35 | — | "الوسطاء هم الأكثر جنيا للأموال 💵💰" *(Middlemen earn the most money) — economics/business model take* |
| 3 | `/p/DVEUAkdDMnC/` | Carousel | 2026-02-22 | 4,010 | 22 | — | "هل هناك مصطلحات اخرى في البزنس تجهل معناها ؟ 🤔💭" *(Are there other business terms you don't know? — glossary loop, comment-bait)* |
| 4 | `/p/DVyfKeQDCkT/` | Carousel | 2026-03-12 | 3,726 | 18 | — | "هل لديك مصطلحات اخرى في البزنس تريد معرفة معناها ؟ 🤔💭" *(Same glossary loop — different terms, same hook structure)* |
| 5 | `/p/DU1Iw7djJ_k/` | Carousel | 2026-02-16 | 3,526 | 55 | — | "ما هو الرفض الذي غير مسار حياتك للأفضل ؟ شاركنا قصتك ... ✍️" *(What rejection changed your life path for the better? Share your story — founder-story bait)* |

**Read:** the network-cross-promo motivational pinned post crushes everything (3.5x the next best). When you strip the pinned, the **best-performing originals are business-glossary carousels and "tell us your founder story" prompts** — actual B2B-relevant hooks.

---

## Top 5 all-time

Sorted by like count over the captured 96-post window (covers Feb 2024 → Apr 2026, the entire active life of this handle since the cross-platform launch):

| # | URL | Format | Date | Likes | Comments | Plays | Hook (translated) |
|---|---|---|---|---|---|---|---|
| 1 | `/reel/C4i7ZpNtU2h/` | Reel | 2024-03-15 | **56,991** | 126 | **1,721,598** | *"I would heal, grow, never disdain a profit, never buy a sheikh"* — Uthman ibn Affan quote, motivational/Islamic finance cross |
| 2 | `/reel/C9pvV0pMco6/` | Reel | 2024-07-20 | 49,925 | 184 | 1,710,716 | *"Why doesn't Rolex sponsor any football player?"* — brand-strategy explainer **(PINNED)** |
| 3 | `/reel/DIRhU12OqjW/` | Reel | 2025-04-10 | 40,304 | 259 | 967,059 | *"From the door of begging to the doors of success"* — rags-to-riches story |
| 4 | `/p/DVRLSI5DN2N/` | Carousel | 2026-02-27 | 27,949 | 1,197 | — | *"Which flame is burning in you right now?"* — network cross-promo PINNED |
| 5 | `/reel/DG01kqOOKSq/` | Reel | 2025-03-05 | 25,980 | 50 | 748,515 | *"What do you think of Jack Ma's saying — agree or different path?"* — Jack Ma quote |

**Pinned posts (3 total, with full captions retrieved):**
1. `/p/DVRLSI5DN2N/` (2026-02-27, carousel) — Network cross-promo with generic motivational hook + tags to all 5 sibling handles. **27,949 likes / 1,197 comments / pinned in Feb-26.**
2. `/p/DCcYFh4MVDo/` (2024-11-16, carousel) — *"Did you know top entrepreneurs don't own their companies?"* — explainer carousel. 15,719 likes / 179 comments.
3. `/reel/C9pvV0pMco6/` (2024-07-20, reel) — *"Why doesn't Rolex sponsor footballers?"* — 49,925 likes / 184 comments / 1.71M plays.

**All-time engagement health:**
- Mean engagement rate (likes+comments / 125K followers) across all 96 = **3.97%**
- Mean ER over last 90 days = **2.94%**
- Mean ER over last 30 days = **1.79%** *(declining trend — losing reach)*
- Median post: **1,092 likes / 22 comments** *(low-mid for 125K followers)*
- 21 of 96 posts (22%) under 500 likes
- 13 of 96 posts (14%) over 10K likes — these are the home runs
- 5 posts with zero comments (mostly reels with `comment_count: 0` — probably comments-disabled rather than zero engagement)

---

## Topic distribution

After stripping the cross-promo boilerplate from each caption (the team appends the same 5-handle network plug to every post — present in 40 of 96 captions, ~42%), the actual topic mix on what the post is *about*:

| Bucket | Count (of 96) | % | Example shortcode + hook |
|---|---|---|---|
| Founder / Startup / Entrepreneurship | 33 | 34% | `DWrXXdDjG_A` *("Are you launching your startup right now?")*, `DCcYFh4MVDo` *("top entrepreneurs don't own their companies")* |
| Macro / Markets / Economy | 19 | 20% | `DFdD9N3OdQg` *("Visa & Mastercard antitrust trial")*, `DF0zWxDIILt` *("will diamond prices keep rising?")* |
| Conversation prompt / Polls | 17 | 18% | `DWXE8erjDJa` *("which social class are you?")*, `DG01kqOOKSq` *("agree with Jack Ma or different path?")* |
| Wealth / Billionaires / Net-worth | 16 | 17% | `C6MYSNfLLiC` *("how old were they when they became billionaires?")*, `DEXusrwuVGE` *("McDonald's is a real estate company")* |
| Companies / Brand case studies | 11 | 11% | `C9pvV0pMco6` *(Rolex)*, `C51BdJgskyA` *(Lulu stores)*, `DFdD9N3OdQg` *(Visa/Mastercard)* |
| Marketing / Branding / Sales | 10 | 10% | `C9pvV0pMco6` *(Rolex sponsorship)*, `C4i7ZpNtU2h` *(Uthman ethos)* |
| Sports / Football economics | 7 | 7% | `C58j0C7Myk1` *(Pérez & Real Madrid)*, `C8Xr-xWsZwD` *(club sustainability)* |
| Personal finance / Habits | 5 | 5% | `DTX3d1njAHY` *("how is tourism in your country?")*, `C8fRQs5sdgd` *(recession explainer)* |
| Motivational / Self-dev cross-pollination | 5 | 5% | `DXcVeUqjPNH` *("your current situation doesn't define your end")* |
| Career / Career-skills | 2 | 2% | `DF3SsLVoEWF` *("strangest interview question?")* |

**Cross-promo network plug (boilerplate appended to caption):** appears in **40 of 96 posts (42%)** — the team templates this 5-handle plug into nearly half of all captions. That's the core distribution mechanic: posts on this handle exist partly to **siphon the 125K base into the 4 sibling handles** (almobadir, almobadir_flousak, almobadir_mindset, shalnack, badrshaqer).

**SME founder-targeting density:** Out of 96 captures, only **35 posts** have a primary topic that maps cleanly to "advice an SME founder would search for" (founder/startup + marketing + career + personal-finance + glossary). That's ~37%. The rest skews toward **business news as entertainment** — billionaire net-worth, brand trivia, football economics, Rolex/McDonald's case studies. For an audience-targeting question, **this content tilts more toward "smart business reader" than "founder building a company".**

---

## Comment goldmine

**Top 3 most-commented posts** — and what's actually in the comments (sample of ~22-23 comments per post, 112 total comments captured):

### #1 — `/p/DVRLSI5DN2N/` (1,197 comments, pinned cross-promo, "which flame is burning in you?")
- **Top 5 comment themes:**
  1. **Religious/Quranic counter-frames:** "make the Prophet your role-model", "let your flame with Allah be the strongest" — multiple high-liked replies frame the secular motivation post in religious language
  2. **Disagreement on the "give up other flames to keep one lit" trade-off** — "this isn't accurate, our religion says give every right its due"
  3. **Personal flame statements** ("my flame is family", "my flame is education")
  4. **Pure emoji/heart reactions** (40%+ of replies)
  5. **Light dunks** ("wrong premise", "misleading framing")
- **Top tagged accounts in replies:** none surfaced from regex parse (most replies are pure text, not @-tags)
- **Recurring objections:** "this is religiously wrong", "we already have the Prophet as role-model", "you don't have to extinguish other flames"
- **Top-liked comment:** light_taha (734 likes) — a religious counter-message about the Prophet's holistic life

### #2 — `/reel/C51BdJgskyA/` (571 comments, Lulu stores story)
- **Top 5 comment themes:**
  1. **Skepticism about the rags-to-riches narrative** — "these facts are wrong; he wasn't poor; we lived through this"
  2. **Pro-Yousef/UAE praise** — "self-made man", "respects UAE government", "Allah bless him"
  3. **Counter-claims with insider knowledge** — "we've known him since the start; he doesn't really own what he runs now"
  4. **Pure praise emoji reactions**
  5. **Inheritance/uncle story corrections** — "his uncle 16 years; either you're wrong or he was already well-off"
- **Recurring objections:** "your facts are inaccurate", **content-credibility pushback is the main negative theme**

### #3 — `/reel/C6Ba-GfrhpZ/` (378 comments, $200M wedding)
- **Top 5 comment themes:**
  1. **Sectarian/regional jokes** — "must be from Bani Israel", "if he were Yemeni, he'd be selling halba in Tawahi" (large amount of regional humor)
  2. **Class-resentment takes** — "this is folly", "wedding-of-the-people-being-trampled" framing
  3. **Sarcasm/mockery memes** — "if he were in Yemen, he'd marry off all the bachelors of Yemen"
  4. **Mild praise** ("masha'Allah")
  5. **"Looks like the host of Karim Show"** — pop-culture comparisons
- **Recurring objections:** "this is excess", "performative wealth", **lower content-credibility skepticism — this is more entertainment-react than founder-react**

**Comment-thread takeaway:** the comments **do not read like a B2B founder community**. They read like a **mass-market Arab social-feed audience reacting to business-as-spectacle** — religious counters, regional humor, content-fact-checking, class-resentment takes. There are very few comments asking "how can I apply this to my business?" or tagging their co-founder. The audience is consuming this as edutainment, not as actionable business advice.

---

## CTA patterns

What does the brand actually ask viewers to DO? Counted across all 96 captions:

| CTA pattern | Posts using it | % |
|---|---|---|
| "Follow us / follow @sibling-handle" (`تابعنا`, `تابع 👈`, `تابع للمزيد`) | **67** | **70%** |
| "Interested in...? / Want to know...? / Looking for...?" (network discovery prompts that route to siblings) | 48 | 50% |
| "What do you think? / Do you agree? / Which do you choose?" (engagement bait) | 14 | 15% |
| "Comment with [word]" (`علق ب`, `اكتب في التعليقات`) | 3 | 3% |
| "Share your story / share with us" | 2 | 2% |
| "Send us [word] in DM" (`ارسل`, `الخاص`, `DM`) | 2 | 2% |
| "Save the post" (`احفظ المنشور`) | 0 | 0% |
| "Link in bio" (`رابط`, `البايو`, `bio`) | **0** | **0%** |
| "Book / Sign up" (`احجز`, `سجل`) | **0** | **0%** |

**Read:** **the only CTA on this account is "follow another @almobadir handle".** There is no save-bait, no link-in-bio, no DM keyword funnel, no agency lead-gen. The CTA pattern is **100% audience-internalization** (keep the user inside the network) and **0% commercial conversion**. For a brand sub-handle nominally focused on "money & business" with 125K followers, the absence of any commercial CTA is the single largest funnel gap.

---

## B2B audience read (with 5 supporting examples)

**Question:** does this handle pull SME founders, or business-news-for-consumers?

**Verdict: predominantly business-news-for-consumers, with a thin founder layer on top.**

Five examples that prove this read:

1. **`/reel/C6Ba-GfrhpZ/` — "$200M wedding, deserved or not?"** (21,806 likes / 378 comments). The hook is a celebrity-wealth provocation, not a founder lesson. Comments are dominated by sectarian jokes, class-resentment takes, and regional humor ("if he were Yemeni..."). **Zero comments from anyone identifying as a founder/operator** in the sample of 22 captured.

2. **`/reel/C8ziaE0qOpF/` — "Have you tried their food?"** (16,156 likes, 363 comments) — a Bik (fast-food) brand piece. Top comments: "kept eating because it's cheap", "your facts are wrong; we lived through this", "why don't they open in Syria?". **This is restaurant-brand fan/critic discourse**, not B2B operator discourse.

3. **`/p/DVRLSI5DN2N/` — pinned cross-promo, "which flame is burning?"** (27,949 likes / 1,197 comments). Top-liked comment (734 likes) is a Quranic counter about the Prophet's life balance. The whole comment thread is **religious/motivational consumer language**, not operator language.

4. **`/reel/C4i7ZpNtU2h/` — Uthman ibn Affan profit ethics** (56,991 likes / 1.72M plays — the all-time #1). Hook is a 7th-century Islamic finance quote. The narrative is **"Islamic-business inspiration for the everyday Arab Muslim"**, not "case study an SME founder can apply Monday morning". Excellent reach, weak operator-actionability.

5. **`/p/DU1Iw7djJ_k/` — "What rejection changed your life for the better?"** (3,526 likes / 55 comments). This IS a founder-prompt — but only **3,526 likes** vs the 27K-56K spike on motivational/Islamic/celebrity content. **The audience rewards entertainment business content far more than operator-prompt content.**

**Counter-evidence (the thin founder layer):**
- `/p/DVEUAkdDMnC/` and `/p/DVyfKeQDCkT/` — back-to-back business-glossary carousels — pull 4,010 and 3,726 likes respectively, with 22 and 18 comments. Engagement well above the median (1,092 likes), suggesting some audience IS reading for business literacy.
- `/p/DW6uf1mDK6D/` — "middlemen earn the most" (7,017 likes) — actual business-model thinking, performs above median.

But these founder-leaning hooks consistently underperform the pop-business celebrity content by 3-10x.

**Audience composition by signal:** the `engaged_follow_count = 50` on this handle (vs sibling @almobadir at 144) indicates a **highly curated outbound list** — almost certainly founder-friends and adjacent creators. But that outbound posture doesn't translate to inbound founder readership. The inbound 125K is **business-curious general Arab audience**, primarily Moroccan-anchored (per the dragnet-loggedin geographic finding) with strong Gulf/Levant participation in comments.

**For a B2B agency funnel (the originally intended use case):** this handle is **the worst-fit of the network** for that role. @almobadir_media (the actual marketing agency) at 57.7K is smaller but vastly better-aligned, with explicit service-category story highlights ("Marketing Services", "Strategy Services", "Creative Services"). @badrshaqer at 317K with calendly link is the actual founder funnel.

---

## LinkedIn-substitute signal + migration candidates

**Question:** Wave 1 confirmed Almobadir has NO LinkedIn company page. Is this IG handle effectively serving as the LinkedIn substitute?

**Answer: partly — but a weak substitute, and not by design.**

What @almobadir_business shares with a hypothetical LinkedIn company page:
- Education-positioned business content
- Long-form carousels (LinkedIn's native preferred format)
- Topical mix (macro, founder, brand case studies) that maps to LinkedIn's business-content surface
- Network cross-promo that would map to a "company family" LinkedIn structure

What it lacks vs a proper LinkedIn company page:
- **No employees / "people we work with" surface** (LinkedIn's primary social proof unit)
- **No services listing / "what we do" anchor** (this lives at @almobadir_media, not here)
- **No client/case-study posts** with named brands
- **No founder-led thought-leadership posts** (those live at @badrshaqer personal)
- **No long-form text or article posts** (IG enforces 2,200-char cap, killing real essays)
- **No SaaS/B2B-software topical content** — zero posts in the sample about HRtech, fintech, productivity software, e-commerce platforms, AI for SMEs
- **No "we're hiring" posts** — confirms this isn't being treated as an employer brand

**Content that would migrate cleanly to a LinkedIn page (highest signal first):**

1. **Business-glossary carousel series** (`DVEUAkdDMnC`, `DVyfKeQDCkT`, etc.) — already in carousel/document-post format that LinkedIn natively supports. Translates 1:1 to LinkedIn carousels with stronger reach there because B2B literacy content has zero competition on IG-MENA but strong organic on LinkedIn-MENA.
2. **Brand strategy case-study reels** (Rolex, Lulu, McDonald's, Visa/Mastercard) — repackage as text-first LinkedIn essays with the video as supporting media. LinkedIn's algorithm favors long-form business writing over video.
3. **"Top entrepreneurs don't own their companies" pinned carousel** (`DCcYFh4MVDo`, 15,719 likes) — this is a textbook LinkedIn essay topic and would likely outperform on LinkedIn given the narrower-but-deeper audience.
4. **Visa/Mastercard antitrust** (`DFdD9N3OdQg`, 12,611 likes) — perfect for a LinkedIn news-commentary post.
5. **Founder-prompt carousels** ("what rejection changed your life", "are you launching your startup") — these belong on a personal LinkedIn (Badr Shaker) more than a company LinkedIn, because LinkedIn rewards individual authorship of vulnerable founder content.

**What does NOT migrate:** the celebrity-wealth/$200M-wedding/Lulu-rags-to-riches content. That's IG/TikTok dopamine and would feel out of place on LinkedIn.

**Strategic read:** if Almobadir wants the agency funnel, **a LinkedIn company page launched at almobadir-media (not at almobadir_business) — with a content slice repurposed from the business-glossary + brand-case-study posts harvested here** — is the highest-leverage move. This IG handle is too lifestyle-skewed to be a clean LinkedIn substitute as-is.

---

## 7 actionable insights for the website (esp. founder-bio framing + agency network card)

1. **Don't quote the @almobadir_business 125K follower number on the agency website without context.** This handle is the **least operator-aligned** of the 5-handle network and its audience is a general Arab business-news-curious public, not SME founders. If the website's "agency network card" needs a flagship follower count, use **@badrshaqer (317K, business-consultant categorized, explicit calendly funnel)** as the founder credibility anchor and let `@almobadir_business` slot as "money & business storytelling for the broader Arab audience" — accurate, defensible, and doesn't oversell the lead source.

2. **The Threads handle is the only outbound link from this 125K-follower account.** No website, no zaap, no calendly, no email. If the website is supposed to be the funnel destination for this handle, **it currently isn't, by design**. Action: as soon as the new website is live, update `external_url` on @almobadir_business to the agency landing page (or a story-network landing page that segments for "I'm a founder" vs "I'm a reader"). This is a 1-click change with potentially the highest leverage in the entire network funnel.

3. **The bio has a likely typo (`يحمه` for `يحكمه` or `يحميه`).** A premium agency website should not link out to a bio with a typo as social proof. Fix the typo before screenshotting the IG profile for an "as featured in" or "our network" gallery on the new website.

4. **`highlight_reel_count = 0` is an unforced error.** The sibling handle @almobadir_media uses story highlights as a **B2B service catalog** ("Marketing Services", "Strategy Services", "Creative Services", "Our Services"). @almobadir_business has zero highlights despite being the brand's nominal "money & business" hub. Action for the website: cite the @almobadir_media highlights model (services-as-highlights) on the agency-services page as the canonical product surface, NOT @almobadir_business.

5. **CTA pattern is 0% commercial.** The phrase "link in bio", "book a session", "join the newsletter", and "save the post" appear in **0 of 96 captions**. The website should not assume IG traffic arrives commerce-ready — they arrive content-warm but CTA-cold. Action: the website's first-screen above-the-fold for this traffic source needs to do the work of "what is this, what's it for, what's the next step" because the IG side never primes those questions. Lean on the @almobadir_media services framework in the network card, not the @almobadir_business voice.

6. **The cadence is ~1 post/week, the engagement rate is 1.79% in last 30 days (declining from 3.97% all-time).** This is a **maintenance-mode account**, not a growth account. If the website's agency-network card is built off this handle's metrics as "proof of distribution power", that proof is **decaying**. Action: the agency network card should bias toward @badrshaqer (founder, growing) and @almobadir_media (agency-positioned) rather than the slowing @almobadir_business. Lead with: founder personal brand → agency identity → adjacent reach handles, in that order.

7. **The founder-bio framing on the website should NOT lean on this handle's follower count for marketing-credentials proof.** The dragnet's MBA-claim discrepancy from `dragnet-loggedin.md` already flagged unverified credential copy. Now add: this handle's 125K is mostly business-as-spectacle audience, not marketing-buyer audience. The honest founder bio anchors are: **Badr Shaker, business consultant (verified IG category), 317K personal IG following, runs Almobadir media network of 5 handles totaling 1.5M+ aggregated reach across IG, founder of Almobadir Media (creative marketing agency, 57.7K IG)**. That framing survives a buyer's due-diligence search; "we have 125K business followers" doesn't, because the buyer can spend 30 seconds on the comments and see the audience composition.

---

## Appendix — captured data

- **96 posts** retrieved via `/api/v1/feed/user/32915734606/?count=30` paginated 8x
- **112 comments** sampled across top-3 most-commented posts via GraphQL `shortcode_media` (`doc_id=8845758582119845`)
- **Profile metadata** retrieved via `/api/v1/users/web_profile_info/?username=almobadir_business`
- All data captured **2026-04-27**, read-only, in user's logged-in Chrome session
- IG `user.id` for @almobadir_business = `32915734606` (recorded for reproducibility — NOT a secret, it's in every public profile response)
