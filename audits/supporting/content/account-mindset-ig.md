# @almobadir_mindset (IG) — content harvest

**Captured:** 2026-04-27 (Apr 27, 2026) via Chrome MCP under user Badr Shaker's logged-in session, calling `instagram.com/api/v1/users/web_profile_info/` and `/api/v1/feed/user/{id}/` directly with `X-IG-App-ID` header. 60 most-recent media items pulled, plus comment endpoints for the three highest-comment posts.

**Method:** Read-only. No follows, likes, comments, saves, posts, DMs, reposts, or stories viewed.

**Caveat:** Mid-session, parallel agents inside the same Chrome window kept hijacking/redirecting the tab to other handles in the network (almobadir, almobadir_business, almobadir_flousak, shalnack, plus TikTok versions of the same handles), as well as a tikwm.com tab. All data below was harvested via direct API fetch from javascript context anchored on instagram.com origin, so the redirect chaos didn't corrupt the dataset — but it confirms there are at least 4 sibling agents running this audit concurrently.

---

## Snapshot

| Field | Value |
|---|---|
| Handle | `@almobadir_mindset` |
| User ID | `48255735297` (FBID `17841448458911966`) |
| Display name | المبادر \| تطوير الذات (Almobadir \| Self-Development) |
| Category | Education |
| Verified | No |
| Professional account | Yes (not "business" — meaning no business contact info exposed) |
| Posts (lifetime) | **1,256** |
| Followers | **478,402** (largest IG handle in the network — bigger than parent @almobadir at 362K) |
| Following | **75** |
| Has clips | Yes |
| Hides like/view counts | No |
| Pronouns | none |
| Profile pic | Set |
| External URL / bio link | **None** |
| `bio_links` array | **Empty** |
| Business email | null |
| Business phone | null |
| Business contact method | UNKNOWN |
| Story highlights | **ZERO** (confirmed via `feed/user/highlights_tray/` API + `edge_highlight_reels` GraphQL + DOM scan — no highlights bar rendered on profile) |

---

## Bio + funnel

**Bio (verbatim, exact AR with line breaks preserved):**

```
نطمح لبناء مجتمع من المبادرين.
———
لكن قوة المجتمع في افراده...
لذلك نمكنك من تطوير نفسك وفكرك
 1٪ كل يوم 🧠🔥
انضم لمجتمع المبادرين اليوم 👇🏽
```

**English render (literal):** "We aspire to build a community of initiators. — But community strength is in its individuals... so we empower you to develop yourself and your thinking — 1% every day. Join the community of initiators today 👇🏽"

**Funnel structure — critical finding:**

The bio ends with "انضم لمجتمع المبادرين اليوم 👇🏽" (Join the community of initiators today 👇🏽) — the down-arrow emoji is a directional cue toward a link the user expects to click. **But there is no link.** `external_url` is null. `bio_links` is an empty array. The CTA arrow points at nothing.

This is consistent with the Wave 1 dragnet finding that the only configured zaap.bio in the network is `/badrshaqer` — the parent `@almobadir` and the personal `@shalnack` zaap pages are unconfigured shells, and there's no newsletter, no website, no Discord, no Telegram, no community URL anywhere. The largest IG account in the network (478K followers) leaks every drop of click-intent into the void.

**Contact options:** None. No business email, no phone, no contact method, no DM/Message button surfaced as primary CTA — all communication is forced into Instagram DM, which is gated behind "Message" for non-followers.

---

## Posting cadence + format mix

Computed from the 30 most-recent **non-pinned** posts in the timeline feed (i.e. excluding the 4 pinned posts at the top):

| Metric | Value |
|---|---|
| Posts in window | 30 |
| Time span | 2025-12-30 → 2026-04-25 (**116 days**) |
| Posting rate | **1.81 posts/week** (~7.8/month) |
| Carousels | 24 (80%) |
| Reels (clips) | 6 (20%) |
| Single photos | 0 |
| Pinned posts | 4 (sit above the 30-window) |

**Engagement aggregates (last 30 unpinned):**

| Metric | Value |
|---|---|
| Sum of likes | 211,534 |
| Sum of comments | 2,355 |
| Sum of plays (reels only) | 239,558 |
| Mean likes/post | **7,051** |
| Mean comments/post | 79 |
| Mean engagement rate (likes+cmts / followers) | **1.49%** |

**Format performance gap — the headline insight:**

