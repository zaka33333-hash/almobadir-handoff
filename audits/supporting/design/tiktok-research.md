# Almobadir Network — TikTok Research

_Research date: 2026-04-26. All findings verified via Bing/Google search and TikTok URL probing. Direct profile scraping was blocked by TikTok's JS-rendered pages, so stat-level numbers (followers, total likes) could NOT be confirmed and are listed as "unverified — manual check required"._

## Verdict

The Almobadir network has a **real but partial TikTok footprint**: three confirmed handles map 1:1 to the topical sub-brands on Instagram (`@almobadir`, `@almobadir_mindset`, `@almobadir_flousak`), but the two newer business-focused IG handles (`@almobadir_business`, `@almobadir_media`) are **not present on TikTok** — they are Instagram-exclusive. The founder's personal handle `@badrshaqer` is **not a TikTok account** — his Instagram bio cross-links to Threads only, and TikTok search returns zero matches under that exact spelling. `@shalnack` is **not a sister brand at all** — it's the personal handle of Shalnack Ryusei, the Creative Director at Almobadir (per LinkedIn), and is not in scope for the brand's network. Net: TikTok is a **secondary distribution channel** for Almobadir — it carries the same three vertical pillars (mainstream finance, mindset, household economy) but the website's own footer doesn't even link to it, suggesting the team treats Instagram as the canonical home and TikTok as a feeder.

## Per-handle table

| IG handle | TikTok handle | TikTok display name | TikTok exists? | Followers | Total likes | URL | Notes |
|---|---|---|---|---|---|---|---|
| @almobadir | @almobadir | المبادر \| مال و أعمال | Confirmed (search-indexed, multiple videos) | Unverified — manual check required | Unverified | https://www.tiktok.com/@almobadir | Money & business pillar. Active since at least 2022 (oldest indexed video ID 7088715272810401030 ≈ Apr 2022). Captions reference `@badrshaqer` as the founder's IG handle. |
| @almobadir_mindset | @almobadir_mindset | المبادر \| تحفيز و تطوير الذات | Confirmed | Unverified | Unverified | https://www.tiktok.com/@almobadir_mindset | Motivation & self-development pillar. |
| @almobadir_flousak | @almobadir_flousak | المبادر \| تدبير و إقتصاد | Confirmed | Unverified | Unverified | https://www.tiktok.com/@almobadir_flousak | Personal-finance / household-economy pillar (Maghrebi spelling "flousak" = "your money"). |
| @almobadir_business | _none_ | — | NOT on TikTok | — | — | — | IG-only (108K IG followers). Search returns zero results for the TikTok URL. |
| @almobadir_media | _none_ | — | NOT on TikTok | — | — | — | IG-only. Search returns zero results. |
| @badrshaqer (founder) | _none_ | — | NOT on TikTok (under that spelling) | — | — | — | His IG bio (325K IG, 37.6K Threads) cross-links to **Instagram + Threads only — no TikTok**. The string `@badrshaqer` is referenced inside `@almobadir`'s TikTok captions ("للمزيد من المحتوى القيم: @badrshaqer") as a vanity callout to his IG, not a TikTok @-mention. |
| @shalnack | _out of scope_ | — | N/A | — | — | — | Shalnack Ryusei is the **Creative Director at Almobadir** (LinkedIn confirmed), not a sister-brand handle. Should not be listed alongside the brand network. |

## Total TikTok reach

**Cannot be quantified from public-search data alone.** TikTok's profile pages render stats via JavaScript that web-fetching cannot execute, and Bing/Google snippets do not surface follower numbers for any of the three confirmed handles. To get exact numbers you (or anyone with a phone/desktop browser) need to load `tiktok.com/@almobadir`, `…_mindset`, and `…_flousak` and read the counters — this takes 30 seconds total.

For sizing context (rough order of magnitude only, NOT to be quoted as fact):
- IG `@almobadir` = 318K, IG `@almobadir_business` = 108K — Almobadir's audience scale on IG is **mid-six-figures per handle**
- TikTok-Saudi creators in the business/finance niche typically have **higher** TikTok follower counts than IG (the platform skews younger and Saudi penetration is ~154% of adults per Statista)
- A reasonable working estimate before manual verification: **the three TikTok handles combined likely sit somewhere between 500K and 2M followers**, but treat that band as a hypothesis until checked

When you check, also note the audience-overlap caveat: a non-trivial fraction of the same humans follow the parent + the two sub-handles, so summed-follower count overstates unique reach.

## Content style differences (TikTok vs Instagram)

