# Almobadir.com · Homepage Spec v3 · Network model

> Based on Badr's MCQ Round 2 (Q4): the references are **thmanyah**, **valuetainment**, **impacttheory**, **thefutur** — not Hormozi / Bloom / Abdaal.
> The site is a **multi-show network**, not a personal brand or single-essay publication.
> This spec replaces the v1 brochure-front and the rolled-back v2 publication-front.

---

## What changed in our model

| Was | Now |
|---|---|
| Founder-led personal brand site | Multi-show editorial network |
| Single essay / single creator voice | Multiple shows under one parent brand |
| Newsletter as the only opt-in | Newsletter as one of several CTAs |
| Founder = whole brand | Founder = one face of the network |
| 11 social handles as social proof | 4 editorial lanes as branded shows; 11 handles fold into them |
| One paid arm (agency) | Portfolio of 5 paid arms shown as a directory |

Reference IA pattern most relevant: **thmanyah.com** (Saudi/pan-Arab, multi-show podcast + video network with per-show landing pages).

---

## Information architecture

```
[Header — slim, network-mode]
[1. Hero            — The network statement + latest-drop pill]
[2. Shows grid      — 4 editorial lanes as branded show cards]
[3. Newsletter      — single visible CTA, AR-first, inline only]
[4. Paid arms       — portfolio of 5 destinations]
[5. Founder slot    — Badr as founder of the network, not the network]
[6. Latest          — most recent piece across the network]
[Footer             — keep current, with shows + paid arms + legal]
```

---

## Section-by-section

### 1. Hero — "The network"

**Job:** within 3 seconds, visitor understands Almobadir = a multi-show Arabic business + finance network.