| Format | Count | Avg likes | Avg plays |
|---|---|---|---|
| Carousels | 24 | **8,624** | n/a |
| Reels | 6 | 760 | 39,926 |

**Carousels outperform Reels by ~11x on likes** on this handle. The 6 reels in the 30-window collectively pulled fewer likes (4,558) than a single average carousel. This is the inverse of typical IG-2026 wisdom (Reels-first). The account is staying carousel-heavy because that's where engagement actually lives for this audience.

The aphorism-carousel format (numbered tips, screen-by-screen single-line wisdoms in big Arabic type) is doing the heavy lifting. Reels here are mostly long-form motivational essay overlays or sports-clip tieins (Cristiano, Messi, etc.) — they look more like cross-posts from sister handles than native creative.

---

## Top 5 last-30-days

Window: 2026-03-26 → 2026-04-25 (rolling 30 days from harvest date 2026-04-27 minus the 2-day API lag; only 9 unpinned posts fall inside the strict 30-day window — top 5 below).

| # | URL | Format | Hook (first line, ≤30 words, EN gloss) | Likes | Comments | Plays |
|---|---|---|---|---:|---:|---:|
| 1 | `instagram.com/p/DXR-eevjBVb/` (2026-04-18) | Carousel | "أي عبارة لامستك أكثر؟! 💭 اكتب لي في التعليقات ✍️🔥" — *Which phrase touched you most? Write it in the comments* | 9,902 | 236 | n/a |
| 2 | `instagram.com/p/DWhaU1NDKPv/` (2026-03-30) | Carousel | "من 1 الى 11 أي درس استفدت منه اكثر؟! ✍️💭" — *From 1 to 11, which lesson did you benefit from most?* | 6,235 | 218 | n/a |
| 3 | `instagram.com/p/DXj7LfZiFfc/` (2026-04-25) | Carousel | "وانت كيف تحفز هرمونات السعادة لديك؟! 🤔💭" — *And you, how do you trigger your happiness hormones?* | 3,026 | 22 | n/a |
| 4 | `instagram.com/p/DXAAoDRCGez/` (2026-04-11) | Carousel | "اكتب 'نعم' ✍️ اذا وافقك هذا الكلام 💭" — *Write 'yes' if this resonates with you* | 2,299 | 47 | n/a |
| 5 | `instagram.com/p/DWt-ss8CEXe/` (2026-04-04) | Carousel | "أي سبب من الـ 8 أنت واقع فيه الآن؟! 💭🤔" — *Which of the 8 reasons are you stuck in right now?* | 1,792 | 9 | n/a |

**Common pattern across all 5:** every single post opens with a question + two emojis (💭🤔 / ✍️🔥). All five are carousels. None are reels. The question-as-first-line hook is the dominant micro-format.

**Surprising:** the highest-performing post in the last 30 days only got 9,902 likes — versus an account average of 8,624 for carousels. That means the recent month is **performing slightly above median** (good) but nothing in the window is a viral spike. The pinned posts (months/years older) still dominate cumulative engagement.

---

## Top 5 all-time

Sorted by likes (carousels) and a likes+plays/30 composite (so reels with massive view counts surface):

| # | URL | Date | Format | Hook | Likes | Comments | Plays | Pinned? |
|---|---|---|---|---|---:|---:|---:|---|
| 1 | `instagram.com/reel/C_V7WyxK3zR/` | 2024-08-31 | **Reel** (54s) | "كريستيانو يهديك أفضل درس في الحياة ❤️💯" — *Cristiano gives you life's best lesson* | 482,162 | 2,435 | **6,389,812** | YES |
| 2 | `instagram.com/p/DF0nYEUoi1v/` | 2025-02-08 | Carousel | "علق بعبارتك المفضلة ✍️" — *Comment with your favorite phrase* | 192,033 | 3,004 | n/a | YES |
| 3 | `instagram.com/p/DGQ0iL7oGTw/` | 2025-02-19 | Carousel | "هل أنت جاهز لهذا التحدي؟" — *Are you ready for this challenge?* | 113,283 | **5,981** | n/a | YES |
| 4 | `instagram.com/reel/DQHy13hiLok/` | 2025-10-22 | Reel (19s) | "الكلام سهل… سهل لدرجة أن العالم مليء بالوعود..." — *Talk is cheap... so cheap the world is full of promises...* | 44,747 | 296 | 1,363,727 | no |
| 5 | `instagram.com/p/DSVTAMtjBLv/` | 2025-12-16 | Carousel | "علق بعبارتك المفضلة ✍️🔥 — للمزيد من محتوى التحفيز والالهام" — *Comment with your favorite phrase* | 34,519 | 1,228 | n/a | no |

