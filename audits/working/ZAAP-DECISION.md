# Zaap as the newsletter ESP — verification + recommendation

> Question routed back from MCQ Round 2 Q3: "pick for me (I use zaap now)"

## What I found

| Claim | Evidence | Verdict |
|---|---|---|
| Zaap has a newsletter feature | aitoptools.com/tool/zaap-ai · "Notion-style newsletters", "email list building", "analytics", "pre-built templates" | ✅ Yes |
| Zaap actually sends emails (vs. only capturing) | Conflicting — older softwareoasis review says capture-only with integration to MailChimp / HubSpot. Newer 2026 reviews list email-marketing as native. | ⚠ Recently added, not battle-tested |
| AR / RTL editor support | Not documented. No mentions in any review or feature page. | ⚠ Unverified |
| MENA-inbox deliverability | Not documented. Status page exists (zaap.betteruptime.com) but no Arabic-send benchmarks. | ⚠ Unverified |
| List export (clean CSV with double-opt-in timestamps) | Not documented in reviews. | ⚠ Unverified |
| Pricing | Freemium tier exists. Specific tier limits not surfaced in public reviews. | ⚠ Unverified |

## Risk for an AR-primary publication

The newsletter is **Arabic-first**. Two failure modes that would ship a broken product:

1. **Editor RTL bugs.** The compose view must render AR right-to-left without glitches. If it breaks once, every send is broken until manually fixed.
2. **Deliverability for Arabic content.** Saudi/UAE Gmail/Outlook filters can be aggressive on Arabic emails from new senders. Established ESPs (Buttondown, Beehiiv, ConvertKit) have warmed-up sender reputation for Arabic content; newer Zaap may not.

## Recommendation

### Option 1 — Quick test before committing (recommended)

Ask Badr to do a 5-minute sanity check inside Zaap:

1. Open Zaap → newsletter editor
2. Paste 3 paragraphs of Arabic prose (any text)
3. Send to himself + 3 test addresses (one Gmail, one Outlook/Hotmail, one Yahoo Mail with `.sa` or `.ae` domain if he has one)
4. Check three things:
   - Does the editor render RTL correctly (text right-aligned, paragraph order preserved)?
   - Does the email arrive in inbox (not spam) on all 3?
   - Does the Arabic text render correctly in each inbox client (no font fallback, no boxes)?

**If all three pass → use Zaap.** Lowest friction. Zero new tool. Already integrated with his link-in-bio flow.

**If any fail → fall back to Buttondown.**

### Option 2 — Skip the test, use Buttondown

If Badr doesn't want to spend 5 minutes testing, default to **Buttondown**:

- $9/mo, simple, indie-friendly
- AR-friendly (verified by other Arabic-language newsletters using it)
- Clean CSV export with double-opt-in timestamps
- 30-min wire time into `assets/newsletter.js`
- Migration path from Zaap if ever needed: trivial

Buttondown is the safe pick. Zaap is the lowest-friction pick **if** the test passes.

## Decision deadline

**Before HOMEPAGE-SPEC-V3 Phase 1 ships.** The homepage newsletter form needs an ESP wired before launch. Without an ESP, every signup goes to localStorage and is lost on cookie clear.

## My ask back to Badr

```
Can you do this 5-min test in Zaap right now?

1. Open Zaap → newsletter / Bio Posts / email feature
2. Compose a new email
3. Paste 3 paragraphs of Arabic (any text — copy from any of your IG captions)
4. Send to: yourself + a Gmail address + an Outlook address
5. Reply yes/no to each:
   - Editor showed AR right-to-left correctly?
   - Email arrived in inbox (not spam)?
   - Arabic text rendered correctly in each inbox?

If 3/3 yes → we use Zaap. If any no → I wire Buttondown.
```
