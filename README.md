# Almobadir Handoff Package

> Self-contained working folder for the almobadir.com website project.
> Everything inside is what a new working session needs to pick up where the prior one stopped.

## What's in this folder

```
almobadir-handoff/
├── README.md                     ← this file (quick orientation)
├── HANDOFF.md                    ← FULL brief for the next session — read first
├── BADR-MCQ.md                   ← 11 questions waiting on Badr's answers
│
├── website/                      ← the live website source code
│   ├── index.html                ← AR homepage (the brochure version, currently live)
│   ├── en/                       ← EN mirror
│   ├── about/, press/, privacy/, terms/, newsletter/
│   ├── 404.html, robots.txt, sitemap.xml, llms.txt, CNAME
│   ├── assets/                   ← v3.css, v3.js, tokens.css, newsletter.js, cookie-banner.js, og/og-1200x630.png, etc.
│   ├── package.json              ← motion@12.38.0 dependency declared
│   └── (no node_modules — run `npm install` if needed)
│
└── audits/
    ├── canonical/                ← THE 5 MUST-READ DOCS
    │   ├── PUNCH-LIST.md           — the original forensic design audit
    │   ├── WOW-BLUEPRINT.md        — the visual signature execution plan
    │   ├── CONTENT-PACK.md         — the canonical content + positioning plan
    │   ├── audit-seo.md            — SEO emergency findings
    │   └── audit-brand-messaging.md — 28 brand-drift findings
    │
    └── supporting/
        ├── design/                ← 38 supporting design reports (per-section, signatures, references)
        └── content/               ← 22 supporting content reports (per-account harvests, topical deep-dives, dragnets)
```

## How to use this folder

### If you're moving to a new machine (e.g., Mac):
1. Zip this entire folder, AirDrop or OneDrive-sync it
2. On the new machine, unzip
3. Open Claude Code with the working directory set to this folder
4. **Open `HANDOFF.md` first** — paste its contents into the new session's first prompt

### If you're starting a new Claude session in-place:
1. Open this folder in Claude Code
2. **Open `HANDOFF.md` first** — paste it as your first message
3. The new session has full context and knows exactly what to do next

### If you're handing this to a different developer / agency:
1. Send them this whole folder
2. Tell them to read `HANDOFF.md` then `BADR-MCQ.md`
3. They'll know:
   - What's been built and shipped (commit log + 16-point launch checklist)
   - What's blocked and on whom (DNS, ESP wiring, founder MCQ answers)
   - What 10/10 means for this brand (the 5 canonical audit docs)
   - The exact rollback paths

## Quick state snapshot (as of handoff time)

| Item | Status |
|---|---|
| Live URL | https://zaka33333-hash.github.io/almobadir-website/ |
| Production domain | almobadir.com — DNS being wired at Hostinger by founder |
| Git branch | `main` at commit `11d7f77` (CNAME file added for production domain) |
| Rollback target | `git reset --hard ae72b8b` (the launch baseline before final restructure attempts) |
| Preserved tag | `publication-v2-attempt` (a Stratechery-style restructure that the founder rolled back) |
| Newsletter | Form captures emails to localStorage queue (simulation) — Buttondown wiring deferred |
| Analytics | Plausible script wired; awaiting `almobadir.com` to be added at plausible.io |
| Founder verdict | Site is currently 4/10 in his eyes — wants 10/10 |

## Read this if you forget anything else

> **`HANDOFF.md` is the briefing.** Everything else is reference.
> Don't try to operate without reading it first.
