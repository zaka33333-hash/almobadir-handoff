# Almobadir · Full Handoff (for my love 💛)

> Hey beautiful — Zak here. I'm handing you the Almobadir project. Everything you need is in this doc. Read it once top to bottom, then keep it open as a reference. The AI agent you'll work with (Claude Code) is good but not psychic — this doc is what makes it useful to you.
>
> **Time to get oriented:** ~30 min reading + ~15 min poking around the repo.
> **Time to be productive solo:** ~1 day.

---

## 0 · The 30-second version

- **Almobadir.com** = a pan-Arab / MENA business + finance content network founded by my brother **Badr Shaqer**.
- It's NOT a personal blog. It's a **network of 4 editorial lanes** (like Thmanyah, Valuetainment, Impact Theory, The Futur) feeding paid arms (agency, consulting, academy, products, apps).
- The 4 lanes: **Mindset** (verde), **Flousak** (azure, finance), **Shalnack** (gold, lifestyle/business), **Business** (crimson).
- Across 11 IG/TikTok handles → **3.3M followers combined**.
- The website is the **central hub** that captures email signups, hosts lead magnets, quizzes, essays, and routes traffic to the paid arms.
- Tech stack: **static HTML/CSS/JS**, hosted on **GitHub Pages**, custom domain `almobadir.com` (DNS not cut over yet — still on Hostinger/WordPress).
- I've shipped **Phase 1**. You're picking up **Phase 2** onward.

---

## 1 · Who's who

| Person | Role | Notes |
|--------|------|-------|
| **Badr Shaqer** | Founder. Moroccan. Lives in Saudi. **Public-facing he is nationality-neutral, pan-MENA.** Never call him Saudi externally, never call him Moroccan externally. | Communicates in Arabic, voice memos, WhatsApp. Slow to respond. Don't wait — keep shipping what doesn't need him. |
| **Zak (me)** | PM / brother / your bf. Pinging you for help because I'm capped on bandwidth. | I'm a Slack-message away if you're stuck. |
| **You 💛** | New PM + design lead on the project. Talks to the agent, reviews output, pushes Badr for answers. | |
| **Claude Code (agent)** | The AI that writes the code and content. You drive it. | |

---

## 2 · The brand rules (memorize these — easy to violate)

1. **Pan-MENA, nationality-neutral.** Never "Saudi entrepreneur", never "Moroccan founder", never "based in Riyadh". Use "MENA", "Arab world", "GMT+3" (not "Riyadh time"). The agent has a memory file enforcing this — if it slips, correct it.
2. **Arabic-primary, English-mirror.** Every page exists in `/` (AR, RTL) and `/en/` (LTR). AR is the source of truth.
3. **Network, not personal brand.** The homepage leads with the network (4 lanes, 11 channels, 3.3M followers), not with Badr's face. Badr's section is a slim "founder" slot lower on the page, à la Tom Bilyeu on Impact Theory.
4. **Don't invent facts about Badr.** Bio, vision, education claims — only what he confirms. The current bios were triangulated from public sources and are still waiting on his replacement copy.
5. **The 5 paid arms** (memorize these names):
   - Almobadir Media — agency (live, `almobadir.media`)
   - Badrshaqer.com — consulting (live)
   - Academy — for creators/business owners (soon)
   - Productivity products + merch (soon)
   - Apps & micro-SaaS (soon)

---

## 3 · Where everything lives

```
~/Desktop/almobadir-handoff/                     ← this repo (planning + docs)
├── HANDOFF.md                                   ← original handoff (read after this doc)
├── HANDOFF-FOR-GIRLFRIEND.md                    ← you are here
├── BADR-MCQ.md                                  ← old MCQ rounds (historical)
├── README.md
└── audits/
    └── working/
        ├── BADR-OPEN-ITEMS.md                   ← THE checklist Badr needs to fill in
        ├── HOMEPAGE-SPEC-V3.md                  ← network-model homepage spec
        └── ZAAP-DECISION.md                     ← newsletter ESP research

/tmp/almobadir-website/                          ← THE ACTUAL WEBSITE (separate git repo)
├── index.html                  ← AR homepage (live)
├── en/index.html               ← EN homepage (live)
├── mindset/  flousak/  shalnack/  business/    ← AR lane pages (placeholders)
├── en/mindset/  en/flousak/  ...               ← EN lane pages (placeholders)
├── about/  press/  …
└── sitemap.xml
```