Based on the indexed video captions and hashtags from `@almobadir` on TikTok (#تحفيز #نجاح #تنمية_بشرية #استثمار #بزنس #اقتصاد #fyp #foryou), the **content is the same vertical pillars as Instagram — finance, business case studies, motivation, side-hustle ideas** — repurposed into TikTok's vertical short-form. Captions are noticeably more casual ("ما رأيك بالموضوع؟؟", "ماذا ستفعل لو…") than the more polished Instagram carousels, and the format leans on hook-question + 30-60s explainer. There is also at least one TikTok-only audience-acquisition tactic visible: cross-promotion of a separate handle (`@random.arabic`) for entertainment-only spillover, suggesting the team uses TikTok to seed multiple owned audiences rather than treating it as a single mirror of IG.

## How to mention TikTok on the almobadir.com website

A few concrete options, ordered from least to most invasive:

- **Hero KPI panel** — the existing line "...عبر إنستغرام، يوتيوب، تيك توك" is **technically accurate** (TikTok presence does exist for 3 out of 5 network handles), but it implies parity. Two upgrades:
  - Conservative: keep the line as-is — it's defensible.
  - Sharper: change to **"شبكة من 5 حسابات على إنستغرام، 3 منها أيضاً على تيك توك"** (a network of 5 IG handles, 3 also on TikTok) — this is more honest and gives the reader a clean factual claim.
- **Channel chips** — for the three handles that have a TikTok counterpart (`@almobadir`, `@almobadir_mindset`, `@almobadir_flousak`), add a small "TT" pill next to the IG pill. For `@almobadir_business` and `@almobadir_media`, leave the IG pill alone — don't fake parity.
- **Footer sitemap** — don't add a separate "TikTok column"; that visually overweights TikTok relative to IG (which is the larger audience). Instead, on each network item list both URLs inline: `Instagram: @almobadir · TikTok: @almobadir`. For the IG-only handles, just show the IG line.
- **Network cards** — a small `TikTok` text badge next to the follower count on the three relevant cards is the cleanest signal. Don't put one on `@almobadir_business` or `@almobadir_media` since they don't have a TikTok.
- **Critical fix to the official site itself** — the current footer of almobadir.com only lists generic IG/FB/Twitter icons (no TikTok icon at all). Whatever the homepage of almobadir.com claims about a multi-platform network, the **footer should add a TikTok icon linking to `@almobadir`** (the parent), or the brand looks inconsistent with its own copy.

## Sources

- [TikTok @almobadir profile (search-indexed, name "المبادر | مال و أعمال")](https://www.tiktok.com/@almobadir) — confirmed handle exists with display name and active video catalog
- [TikTok @almobadir_mindset profile (name "المبادر | تحفيز و تطوير الذات")](https://www.tiktok.com/@almobadir_mindset) — confirmed via Bing site:tiktok.com search
- [TikTok @almobadir_flousak profile (name "المبادر | تدبير و إقتصاد")](https://www.tiktok.com/@almobadir_flousak) — confirmed via Bing site:tiktok.com search
- [Instagram @almobadir (318K followers, "مال و أعمال")](https://www.instagram.com/almobadir/) — IG counterpart confirmed
- [Instagram @badrshaqer (325K followers, "بزنس و تسويق")](https://www.instagram.com/badrshaqer/) — confirmed founder handle
- [Threads @badrshaqer (37.6K followers)](https://www.threads.com/@badrshaqer) — second-platform link from IG bio; **no TikTok link in his bio**
- [Instagram @almobadir_business (108K followers)](https://www.instagram.com/almobadir_business/) — IG-only; no TikTok counterpart found in any indexing
- [LinkedIn — Shalnack Ryusei, Creative Director at Almobadir](https://www.linkedin.com/in/shalnack-ryusei-105946186/) — proves `@shalnack` is a person at the company, not a sister brand
- [Favikon — Top B2B Influencers in Saudi Arabia (Badr Shaqer ranked #6, founder of @almobadir)](https://www.favikon.com/blog/top-b2b-influencers-saudi-arabia) — third-party confirmation Badr Shaqer founded Almobadir; lists no TikTok stats for him
- [almobadir.com — homepage and "Who We Are" page](https://almobadir.com/%D9%85%D9%86-%D9%86%D8%AD%D9%86/) — footer shows IG, FB, Twitter icons; **no TikTok icon present on the official website**
- TikTok caption referencing `@badrshaqer` as a cross-platform pointer: https://www.tiktok.com/@almobadir/video/7088715272810401030 (Apr 2022) — establishes Apr 2022 as a lower bound on the parent-handle's TikTok presence

## What this report does NOT confirm (manual check needed)

The following data points need a **30-second manual check** by opening the three TikTok URLs in any browser, because TikTok blocks automated scraping:

1. Exact follower count for `@almobadir` (TikTok)
2. Exact follower count for `@almobadir_mindset` (TikTok)
3. Exact follower count for `@almobadir_flousak` (TikTok)
4. Total cumulative likes per handle (number under "Likes" label on each profile)
5. Last-posted date / posting cadence — to confirm the accounts are still actively updated in 2026 (search-indexed videos go up to 2024; nothing confirms 2025-26 activity)

Recommend a quick mobile or desktop visit before committing copy to the live site.