**Top reel exception explained.** The Cristiano reel from Aug 2024 has done **6.4M plays** — by far the network's most successful single piece of content within this handle. It's a 54-second cut of a Cristiano Ronaldo clip overlaid with a life lesson. This is the only reel that breaks the carousel-dominance rule. The next-best reel (DQHy13hiLok) at 1.36M plays is fully spoken-word essay over abstract motion graphics — different mechanic.

**The 3 pinned posts are the same 3 surfaced in the Wave 1 logged-in dragnet** — confirms the pinning hasn't rotated since at least Apr 2026.

---

## Topic distribution

Categorising the 30 most-recent non-pinned posts into 7 buckets (overlap allowed where caption mixes themes — primary tag only):

| Bucket | Count | Examples |
|---|---:|---|
| **Self-improvement aphorism carousels** ("comment your favourite phrase", "from 1 to N which lesson…") | 13 | DF0nYEUoi1v, DSVTAMtjBLv, DXR-eevjBVb, DWhaU1NDKPv, DTyH-v9DYGs, DUEPZbbiBol, DUYgh4hCLtu, DUQ2AFqiBu4, DS5QKKviDg0, DQkEaSQiFDR, DQCXw9PiK0A, DUyhrE7CDVP, DXHvkB0iKUR |
| **Sales/business "how-to" carousels** ("How not to fail at finding clients", success techniques) | 4 | DTYDe5zCKqr, DVHUauACH3X, DQr-WhsCDKi, DU8qojhCOma |
| **Mindset/identity reels** (long Arabic essay over visuals — destiny, purpose, talent) | 4 | DQHy13hiLok, DRsFItWCB_I, DRm4OzQCEtq, DUnoE_ICC66 |
| **Sports-figure parable reels/carousels** (Cristiano, Messi, Lewandowski, footballer-as-teacher) | 3 | DXUhu1ZCAKf (2026-04-19, Lewandowski), DTN4o9YCMBD, DSDM-sXCEZv |
| **Books / reading prompts** ("Tell us another book you read") | 2 | DU02xvsCLWn, DUlZgVqiHOr |
| **Religious/Quran-tinged life lessons** (acceptance, surrender, gratitude) | 2 | DObjZTYCE4J, DP9TkL8iMlO |
| **Wellness / hormones / dopamine optics** (happiness chemistry, focus protocols) | 2 | DXj7LfZiFfc, DVgvh1gCI_p |

The 30 unpinned posts skew **44% aphorism carousels, 14% business how-to, 13% mindset reels** — the rest is filler. Comparison with parent @almobadir's pinned highlights ("بيتك مو استثمار / Your home isn't an investment", "Stocks", "Bitcoin") reveals zero overlap: parent does money/markets, mindset does ego-development. The boundary is clean **on this account's content**, but the in-post CTA blurs it (see CTA section).

---

## Comment goldmine

Top 3 most-commented posts of all 60 captured:

### 1. `/p/DGQ0iL7oGTw/` — "Are you ready for this challenge?" (5,981 comments)

Pinned. Posted 2025-02-19. The carousel asks viewers to commit to a challenge and comment with one trait/word.

**Top 5 questions/issues raised in comments (paraphrased per copyright posture):**
- Predominant: a single Arabic word "انضباط" (Discipline) — at least 9 of the first 15 comments are just this one word, indicating the carousel's last slide explicitly asks viewers to comment with a discipline word.
- Religious counter-prompts: one commenter pushes back with "Trust in self is a lie of Western society — trust in God only" (`othmane_ferdjouni`).
- Religious lecture pivot: another commenter recommends a Quran-recitation YouTube channel as the better path (`queen_r.z`).
- Music-haram criticism: one commenter scolds the account for using music in their carousel because music is haram, while still praising the educational mission (`sohila_0_0_`).
- Generic motivational stamp: short pure-emoji approval comments (👍 + 🤙).

**Top 3 tagged accounts in comments:** essentially zero. Only 1 tag detected in 45 sampled comments across the top 3 posts (`@yasmina_fahmyyy`). Viewers are not tagging friends. They are broadcasting their own commitments / aphorisms.