**Heads up:** the website repo lives in `/tmp/` because I cloned it there. **Move it to `~/Desktop/almobadir-website/` first thing** so a system reboot doesn't nuke it. Then `cd ~/Desktop/almobadir-website && git remote -v` to confirm it's pointed at `zaka33333-hash/almobadir-website`.

**Live URL right now:** https://zaka33333-hash.github.io/almobadir-website/
**Production URL (not cut over):** https://almobadir.com (still old WordPress — DO NOT re-add the CNAME file until Badr finishes DNS step B1)

---

## 4 · What's done (Phase 1, commit `bfb0f02` and earlier)

✅ Hero copy rewritten as a network statement ("4 lanes. 11 channels. 3.3M followers. One voice.")
✅ Manifesto + method sections deleted from homepage (moved/queued for `/about/`)
✅ 7-handle grid replaced with 4-lane "shows" grid
✅ 5 paid-arm cards added (2 LIVE, 3 SOON)
✅ 8 lane page placeholders created (4 AR + 4 EN) with lane colors, descriptions, newsletter CTAs
✅ Sitemap updated with the 8 new URLs
✅ Full MENA neutralization across 19 files — killed "Saudi", "Riyadh", "Morocco" from public-facing copy (kept only in legal: PDPL refs, governing law, timezone identifier)
✅ CNAME removed (was causing a 301 redirect to the dead WordPress) — site is reachable on github.io
✅ `BADR-OPEN-ITEMS.md` checklist created — single fillable doc consolidating 7 questions + 10 tasks for Badr

---

## 5 · What's BLOCKED on Badr (don't start these — read `audits/working/BADR-OPEN-ITEMS.md`)

These need his answers before we can ship:

- **A1** Zaap newsletter test → which ESP to wire (Zaap vs Buttondown $9/mo)
- **A2** Newsletter cadence (default: weekly Sunday GMT+3)
- **A3** Real founder bio (3 lines, AR, his own voice) — replaces triangulated placeholder
- **A4** 12-month vision (paragraph or voice memo)
- **A5** The "one sentence" — hero lead line
- **A6** Live events slot? (yes / no / maybe)
- **A7** "Latest" homepage section: RSS-auto / hand-curated / kill
- **B1+B2** Hostinger DNS cutover + GitHub Pages custom domain
- **B3** Plausible signup
- **B4** 7 IG handles → Business accounts
- **B5** 4 IG handles missing bio link
- **B6** LinkedIn MBA education entry
- **B7** 5 press contacts
- **B8** Real founder photo session (Riyadh photographer)
- **B9** 3 lead-magnet outlines
- **B10** Quiz #1 archetypes ("What kind of Arab founder are you?")

**Your job re: Badr:** chase him for these answers. WhatsApp, voice memo, whatever works. The checklist file is share-able via GitHub link. **Don't let this stall more than a week.**

---

## 6 · What YOU can ship without Badr (Phase 2 polish)

In rough priority order:

1. **Move the website repo from `/tmp/` to `~/Desktop/almobadir-website/`** (5 min, safety)
2. **⌘K palette refresh** — currently lists the 7 old IG handles. Update to: 4 lane pages + 5 paid arms + newsletter + about + press. Search `<!-- cmdk -->` or `cmd-k` in `index.html` and `en/index.html`.
3. **Footer fold** — same problem: 7 handles → restructure into 2 columns (4 lanes / 5 arms).
4. **Founder section slim** — currently still verbose. Compress to: name + role + 1 line + photo placeholder + "more on /about/" link. Reference: Tom Bilyeu's slot on impacttheory.com.
5. **Move manifesto + method** content (currently deleted from homepage) **into `/about/`** so it's not lost.
6. **OG image PNG re-render** — `og-image.png` still says "RIYADH" — replace with neutral MENA-coded asset. Find any image generator (or ask the agent to write a script).
7. **Lane page enrichment** — the 4 lane pages are placeholders right now. Each should grow into a real page: lane manifesto, top 5 posts/clips from that lane's handles, newsletter signup, related paid arm.

**For each of these, open Claude Code in `~/Desktop/almobadir-website/` and brief the agent specifically.** See section 8 for how to talk to it.