**Visual primitive:** keep the v1 wordmark + spring entrance + hover glow (it's strong). Replace the founder-confessional tagline with a **network statement.**

**Copy stubs:**

AR primary:
```
المبادر
شبكة محتوى الأعمال والمال للعالم العربي.
أربع لاينات. ١١ قناة. ٣٫٣ مليون متابع. صوت واحد.
```

EN mirror:
```
Almobadir
The Arab world's business + finance network.
Four shows. Eleven channels. Three million followers. One voice.
```

**Below the wordmark:** a thin "live now / latest drop" pill — pulls the most recent piece from the network (latest IG post, latest TikTok, latest newsletter issue). Direct steal from thmanyah's homepage pattern.

**Killed:** founder photo, KPI cascade, manifesto, "SAUDI ARABIA / SYNC LIVE / KSA" mono block (already gone via MENA pass).

**Components reused from v1:** wordmark + spring entrance, crimson rule, page scroll progress.

---

### 2. Shows grid — the 4 lanes

**Job:** the visitor enters via the show they already love. (thmanyah pattern — homepage IS the shows grid.)

**Layout:** 4-card grid · 2×2 on mobile · clicks through to per-lane pages.

| Lane | Color (current handle palette) | Description (one line) |
|---|---|---|
| **Mindset** · مايندست | #1FA37A green | Self-development, mental models, how to think |
| **Flousak** · فلوسك | (azure) | Personal finance, financial freedom, money clarity |
| **Shalnack** · شلنك | #E8B855 gold | Creative direction, Islamic guidance, design wisdom |
| **Business** · بزنس | #E6213B crimson | Business, marketing, founder POV, deals |

**Each card has:**
- Lane name (AR + EN paired, like a show identifier)
- One-line description
- Color stripe identifier
- "Latest from lane" — pulls latest post handle for that lane
- Clicks through to `/mindset/`, `/flousak/`, `/shalnack/`, `/business/`

**Biggest IA change:** currently the homepage has a "network grid" of 11 social handles. The new structure folds those 11 into 4 *lanes*; each lane page lists the handles inside it. Network primitive flips from "handle" → "show".

---

### 3. Newsletter spine

**Job:** capture mechanic for the value-distribution layer. Same form as v1, repositioned as one of several CTAs (not THE CTA).

**Copy stubs:**

AR:
```
النشرة الأسبوعية
قراءة واحدة. كل أحد. بالعربية.
٥ دقائق · اقتصاد، أعمال، تحليل من قلب أسواق المنطقة.
```

**Visual:** single email input + button. Inline only. **NO popup, modal, scroll-trigger** (HANDOFF anti-pattern).

**Component reused:** existing `assets/newsletter.js` — backend swap for ESP happens in 1 line of config.

---

### 4. Paid arms — portfolio of 5 destinations

**Job:** show the network's businesses. Visitor sees Almobadir is a **portfolio**, not a single product.

**Layout:** 5-card grid. Compact, dense — looks like a holding-company directory, not a "products" hard-sell.

| # | Card | Status | CTA | Destination |
|---|---|---|---|---|
| 1 | **Almobadir Media** — agency | Live | "احجز اجتماعاً" | calendly.com/badrshaqer_consulting/almobadirmedia |
| 2 | **Badrshaqer.com** — consulting (sessions + transformation packs) | Live | "احجز جلسة" | badrshaqer.com |
| 3 | **Academy** — digital products for creators + biz owners | Coming | "اشترك للإطلاق" | email capture |
| 4 | **Productivity products** — merch + tools | Coming | "اشترك للإطلاق" | email capture |
| 5 | **Apps & micro-SaaS** — book summaries + habits + future | Coming | "اشترك للإطلاق" | email capture |

> "Coming soon" cards are an anti-pattern when vague. With each framed as clear what-this-is + email-capture for launch notify, they become credible bets, not vapor. Compare: thefutur.com's product grid mixes launched and unlaunched products side by side.

---

### 5. Founder slot — Badr as founder of the network

**Job:** humanize the brand without making the homepage about him. **Bilyeu-on-impacttheory pattern** — visible, but one of many.

**Layout:** single-section row, image left + 2-paragraph bio right. Smaller than the v1 founder panel.

**Copy stub (AR):**
```
بدر شاكر
مؤسس المبادر. أطلق الشبكة في ٢٠٢٢ على فرضية أنّ أعقد مفاهيم الأعمال يمكن سردها بنفس جاذبية الأفلام.
أربع سنوات لاحقاً، ٣٫٣ مليون متابع عبر ١١ قناة، ووكالة تنفيذية تخدم ١٤٣+ علامة.
[كل المحتوى من بدر ←]   /about/
```

**Photo slot:** placeholder (current AI image stays). MCQ T5 still open.

---

### 6. Latest — most recent content drop

**Job:** prove the network is alive. (thmanyah / valuetainment pattern: homepage shows the latest episode/issue.)

**Layout:** 3-card row of the most recent network drops, mixed across lanes. Title + lane tag + thumbnail + 2-line teaser.

**v3.0 launch:** hand-curated. **Later:** pulled from RSS / sitemap.

---

### 7. Footer

Keep current footer with three changes:
- "Network" link list surfaces the **4 lanes** (not 11 handles directly — handles surface inside lane pages)
- New "Paid arms" column — 5 links to almobadir.media, badrshaqer.com, academy, merch, apps
- Keep MENA neutralization from commit `07220df`

---

## What survives from v1

- Wordmark + spring entrance
- Cookie consent banner
- JSON-LD schema (already pan-MENA-fixed)
- AR/EN lang toggle + lang-toggle morph
- ⌘K command palette (entries need updating to add 4 lane pages + 5 paid arms)
- Page scroll progress
- Newsletter abstraction (`newsletter.js`)

## What gets killed (or moved)

| Element | Disposition |
|---|---|
| Hero KPI cascade | killed; "3.3M / 11 / 4" claim moves into the network statement line |
| Manifesto ink-bleed section | moves to `/about/` |
| Founder portrait as homepage centerpiece | moves to founder section + `/about/` |
| 11-handle network grid | folded into 4-lane shows grid |
| Method numerals + cards | moves to `/about/` or each lane page |

## What needs to be built net-new

- 4 lane pages: `/mindset/`, `/flousak/`, `/shalnack/`, `/business/`
- 5 paid-arm cards on homepage (with email-capture for the 3 unlaunched)
- "Latest" content row (hand-curated for v3)
- `/library/` for lead magnets + quizzes (Phase 3, deferred)
- Quiz mechanic (Phase 3, deferred)

---

## Sequencing

| Phase | Scope | Blocked on |
|---|---|---|
| **1** | Ship the new homepage IA with copy stubs filled in. Lane pages can be empty placeholders for v3.0. | Badr decisions in §"Outstanding" below |
| **2** | Flesh out the 4 lane pages with lane-specific descriptions + handles + lane-specific subscribe. | Phase 1 ships |
| **3** | `/library/` + quizzes + first 3 lead magnets. | Phase 2 ships + lead-magnet writing |
| **4** | Real founder photo + remaining IG handle handoff tasks. | Photo session (MCQ T5) |

---

## Outstanding before Phase 1 can ship

| # | Block | Owner | Notes |
|---|---|---|---|
| 1 | Confirm shows-grid naming: keep handle names (Mindset / Flousak / Shalnack / Business) or rebrand as show titles? | Badr | 30 sec — default = keep handle names for v3 |
| 2 | Confirm founder slot weight: small / medium / paired-with-hero? | Badr | Default = small (Bilyeu-pattern) |
| 3 | Approve the 5 paid-arm cards as listed | Badr | 1 min |
| 4 | Newsletter ESP final pick | Both | See ZAAP-DECISION below |

---

## Future Q5 round (deferred — don't trigger yet)

These are the questions the Q4 network-framing answer triggers, but we save them for after Phase 1 ships:

1. **Show names** vs handle names — should the on-site brand be different from the social handle?
2. **Founder slot weight** — exact pixel weight on the homepage
3. **Latest issue / latest video** surfacing — auto-pull or hand-curated?
4. **Conference / live events** — three of the four references run physical events (Vault, Impact Live, Futur Festival, thmanyah live tapings). Should the design leave a slot for a future "Almobadir Live"?