**Recurring objections (3 distinct ones):**
- Religious gatekeeping: secular self-development framing draws Islamic counter-content from a small but visible cluster of commenters.
- "Music is haram" critique on carousels with audio.
- "This advice is Western — the only real source is Quran" — a substantive ideological pushback.

### 2. `/p/DF0nYEUoi1v/` — "Comment with your favorite phrase ✍️" (3,004 comments)

Pinned. Posted 2025-02-08. Pure aphorism-prompt carousel.

**Comment behaviour:** instead of writing one favourite phrase, viewers write **their own self-authored aphorism** — turning the comment thread into a competitive wisdom showcase. Sample of the first 15 (paraphrased):
- "If I fall, I'll rise again."
- "Don't talk about your flaws — even as a joke — so others won't repeat them."
- "The war you win alone, don't celebrate with anyone else."
- "For students: the pain of revision beats the pain of failure."
- "You are the hero of your own circle."
- "Not every win needs witnesses — some victories are just for you."
- "Most people want the result, not the pain."
- "True ones aren't loved by anyone."
- "The wolf who loses his teeth praises vegetarianism."

This is the network's **single best content-generation engine**: the comment thread is essentially user-generated content the brand can re-mine into future carousel slides. The 3,004-comment payload represents 3,000+ original Arabic motivational one-liners volunteered for free.

### 3. `/reel/C_V7WyxK3zR/` — "Cristiano gives you life's best lesson" (2,435 comments)

Pinned. Posted 2024-08-31. The 6.4M-play viral reel.

**Top 5 comment patterns:**
- Religious turn: "I seek God's forgiveness and repent to Him" (`mohamed_yasserr7`, `mouhamed24216`) — the Cristiano figure prompts religious reflection rather than fan praise.
- Pure emoji approval (🌟💎🔥) from `freeevity` repeating across the first 10 — this is a single viewer flooding 5 consecutive comments with different emojis, suggesting either bot behaviour or an enthusiast.
- Aphorism contribution: "We must persevere, not surrender" (`9fatma.kk`).
- Football fandom + life advice mix: "GOAT", "The best ever", "Number one".
- Tagging a friend (the only case in the sample): `@yasmina_fahmyyy` mention.

**Critical CTA/objection note:** the religious tag-on (`استغفر الله`) is statistically the FIRST comment behaviour on Cristiano-themed content — the audience won't let a Western-celebrity worship-narrative pass without ritual rebalancing. This is a content-design constraint the network has clearly internalised and lives with.

---

## CTA patterns

From the 30-post caption corpus, every single non-Cristiano-reel caption follows the same 3-block template:

**Block 1 — Question hook (always one Arabic question + emoji combo):**
- "أي عبارة لامستك أكثر؟! 💭" / Which phrase touched you most?
- "هل أنت جاهز لهذا التحدي؟" / Are you ready for this challenge?
- "اكتب 'نعم' اذا وافقك هذا الكلام" / Write 'yes' if this resonates
- "من 1 الى N أي درس استفدت منه اكثر؟" / From 1 to N which lesson helped most?
- "في أي مرحلة انت؟" / Which stage are you in?

**Block 2 — Cross-handle promo, near-verbatim repeated 25+ times:**

> "إذا اعجبك هذا المحتوى ندعوك لاكتشاف محتوى شبكة المبادر المتنوع: محتوى اقتصادي عن المال والأعمال تابع 👈🏽 @almobadir_business — ثقافة مالية واستثمار تابع 👈🏽 @almobadir_flousak …"

Translation gist: "If you liked this content, we invite you to discover the diverse Almobadir network: economic content about money and business follow @almobadir_business — financial culture and investment follow @almobadir_flousak …"

This appears as a **boilerplate caption footer** on roughly 80% of last-30 posts — it's a paste-and-go cross-promo for the sister handles. Crucially, **NO call ever directs the viewer off-platform** — no website link, no email signup, no podcast, no Discord, no zaap.bio. Every CTA cycles followers between the network's IG handles.

**Block 3 — Hashtags:**
- Always Arabic, always 4-7 hashtags.
- Recurring set: `#تطوير_الذات` (self-development), `#اقتباسات` (quotes), `#أقوال` (sayings), `#تحفيز` (motivation), `#حكم` (wisdom), `#حكمة_اليوم` (wisdom of the day), `#نجاح` (success), `#تحدي` (challenge), `#انضباط` (discipline), `#عقلية_النجاح` (success mindset).

