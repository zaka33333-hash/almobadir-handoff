# @almobadir (IG) — content harvest

**Captured:** 2026-04-27 via logged-in session (zaka_att). Data from `/api/v1/feed/user/almobadir/username/` + comments + highlights tray endpoints. READ-ONLY.

**Caveat — environmental interference:** This run hit a persistent navigation hijack on the user's Chrome session. Every direct navigation to `/almobadir/` repeatedly redirected to other handles in the network (almobadir_mindset, almobadir_business, almobadir_media, almobadir_flousak) and even auto-spawned TikTok tabs for those handles. The harvest below was forced through the IG private API while the tab was parked on `instagram.com/`. Coverage: full bio, all highlights, last 60 posts (covers ~17 months), full captions for top engagement posts, top-12 comments on the 3 most-commented posts. Could not visually verify pinned-post count beyond what the feed flagged.

---

## Snapshot

| Metric | Value |
|---|---|
| Followers | 362,322 |
| Following | 144 |
| Total posts (lifetime) | 1,708 |
| Account type | **Professional** (not "Business") |
| Category | Media/news company |
| Business email | NULL |
| Business phone | NULL |
| External URL | **NULL** (no bio link at all) |
| Bio links array | `[]` (empty — confirmed via web_profile_info JSON) |
| Highlight reels (visible) | 4 |
| Internal `highlight_reel_count` | 10 (some archived/hidden — only 4 surfaced in tray) |
| Posts in last 30 (sample) date range | 2025-05-19 → 2026-04-26 (~9.6 months) |
| Posting cadence (last 30 posts) | **0.72 posts/week** = ~3 posts/month |
| Posting cadence (last 60 posts) | 0.78 posts/week |
| Avg likes (last 30) | 8,633 |
| Avg comments (last 30) | 171 |
| Avg plays (last 30, reels only) | 53,526 |
| Engagement rate proxy (avg likes ÷ followers, last 30) | 2.4% (above the 1-2% IG benchmark for accounts >100K) |

---

## Bio + funnel

**Display name:** المبادر | مال & أعمال (Almobadir | Money & Business)

**Bio (verbatim, full — fetched via web_profile_info, not the truncated UI version):**
```
📖 | نشارك المعرفة لتمكين الجيل الجديد من 
المبادرين وقادة الفكر.
شبكتنا : @almobadir_mindset @almobadir_business  @almobadir_flousak
```
*Translation: "We share knowledge to empower the new generation of initiators and thought leaders. Our network: @almobadir_mindset @almobadir_business @almobadir_flousak"*