---

## 7 · What's coming later (Phase 3+, blocked on Badr)

- `/library/` lead-magnet hub + 3 PDF lead magnets written
- Quiz #1 mechanic ("What kind of Arab founder are you?") + email capture wire
- Newsletter ESP fully wired (Zaap or Buttondown)
- Real founder photo integration site-wide
- Press kit + outreach drafts for 5 contacts
- Production domain re-wire (re-add CNAME after DNS lands)

---

## 8 · How to work with Claude Code (the agent)

You'll spend most of your time in a terminal, in the website repo, talking to Claude Code. Here's the playbook:

### Opening a session
```
cd ~/Desktop/almobadir-website
claude
```

### How to brief it
The agent **has no memory of our conversations**. Every new session starts cold. So:

1. **Point it at this doc first:** "Read `~/Desktop/almobadir-handoff/HANDOFF-FOR-GIRLFRIEND.md` and `~/Desktop/almobadir-handoff/HANDOFF.md` before doing anything."
2. **State the task in one sentence:** what + why + done-criteria.
3. **Give it the file paths.** Don't make it search.
4. **Tell it the brand rules** if the task touches copy (pan-MENA, nationality-neutral, AR-primary).

### Example good prompt
> "In `~/Desktop/almobadir-website/index.html` and `en/index.html`, the ⌘K command palette (search for `cmd-k`) currently lists 7 old Instagram handles. Replace those entries with: 4 lane pages (mindset, flousak, shalnack, business), 5 paid arms (almobadir media, badrshaqer.com, academy, products, apps), newsletter, about, press. Keep the same visual styling. AR file is source of truth — mirror to EN."

### Example bad prompt
> "fix the command palette" ← agent doesn't know which palette, which file, what fix.

### When to stop the agent
- It's going in circles → interrupt, rephrase, give it the answer.
- It's writing too much → "make this smaller" / "just the diff".
- It claims something is done → **always check the live site** before believing it. Open the github.io URL on your phone.

### Git hygiene
- The agent will commit. **Read the commit before pushing.** `git diff HEAD~1` is your friend.
- If the agent breaks the site, `git revert <hash>` and re-brief.
- Never `--force` push.

---

## 9 · The one button you'll press often

After Phase 2 changes ship to the website repo, the live site updates automatically via GitHub Pages (usually 1-2 min). Watch the deployment:

```
gh run watch
```

If a deploy fails, the agent can read the logs:
> "Run `gh run view --log-failed` and fix whatever broke."

---

## 10 · Daily / weekly rhythm I'd suggest

- **Daily (10 min):** check if Badr replied in WhatsApp. If yes → log answers into `BADR-OPEN-ITEMS.md`, then brief the agent on the unblocked task.
- **Weekly (2-3 hours):** one Phase 2 polish task (section 6).
- **Weekly (15 min):** open the live site on a phone. Scroll. Note anything broken / ugly / off-brand. Send to me OR brief the agent to fix.

---

## 11 · Stuff that will trip you up

- **The agent will sometimes call Badr "Saudi" or mention Riyadh.** Catch it. Correct it. There's a memory file enforcing the rule but agents drift.
- **The website is bilingual.** Any copy change in `/index.html` (AR) must also land in `/en/index.html` (EN). The agent usually handles this if you remind it.
- **Don't trust the agent's "done" claim.** Open the live URL.
- **CNAME file = death.** If you see anyone re-add a `CNAME` file before DNS is cut over, that 301-redirects the whole site to the dead WordPress. Don't.
- **`/tmp/` is volatile.** Move the website repo out of there immediately.

---

## 12 · If you're stuck

1. Re-read this doc.
2. Read `HANDOFF.md` (the original, more technical).
3. Check `audits/working/HOMEPAGE-SPEC-V3.md` for the design rationale.
4. WhatsApp me. Seriously, don't suffer alone.

---

## 13 · The vibe

This is a real business. Badr's audience is 3.3M people. We're building the central hub that turns followers into subscribers into customers. Take it seriously, but don't be precious — ship, observe, iterate.

You've got this. I love you. 💛

— Zak

*Last updated · 2026-05-12 · Phase 1 shipped (commit bfb0f02). Awaiting Badr's answers on `BADR-OPEN-ITEMS.md` to unblock Phase 2/3.*