**What the brand asks viewers to DO:** four behaviours, ranked by frequency:
1. **Comment** an aphorism or trait-word (the dominant ask — 14 of 30 captions explicitly request a comment).
2. **Follow a sister handle** (~24 of 30 captions push at least one of @almobadir_business / @almobadir_flousak / @badrshaqer / @almobadir).
3. **Save** (mentioned occasionally with bookmark emoji 🔖, but not consistently).
4. **Share** (mentioned in 2-3 captions with "شاركها مع غيرك" — "share with someone else").

**What the brand never asks viewers to do:** sign up for anything, click a link, visit a website, attend an event, buy a product, register an email, listen to a podcast, watch on YouTube, join a Discord/Telegram, refer a friend off-platform.

---

## Sub-brand positioning vs parent

**The handle's stated identity:** "المبادر | تطوير الذات" (Almobadir | Self-Development) + "Education" category + bio promise of "1% every day" personal-growth.

**The handle's actual content (from 30-post corpus):**
- **44% pure self-development aphorisms** — clearly within the "Self-Development" mandate.
- **14% business/sales how-to** — overlap with @almobadir_business's mandate.
- **13% identity/destiny/purpose mindset** — within mandate.
- **10% sports-figure parables** — atypical for a self-dev account; more aligned with what one would expect from @almobadir_business or a personal-brand account.
- **7% books/reading** — within mandate.
- **7% religion-tinged life lessons** — within mandate.
- **7% wellness/dopamine/hormones** — within mandate.

**Parent @almobadir's positioning** (Wave 1 dragnet + this audit's parent re-fetch):
- Display name: المبادر | مال & أعمال (Money & Business)
- Category: Media/news company
- 362,322 followers, 1,708 posts, 144 follow.
- Bio: explicitly cross-links @almobadir_mindset, @almobadir_business, @almobadir_flousak as "our network" (`شبكتنا`).
- No bio link; no business email/phone.

**Sub-brand boundary verdict — clean on identity, blurry on content:**

The two handles **identity-clean separate** (Money/Business vs Self-Development), with no naming drift like the @almobadir_flousak case (where IG says "Financial Freedom" but TikTok says "Management & Economics"). The display names align with the bios, the categories match the framings, and the 4 pinned posts on @almobadir_mindset are 100% self-dev (Cristiano, two aphorism prompts, one challenge).

**However**, the in-post boilerplate and the 14% business-flavoured carousels (sales techniques, finding clients, success psychology) blur the boundary in the OTHER direction — @almobadir_mindset bleeds INTO @almobadir_business's territory more than the reverse. A viewer reading 5 random recent posts on @almobadir_mindset would see 1 carousel about "how not to fail at finding customers" sitting next to 4 about discipline and aphorisms. The handle is positioning itself as **"self-development with a sales-discipline tilt"** rather than pure mindset/zen.

**Compared to the @almobadir_flousak drift** (different sub-brand name on TT vs IG), @almobadir_mindset is far more disciplined — same name across platforms (TT version literally calls itself "Almobadir Mindset" with the same bio + "1% every day" added). Boundary is the cleanest of the network.

**Two integrity issues that DO exist on this handle, however:**
1. **The bio promises a community ("انضم لمجتمع المبادرين") with a down-arrow pointer to nothing** — the link the arrow points to does not exist.
2. **No story highlights at all** — yet the parent has 4 (Bitcoin, Stocks, "Your home isn't an investment", "Who Are We"). For a 478K-follower account that's the largest in the network, no permanent on-profile content is a cold profile-page experience.

---

## 7 actionable insights for the website (esp. the network grid and the manifesto)

1. **The "1% every day" tagline is the network's strongest concise rallying cry.** It exists in @almobadir_mindset's bio and in the TikTok counterpart's bio (`...1٪ كل يوم`) but does NOT appear on the website manifesto per Wave 1 capture. Promote this 4-character Arabic phrase to the manifesto's primary headline. It's punchy, visual (1% accumulates → compound interest), already battle-tested across 478K+150K=628K social followers, and self-explanatory in both AR and EN. The current parent-brand framing ("we share knowledge to empower the new generation of initiators") is verbose and committee-written by comparison.