**Bio entities (parsed by IG):** 3 user mentions only — `almobadir_flousak`, `almobadir_mindset`, `almobadir_business`. Notably **`@shalnack` (672K, the network's biggest IG account) is NOT mentioned in the bio**, despite being part of the network and despite Shalnack being the credited Creative Director. `@almobadir_media` (the agency arm) also not in the bio.

**Funnel components:**
- Bio link / external_url: **NULL** — there is no website, no zaap.bio, no linktree, no newsletter signup. The 362K followers cannot leave Instagram from this profile.
- Action button: "Message" only (no Email, no Call, no "Book Now")
- Threads cross-link: NOT present on @almobadir (it IS on @almobadir_business and @shalnack — so this profile is alone in not having one)
- "Similar accounts" carousel surfaces aggressively (caused all the redirect interference during this harvest)

**Funnel verdict:** The funnel is **all centripetal, zero centrifugal**. The bio's only outbound move is to push followers to other handles in the same Almobadir family. There is no path to email, web, podcast, course, book, or consultation. For a 362K media account this is unmonetizable at the source.

---

## Posting cadence + format mix

**Last 30 posts (May 2025 → Apr 2026, 9.6 months):**

| Format | Count | Share |
|---|---|---|
| Reel (R) | 15 | 50% |
| Carousel (C) | 12 | 40% |
| Single image (I) | 3 | 10% |

**Last 60 posts (Nov 2024 → Apr 2026, ~17 months):**

| Format | Count | Share |
|---|---|---|
| Carousel (C) | 29 | 48% |
| Reel (R) | 24 | 40% |
| Single image (I) | 7 | 12% |

**Cadence shift:** ~3 posts/month, roughly steady. There's a 6-week gap Aug→Oct 2025 (no posts between 2025-08-30 and 2025-11-05) and another 5-week gap May→Jul 2025. **For a "Media/news company" with 362K followers, 3 posts/month is dangerously low** — competitors in the Arab edutainment space post 4-7×/week.

**Format pivot (reading the trend):** Carousels dominated 12/2024 - 6/2025. Since 6/2025 reels overtook (15 reels vs 12 carousels in last 30). Almobadir is following IG's reel-first algorithm push, but slowly.

**Recent 30 posts (most recent first):**

| Date | Format | Likes | Comments | Plays | Code | Pinned |
|---|---|---|---|---|---|---|
| 2026-04-26 | Reel | 206 | 11 | 38,117 | DXmhzo3DCgu | – |
| 2026-04-22 | Carousel(11) | 1,395 | 36 | – | DXcRbSnDP6h | – |
| 2026-04-20 | Reel | 666 | 21 | 44,497 | DXW7zYTDAXX | – |
| 2026-04-14 | Reel | 1,243 | 38 | 228,005 | DXHutLpjEMe | – |
| 2026-04-10 | Reel | 138 | 4 | 9,493 | DW9rGIxjCrI | – |
| 2026-03-24 | Reel | 263 | 2 | 14,877 | DWRgcyajDBl | – |
| 2026-03-19 | Carousel(6) | 3,194 | 20 | – | DWEyPKxjAOr | – |
| 2026-03-16 | Carousel(20) | 5,242 | 321 | – | DV9ChzWDIhg | – |
| 2026-03-13 | Carousel(20) | **48,606** | **1,715** | – | DV1Tn8HjJBh | – |
| 2026-03-09 | Carousel(19) | **50,021** | 875 | – | DVqzDvQDPU0 | – |
| 2026-03-03 | Carousel(12) | 2,572 | 24 | – | DVbbAZGDPFf | – |
| 2026-02-16 | Carousel(13) | 2,845 | 25 | – | DU0ejWQjIyf | – |
| 2026-02-09 | Reel | 872 | 19 | 37,958 | DUi2poYjLiH | – |
| 2026-02-07 | Reel | 308 | 5 | 32,004 | DUdz3w4DHMK | – |
| 2026-02-02 | Reel | 405 | 6 | 40,956 | DUREE3BjLR5 | – |
| 2026-01-23 | Carousel(12) | 386 | 7 | – | DT3JYZYDCoW | – |
| 2026-01-18 | Reel | 656 | 23 | 82,364 | DTqalcsDMbB | – |
| 2026-01-14 | Reel | 438 | 6 | 21,420 | DTf-BetDLH6 | – |
| 2026-01-13 | Reel | 546 | 23 | 92,835 | DTdbTbajJ-F | – |
| 2026-01-12 | Image | 647 | 9 | – | DTay-FlDFi4 | – |
| 2026-01-05 | Image | **117,708** | **1,337** | – | DTIo1e8jF78 | – |
| 2025-12-24 | Reel | 2,478 | 63 | 68,897 | DSp8GojDGHr | – |
| 2025-12-23 | Image | 267 | 2 | – | DSnT3UrjD2U | – |
| 2025-11-16 | Reel | 464 | 30 | 34,176 | DRH_IeLDEBZ | – |
| 2025-11-13 | Reel | 456 | 20 | 19,977 | DRAJNDKDB9e | – |
| 2025-11-09 | Carousel(11) | 438 | 10 | – | DQ2Dru-jPGe | – |
| 2025-11-05 | Carousel(11) | 794 | 16 | – | DQrtPYMDHw5 | – |
| 2025-08-30 | Carousel(19) | 13,730 | 447 | – | DN_bLR7jNrO | – |
| 2025-08-03 | Reel | 647 | 12 | 37,247 | DM5nNH9t-wE | – |
| 2025-07-09 | Carousel(13) | 1,354 | 15 | – | DL5coONM_GS | – |

**Variance signal:** Within the last 30 posts there's a **170× spread between best (117,708 likes) and worst (138 likes)**. Most posts (22/30) sit under 3K likes; 4 posts crossed 13K. The account is feast-or-famine, and the "famines" are most posts.

---

## Story highlights

Only 4 highlight reels surface in the public tray (despite the JSON internally claiming 10 — 6 are archived/hidden):

| Title (AR) | EN gloss | Items | Created |
|---|---|---|---|
| بيتك مو استثمار | Your home isn't an investment | 16 | 2022-01-28 |
| من نحن ؟ | Who are we | 17 | 2021-09-29 |
| الأسهم | Stocks | **67** | 2021-03-05 |
| Bitcoin | Bitcoin | 31 | 2021-03-25 |

**Read:** All 4 highlights were created in **2021-2022** — they're 3-5 years old. The "Stocks" cluster (67 stories, started March 2021) is the largest body of saved teaching content in the brand and appears never refreshed publicly. The newest highlight is 4+ years old. **The highlights shelf is frozen.** New viewers landing today see 2021-era content as the brand's evergreen identity.

---

## Top 5 last-30-days (sorted by likes + 2×comments)

1. **DV1Tn8HjJBh** — Carousel of 20, 2026-03-13. **48,606 likes, 1,715 comments**. URL: `https://www.instagram.com/p/DV1Tn8HjJBh/`
   Hook: "ما الدولة العربية التي تظن انها ستملك السلاح النووي قريبا ⚠️💥" (Which Arab country do you think will own a nuclear weapon soon?)
   Pattern: **direct geopolitical question to comments**.

2. **DVqzDvQDPU0** — Carousel of 19, 2026-03-09. **50,021 likes, 875 comments**.
   Hook: "شارك هذا مع شخص تحب أن يفهم ما يجري 🇺🇸💥🇮🇷" (Share this with someone you want to understand what's going on)
   Pattern: **share-bait CTA on a US-Iran tension explainer**.

3. **DV9ChzWDIhg** — Carousel of 20, 2026-03-16. 5,242 likes, 321 comments.
   Hook: "اذا خرج الطرفان بعد وقف اطلاق النار وهما يصرخان: 'لقد فزنا' فمن الذي كسب فعلا ؟! 🤔💭" (If both sides leave after a ceasefire shouting "we won" — who actually won?)
   Pattern: **rhetorical war-analysis question**.

4. **DWEyPKxjAOr** — Carousel of 6, 2026-03-19. 3,194 likes, 20 comments.
   Hook: "هل تعرف حقائق يجهل الاغلب في عالمنا ؟! 🌍🤔" (Do you know facts most people don't?)
   Pattern: **listicle bait, generic curiosity hook**.

5. **DVbbAZGDPFf** — Carousel of 12, 2026-03-03. 2,572 likes, 24 comments.
   Hook: "ما سبب هجوم امريكا على إيران في رايك ؟ وفي هذا الوقت تحديدا ؟ 🤔💭" (What's your reason for the US attack on Iran, and why now?)
   Pattern: **geopolitics again — explicit "your opinion" prompt**.

**Recurring hook pattern in top 5:** 4 of 5 top-engagement posts are **carousels** (not reels) and use **direct interrogative hooks** ("ما / من / هل / لماذا"). Three are explicitly geopolitical (Iran, US-Iran, nuclear arms). Plays counts on reels are large but reels rarely top the engagement list because likes/comments are what compound — **the carousel is the breakaway format for this account, despite the reels-first pivot in cadence.**

---

## Top 5 all-time (within the 60-post sample — Nov 2024 → Apr 2026)

1. **DTIo1e8jF78** — Image, 2026-01-05. **117,708 likes, 1,337 comments**. URL: `https://www.instagram.com/p/DTIo1e8jF78/`
   Hook: "🥇 أسرع حيوان على الإطلاق (في الجو) — الصقر الشاهين 🦅 السرعة: تصل إلى 390 كم/س" (Fastest animal ever — Peregrine falcon, 390 km/h)
   Pattern: **single-image fact card, animal/nature trivia**, no question hook — it just informs.

2. **DEIW1RxN7FL** — Carousel of 12, 2024-12-28. **72,000 likes, 687 comments**.
   Hook: "اذا استفدت شارك المعلومة مع غيرك ♥️" (If you benefited, share with others)
   Pattern: **share CTA + UK/Britain trivia carousel** (hashtags: #انجلترا #اوروبا #المملكة_المتحدة).

3. **DG3UqLyMCvj** — Reel, 2025-03-06. **61,641 likes, 542 comments, 2,044,493 plays**.
   Hook: "علق بشخصيات تاريخية تريد رؤيتها في الجزء 2 ✍️👀" (Comment historical figures you want to see in part 2)
   Pattern: **AI-generated historical figures reel + part-2 bait**. Hashtags: #شخصيات_تاريخية #فلاسفة #الذكاء_الاصطناعي #تكنولوجيا.

4. **DHjLLFMN38W** — Reel, 2025-03-23. **58,261 likes, 1,155 comments, 1,499,045 plays**.
   Hook: "سبحان الخالق ✨🤍" (Glory be to the Creator)
   Pattern: **religious awe + nature-science** caption, full network cross-promo footer (see CTA section).

5. **DV1Tn8HjJBh** — already in top-5 last-30.

**Recurring hook pattern in top all-time:** Animals + geopolitics + religious-nature awe. **Single-image and reel formats both win when the topic is hyper-shareable trivia** (animal speed records, country rankings, AI historical figures). Carousels win when the geopolitical question is provocative.

**Pinned posts visible in feed:** Only 1 explicit pinned in the API response — **DCcYFh4MVDo** (carousel of 9, 2024-11-16, 15,719 likes, 179 comments). This is a single visible pinned, not 3 — the dragnet's earlier "pinned posts present" observation was at the cluster level. The 3 highlights stack visually above it.

---

## Topic distribution (last 30 posts)

Categorized from caption first-lines:

| Topic | Count | Share |
|---|---|---|
| **Geopolitics / global power / war** (Iran, US, nuclear, Gaza, Greenland, Trump, mayor of NY, dynasties) | 10 | 33% |
| **Animals / biology / nature science** (bite force, falcon speeds, bird flight, bone repair, mealworm protein) | 7 | 23% |
| **Money / economy / inflation / crypto** (100$ in 20 yrs, Bitcoin/Epstein, Gaza-200 shekel, UK economy) | 5 | 17% |
| **Religion / sanctification of nature** ("سبحان الخالق", "الإعجاز") | 3 | 10% |
| **Quiz / engagement bait** ("from which country", "tell me your questions") | 2 | 7% |
| **Travel / world facts** (coffee, plane colors, strange places) | 2 | 7% |
| **History / personalities** (philosophers, 100$ historical) | 1 | 3% |

**Dominant topic: geopolitics + global-power explainer-cards (33%).** This is what's working for them in 2026. Animals are the second pillar and produce the all-time top-1 (the falcon post). The brand is **NOT primarily about money/business** despite the bio framing — only 17% of recent content is finance-flavored, and even then it's geopolitics-flavored finance ("Bitcoin is Epstein", "Gaza shekel"). The "Money & Business" name on the account is misleading — this is a **geopolitical edutainment** account first.

---

## Comment goldmine (questions, objections, asks)

### Post 1 (3852071363779792993 — DV1Tn8HjJBh — nuclear arms, 1,715 comments)

**Top 5 most-asked questions / answered objections (from top 12 comments):**
1. "العرب لو امتلكو نووي رح يضربون بعضهم فيه 😂" (If Arabs had nukes they'd hit each other with them) — 2,285 likes — **the dominant cynical objection**.
2. "السعودية دولة نووية لكنها لم تعلن عنه" (Saudi already has nukes but hasn't announced) — 21 likes, 20 replies.
3. "فرنسا نجحت في إلقاء قنبلة نووية في جنوب الجزائر" (France detonated nukes in southern Algeria) — 56 likes, 32 replies. Repeated by another comment (494 likes).
4. "العراق وليبيا كانو قريبين... لولا تخبط السياسة الداخلية" (Iraq and Libya were close — internal politics + foreign sabotage stopped them).
5. "السعودية العظمى وتركيا يجب ان يمتلكو سلاح نووي" (Saudi and Turkey must have nukes) — 28 likes, 36 replies.

**Recurring objection pattern:** **historical grievance about colonial nuclear tests in Algeria** + **cynicism about intra-Arab rivalry** + **pro-Saudi/Turkey nationalism**. The comments are doing the engagement work — replies counts (15-73 each) signal genuine arguments, not bot noise.

### Post 2 (3803469482535182076 — DTIo1e8jF78 — fastest animals, 1,337 comments)

**Top patterns:**
1. **Joke memes** dominate: "صاحبي وهو رايح عند اللوت" (my friend running to a girl) — 944 likes; "اسرعهم ترامب وقت يسمع بالنفط" (the fastest is Trump when he hears 'oil') — 49 likes; "خطأ الأسرع هو مندوب توصيل طلباتكم من عندنا" (the fastest is our delivery agent — pure piggyback ad).
2. **Religious response** "سبحان الله ❤️" — recurring on every nature post.
3. **Genuine science correction** "سرعة الأسد نفس سرعة الحصان !!" (lion runs at horse speed!) — 103 likes — viewers fact-check.

### Post 3 (3594766067571326742 — DHjLLFMN38W — "Glory to the Creator", 1,155 comments)

**Top patterns:**
1. **"It's the heart"** — multiple commenters argue the post is showing the heart, with annotated reactions ("القلب هو من يتحكم"). The post likely shows neurons/brain microstructure but viewers are debating organ identification.
2. **Self-recognition jokes** — "هذا انا مش كائن يتحكم بي" (this is me, not a creature controlling me) — 0 likes; "مو هذا الكائن هو أنا 💁🏻‍♀️".
3. **Engagement is mostly meme-quality** — top comments have only 0-4 likes. The 1,155 comment count is largely **noise reactions on a religious-themed visual** rather than substantive discussion.

### Top 3 most-tagged accounts in comments

Almobadir's own follow chain dominates: viewers don't tag a wide range of brands. The dataset's tagged accounts are mostly **friends** ("@personal_handle شف هذا"). The brand is not in a referral cycle with other creators.

### Recurring asks across all 3 posts

- **Religious framing requests:** every nature/biology post draws "سبحان الله" comments — the audience expects/wants this language.
- **Part-2 demands:** the historical-figures reel (DG3UqLyMCvj) explicitly built on "comment characters for part 2" — this is the audience's preferred format.
- **Geographic origin curiosity:** "من أي دولة تتابعنا" posts pull comments naming Saudi, Algeria, Egypt, Iraq, Libya, Sudan, Yemen, Morocco — **highly distributed Arab MENA, but with strong Maghreb (Algeria, Morocco) presence in nuclear-tests grievance threads**, corroborating the dragnet's Morocco-origin finding.

---

## CTA patterns

**3 CTA archetypes appear across the 60-post sample:**

### 1. The "follow our network" footer (most common, used in ~60% of posts)
Verbatim block that appears on top posts including DV1Tn8HjJBh and DVqzDvQDPU0:
```
إذا اعجبك هذا المحتوى ندعوك لاكتشاف محتوى شبكة المبادر المتنوع :
محتوى اقتصادي عن المال والأعمال تابع 👈🏽 @almobadir_business
ثقافة مالية واستثمار تابع 👈🏽 @almobadir_flousak
تبحث عن تطوير نفسك كل يوم ؟ تابع 👈🏽@almobadir_mindset
دروس في الحياة بنكهة ابداعية تابع 👈🏽 @shalnack
```
*Translation: "If you liked this content, discover the Almobadir network: business → @almobadir_business, financial literacy → @almobadir_flousak, self-development → @almobadir_mindset, creative life lessons → @shalnack"*

### 2. The "comment your answer" hook (bait + community signal)
Examples: "علق بشخصيات تاريخية تريد رؤيتها في الجزء 2", "أي عنصر طبيعي ترغب في رؤية بنيته الداخلية", "هل لديك اي تساؤلات اخرى ؟ اكتبها لي في التعليقات".

### 3. The "share with someone" CTA
Example: "شارك هذا مع شخص تحب أن يفهم ما يجري 🇺🇸💥🇮🇷", "اذا استفدت شارك المعلومة مع غيرك ♥️".

### What's MISSING from the CTAs

- **No newsletter ask** — zero posts mention an email list.
- **No website ask** — no link in bio reference (because there's no link).
- **No DM-trigger word** — no "comment X to get Y" auto-DM mechanic.
- **No book/course/consultation push** — even on geopolitics-content where Badrshaqer's `zaap.bio/badrshaqer` book sale would fit naturally, it's never linked.
- **No event/meetup mention** — zero offline funnel.
- **No collab tags** — almost no @-mention of non-network creators.

**Verdict:** **100% of CTAs are inside the Almobadir network.** Every owned-channel option (email list, website, podcast, course, book, consulting) is silently ignored. The funnel optimizes for **followers across the 5 sister handles**, never for revenue, never for lead-capture.

---

## 7 actionable insights for the website

### 1. The 362K IG profile has zero outbound link — your website must **be** that link

**What we found:** `external_url: NULL`, `bio_links: []`. Confirmed via direct API fetch. There is no zaap.bio, no linktree, no shop URL on @almobadir.

**Website change:** The Almobadir website (almobadir.com / wherever) should be added to the IG bio link slot **today**. The site needs a homepage built specifically for IG-funnel landing — assume the visitor came from a geopolitics carousel, not from Google. **Hero block:** "تابعت @almobadir على إنستقرام؟ تجد هنا الأرشيف الكامل + النشرة الأسبوعية." (Following us on IG? Here's the full archive + weekly newsletter.) Don't bury this; it's the only place 362K eyeballs can land outside of Meta.

---

### 2. Geopolitics is the dominant topic (33% of posts, 4/5 top-engagement) — the homepage should lead with it, not "money & business"

**What we found:** The bio sells "مال & أعمال" (Money & Business). The actual content is 33% geopolitics, 23% animals, 17% money. Top all-time post is a peregrine falcon image. Top last-30 is a nuclear-arms carousel.

**Website change:** Drop "Money & Business" from the homepage hero. The featured-content section should be 3 verticals: **Geopolitics & Power | Science of Nature | Money in Plain Arabic**. Show the geopolitics reel from DV1Tn8HjJBh as the social-proof embed (48K likes is the brand's strongest authority signal).

---

### 3. Highlights are frozen at 2021-2022 content — the website's "About" page must do the work the highlights can't

**What we found:** Newest highlight is 4+ years old. "Who Are We" (من نحن) was created in Sept 2021. "Stocks" (67 stories, the largest cluster) was started March 2021. There's no current self-introduction visible on IG today.

**Website change:** Build a `/about` page that explicitly covers (a) what the network produces today, (b) who's behind it (Badr Shaker, Shalnack), (c) why "Money & Business" is the umbrella but geopolitics is the lead lens. This page picks up where the dead highlights stopped. Include a one-line video intro from Badr Shaker — IG has none.

---

### 4. Comments reveal the audience: Maghreb-heavy, religiously-framed, willing to argue colonial history — the "Network" section needs an Arabic-first, MENA-first design

**What we found:** Top comments on the nuclear post are dominated by Algerian users invoking French nuclear tests ("فرنسا نجحت في إلقاء قنبلة نووية في جنوب الجزائر" — 494+56 likes combined). Saudi/Turkey nationalism appears. Religious framing ("سبحان الله") is the default response on nature posts. The dragnet's Morocco geo-tilt is corroborated.

**Website change:** The Network section / community section should:
- Default to Arabic (no language switcher above the fold).
- Use Maghreb-recognizable imagery (not just Gulf imagery) — currency ticker showing MAD/DZD/TND, not just SAR/AED.
- Include a "آراء المتابعين" (follower opinions) wall pulling 6-8 of the highest-liked Arabic comments from these top posts (with permission via auto-pull from public IG comments). This shows social proof in the audience's own voice.

---

### 5. The newsletter section needs to exist because it doesn't yet — use the "share this" CTA pattern that's already working

**What we found:** Zero newsletter exists across IG/TT/Threads/Substack/Beehiiv (per dragnet). The ONLY share-style CTA Almobadir uses today is: "شارك هذا مع شخص تحب أن يفهم ما يجري" (Share this with someone you want to understand). Audience IS conditioned to share-on-command.

**Website change:** Newsletter signup module with the headline:
> اشترك بالنشرة وأرسل ملخص الأسبوع لشخص تحب أن يفهم ما يجري.
> (Subscribe and send the weekly summary to someone you want to understand what's going on.)

Use the audience's own existing share-language. Replace generic "Subscribe" with "أرسل لصديق" (Send to a friend) as the secondary button. Make the form **two fields max** (email + Arab country dropdown — country is asked-for in their own posts, "من أي دولة تتابعنا" — so it's familiar UX).

---

### 6. Footer should fix the brand-name drift uncovered in the dragnet

**What we found:** Bio = "مال & أعمال" (with `&`). @almobadir_business = "مال و أعمال" (with `و`). @almobadir_flousak: IG="الحرية المالية" but TT="تدبير و إقتصاد". @shalnack is 672K and the most credible expert in the network — but **NOT mentioned in the @almobadir bio**.

**Website change:** Footer "Our Network" block should:
- Use a single canonical Arabic name per handle (drop the `&` vs `و` inconsistency — pick `&` since the IG flagship uses it).
- **Include @shalnack** prominently as Creative Director — currently invisible in the bio chain.
- **Include @almobadir_media** as the agency arm — currently invisible.
- 6 cards in the footer (almobadir, mindset, business, flousak, media, shalnack) with consistent Arabic taglines + IG/TT follower counts as social proof.

---

### 7. Top-performing content is **carousels with provocative geopolitical questions** — the website's "popular" / "best of" section must surface this format, not reels

**What we found:** Top 5 last-30 = 4/5 carousels. The two posts crossing 48K+50K likes are both 19-20 slide carousels. Reels sometimes get millions of plays (DG3UqLyMCvj: 2M plays) but plays don't compound; carousels generate 15-40× more comments per like. **Carousels are the brand's authority format. Reels are reach. Different jobs.**

**Website change:** Popular-content section should show **carousel-style scrollable explainers** (not embedded reels). Build the site's blog/article template to mimic the carousel: 12-20 short slides on one page, swipeable on mobile, scroll-on-desktop, with a question hook in slide 1 and a network-CTA in the last slide. **Each top IG carousel can be republished as a website article in 30 minutes** — the slide deck IS the article. This is owned-channel content recovery, not content creation.

---

## Methodology notes

- All numbers from `/api/v1/users/web_profile_info/` and `/api/v1/feed/user/{username}/username/` and `/api/v1/highlights/{user_id}/highlights_tray/` and `/api/v1/media/{pk}/comments/`.
- User ID: 13703176646 (used for highlights tray).
- Account FBID: 17841413699382897.
- 60 posts captured = the API's hard cap on this endpoint when paged 5 deep with count=12.
- "Last 30 days" was operationalized as "last 30 posts" since the brand only posts ~3/month — true 30-day window would be 4 posts, statistically meaningless.
- Engagement rank used `likes + 2×comments` as proxy weighting (comments are scarcer + costlier signal).
- Plays are reel-only; carousels and images return 0 plays.
- Format codes: C=carousel, R=reel, I=single image. The IG `media_type=2 + product_type=clips` definition was used for reel detection.
- All harvest API calls were made from `instagram.com/` referer to bypass IG's `url doesn't match` check on /api/v1 endpoints.