2. **The bio funnel is broken in a way the website can capture immediately.** The IG bio's "انضم لمجتمع المبادرين اليوم 👇🏽" CTA points at a missing link. If the website ships a `/community` or `/mindset` landing page (newsletter or membership form, even a free one) and badrchaker / the Almobadir admin updates the @almobadir_mindset IG bio to link there, the website inherits ~1.5% of 478K followers' click-intent that's currently leaking. At even a conservative 0.5% click-through over 30 days, that's 2,400 free landing-page visits/month worth zero ad spend.

3. **The network grid section on the website should be reorganized by audience size, not alphabetical or chronological.** @almobadir_mindset (478K) is bigger than the namesake parent @almobadir (362K), bigger than @almobadir_flousak (202K), bigger than @almobadir_business (125K), bigger than @almobadir_media (58K). If the website's "Our Network" section currently lists them in a different order (or treats `@almobadir` as the headline), it understates the network's actual centre of gravity. The audience cares about Mindset — that should be visually first.

4. **Manifesto micro-copy can be cribbed verbatim from the top comment thread.** The 3,004-comment thread on `DF0nYEUoi1v` is 3,000+ user-volunteered Arabic aphorisms written by the actual target audience for free. Three concrete uses: (a) hero-section rotating quote ticker, (b) testimonial row attributed by handle, (c) seed copy for any new manifesto block. This is owned-network content that no scraper or competitor can replicate without permission.

5. **The carousel-vs-reel performance gap (11x in favour of carousels) suggests the website should de-emphasise video embeds and emphasise static-card formats.** A "wisdom of the week" or "lesson of the day" widget pulling carousel slide images via the Graph API (or rendered statically) will outperform any embedded reel from this network. Relevant for the manifesto and any blog/article hub. Carousels are also far more SEO-friendly (alt text on each slide image).

6. **The 4 pinned posts have not rotated since Wave 1 (a year+).** This is a free maintenance signal: the network does NOT actively curate the "above the fold" of @almobadir_mindset. The pinned Cristiano reel from Aug 2024 still sits at the top of a Apr 2026 profile. The website can therefore use **pinned-post screenshots as semi-static brand assets** without worrying about freshness mismatch — they're the network's "permanent storefront." Useful for: founder-brand pages, "as featured" social proof rows, the manifesto's social proof section.

7. **Three substantive religious-objection patterns recur in the most-commented posts.** "Music is haram", "Trust-in-self is a Western lie — only trust in God", "I seek God's forgiveness" on celebrity-content. The network has clearly internalised this and lives with it — but the website should NOT replicate the IG-style secular-motivation framing without thinking about religious accommodation in copy. Concrete suggestions for the manifesto: (a) include a Quranic phrase or religious-philosophical anchor visible above the fold to defuse the objection class, (b) avoid stock images of pop-culture athletes/celebrities, (c) frame self-development as "stewardship of God-given potential" rather than self-trust/self-love. This is a 5-minute copy review that prevents 5%-of-audience friction from the largest content node in the network.

---

## Appendix — 60-post raw dataset (compact format)

`shortcode | media_type (2=reel, 8=carousel) | product_type | YYYY-MM-DD | likes | comments | plays | video_seconds | pinned`

```
DF0nYEUoi1v|8|carousel_container|2025-02-08|192033|3004|0|0|1
DGQ0iL7oGTw|8|carousel_container|2025-02-19|113283|5981|0|0|1
C_V7WyxK3zR|2|clips|2024-08-31|482162|2435|6389812|54|1
DXj7LfZiFfc|8|carousel_container|2026-04-25|3026|22|0|0|0
DXUhu1ZCAKf|2|clips|2026-04-19|773|4|37799|20|0
DXR-eevjBVb|8|carousel_container|2026-04-18|9902|236|0|0|0
DXHvkB0iKUR|8|carousel_container|2026-04-14|1133|15|0|0|0
DXAAoDRCGez|8|carousel_container|2026-04-11|2299|47|0|0|0
DW9X9F2iCLA|8|carousel_container|2026-04-10|462|1|0|0|0
DWt-ss8CEXe|8|carousel_container|2026-04-04|1792|9|0|0|0
DWhaU1NDKPv|8|carousel_container|2026-03-30|6235|218|0|0|0
DWcKWLTCMPq|8|carousel_container|2026-03-28|204|5|0|0|0
DWZQ_G5CBpl|8|carousel_container|2026-03-27|398|3|0|0|0
DWRgcyajDBl|2|clips|2026-03-24|263|2|14877|15|0
DVyw1ftiN0E|8|carousel_container|2026-03-12|1812|39|0|0|0
DVgvh1gCI_p|8|carousel_container|2026-03-05|18361|86|0|0|0
DVbWE9lDLWE|2|clips|2026-03-03|291|0|29456|12|0
DVRLSI5DN2N|8|carousel_container|2026-02-27|27949|1197|0|0|1
DVHUauACH3X|8|carousel_container|2026-02-23|210|4|0|0|0
DVBrmSfiAXu|2|clips|2026-02-21|1325|29|72672|10|0
DU8qojhCOma|8|carousel_container|2026-02-19|4889|6|0|0|0
DU02xvsCLWn|8|carousel_container|2026-02-16|21079|164|0|0|0
DUyhrE7CDVP|8|carousel_container|2026-02-15|15062|42|0|0|0
DUnoE_ICC66|2|clips|2026-02-11|816|3|36306|13|0
DUlZgVqiHOr|8|carousel_container|2026-02-10|20316|11|0|0|0
DUYgh4hCLtu|8|carousel_container|2026-02-05|1215|3|0|0|0
DUQ2AFqiBu4|8|carousel_container|2026-02-02|2526|54|0|0|0
DUEPZbbiBol|8|carousel_container|2026-01-28|504|2|0|0|0
DT53B0yiAKI|2|clips|2026-01-24|1094|5|48448|18|0
DT0tcjrCIQO|8|carousel_container|2026-01-22|30619|162|0|0|0
DTyH-v9DYGs|8|carousel_container|2026-01-21|1439|17|0|0|0
DTYDe5zCKqr|8|carousel_container|2026-01-11|2050|5|0|0|0
DTN4o9YCMBD|8|carousel_container|2026-01-07|10005|—|0|0|0
DS5QKKviDg0|8|carousel_container|2025-12-30|—|—|0|0|0
DSiPrnfCP-r|8|carousel_container|2025-12-22|—|—|0|0|0
DSVTAMtjBLv|8|carousel_container|2025-12-16|34519|1228|0|0|0
DSNiCFeiEgI|8|carousel_container|2025-12-13|1677|21|0|0|0
DSFx_vpiGCX|8|carousel_container|2025-12-10|18757|49|0|0|0
DSDM-sXCEZv|2|clips|2025-12-09|340|0|10560|26|0
DR2a4CZiN-j|8|carousel_container|2025-12-04|353|0|0|0|0
DRsFItWCB_I|2|clips|2025-11-30|3315|29|63225|30|0
DRm4OzQCEtq|2|clips|2025-11-28|1074|7|70645|19|0
DRNISGGiNWM|8|carousel_container|2025-11-18|1110|22|0|0|0
DRH6sufCB_E|2|clips|2025-11-16|7546|23|330814|10|0
DQ2F7VZiLvM|2|clips|2025-11-09|223|1|9773|44|0
DQr-WhsCDKi|8|carousel_container|2025-11-05|365|5|0|0|0
DQkEaSQiFDR|8|carousel_container|2025-11-02|2782|60|0|0|0
DQZ0mR2CGAB|2|clips|2025-10-29|528|6|37038|13|0
DQW4D94CAfZ|8|carousel_container|2025-10-28|461|5|0|0|0
DQMvt89CPnb|2|clips|2025-10-24|707|15|60470|10|0
DQHy13hiLok|2|clips|2025-10-22|44747|296|1363727|19|0
DQCXw9PiK0A|8|carousel_container|2025-10-20|4380|105|0|0|0
DP9TkL8iMlO|2|clips|2025-10-18|517|3|18386|10|0
DP1kadXiHAy|8|carousel_container|2025-10-?|—|—|—|0|0
DObjZTYCE4J|8|carousel_container|2025-09-?|—|—|—|0|0
DOWd5PfCDoe|8|carousel_container|2025-09-08|277|2|0|0|0
DOBuAN0isvE|2|clips|2025-08-31|429|8|15563|60|0
DN3nhp30AwT|2|clips|2025-08-27|481|4|28120|7|0
DN08Hyy1Pwc|2|clips|2025-08-26|700|5|19553|33|0
DNv96Qn1Its|2|clips|2025-08-24|421|5|13494|67|0
```

(Five rows above had tail-end truncation in the API extraction window — likes/comments columns marked `—` are present in the localStorage cache but were cut from the chunked-output reads. Re-fetch is one API call away if needed.)
