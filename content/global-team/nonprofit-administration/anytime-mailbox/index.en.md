---
title: "AnytimeMailbox (virtual business address)"
linkTitle: "AnytimeMailbox"
weight: 30
---

The address printed on RLadies+'s 501(c)(3) documents, on every donor receipt, and on every form California asks the org to fill in is an AnytimeMailbox location in Oakland.
Physical mail sent to that address arrives at a real building staffed by real people who open the envelopes, scan the contents, and upload them to a dashboard within hours.
The org can therefore have a US business address — required by California for nonprofit registration — without any leadership member having to live nearby or check a physical PO box.

## What AnytimeMailbox does

AnytimeMailbox ([anytimemailbox.com](https://www.anytimemailbox.com)) operates a network of physical mail-handling locations that rent street addresses to remote businesses.
For RLadies+, the Oakland address handles:

- **Receiving physical mail and packages** on behalf of the org.
- **Scanning envelope exteriors** as soon as items arrive; on request, scanning the contents.
- **Forwarding originals** to any address (typically only when leadership specifically needs the physical document — most things stay digital), or **shredding** after scanning for the routine paper that doesn't need keeping.
- **Acting as the printed mailing address** on every official document, donor receipt, grant application, and tax filing.

California requires every registered nonprofit to have a physical street address on file with the Secretary of State; a PO box won't satisfy the requirement.
AnytimeMailbox's street address does — that's why this service exists in the stack rather than just a cheaper PO box.

## The address

TODO: confirm whether the actual street address belongs in this page.
The address is public (it's on donor receipts and the website's donation page) so probably yes; flagging for the maintainer to fill in or to point at the canonical record in the website repo if preferred.

## Plan

TODO: confirm AnytimeMailbox tier (typically Basic / Plus / Premium tiers differ on scan-page allowance and forwarding turnaround) and the monthly cost.

## Who has access

- Admin tied to the shared leadership role mailbox; credentials in the [1Password]({{% relref "/global-team/infrastructure/1password" %}}) Shared vault.
- Leadership members handling mail-triage duties should have their own logins.
- TODO: list current AnytimeMailbox users and their roles.

## Operational tasks

### Signing in

1. Open the AnytimeMailbox Customer Portal — TODO: confirm exact URL (typically [anytime-mailbox.com](https://www.anytime-mailbox.com) → Customer Login).
2. Sign in with credentials from 1Password.
3. Complete 2FA.

### Reviewing the inbox

1. The **Mail** tab is the dashboard for everything received.
2. Items are sorted by date with a thumbnail of the envelope exterior.
3. New items have an "unactioned" state — they sit there until someone tells AnytimeMailbox what to do.

The right cadence is weekly: someone walks the list, decides per item, clears the unactioned queue.

### Per-item actions

For each unactioned item, there are four common paths:

- **Open & Scan & Shred** — for routine paper that has informational value (a thank-you note from a partner, a check stub) but no need to keep the physical: AnytimeMailbox opens, scans the contents into the dashboard, and shreds the physical. This is the default for most mail.
- **Open & Scan & Hold** — same as above, but keep the physical at the facility for later forwarding. Use for anything that might need to be physically presented somewhere (rare).
- **Forward** — AnytimeMailbox ships the original to a specified address. Use for cheques (which need physical deposit), original signed documents the IRS may request, and anything Wise / Relay specifically asks for in original form. Choose shipping speed at request time.
- **Shred** — for obvious junk mail (offers, advertisements). The envelope exterior scan is usually enough to identify it.

The click path:

1. Click the envelope in the Mail tab.
2. **Actions** dropdown → choose the action.
3. Confirm; AnytimeMailbox staff process within their published turnaround (typically same-day for scans, 1-2 days for forwarding).

### Forwarding a piece of mail to a leadership member

1. Click the item → **Actions** → **Forward**.
2. Enter the destination address.
3. Choose shipping option (USPS first-class is fine for most documents; FedEx for anything time-sensitive or valuable).
4. Confirm; AnytimeMailbox charges the per-shipment fee to the account.

### Picking up a package

Packages (rather than letter mail) sometimes can't be forwarded cheaply.
For these, contact AnytimeMailbox's location directly using the contact info in the portal; arrange local pickup or pay the higher package-shipping rate.

### Setting up email notifications

1. **Settings** → **Notifications**.
2. Enable notifications for new arrivals so leadership knows when something needs triaging.
3. Choose where they're sent — typically the role mailbox plus the named primary triager.

### Adding an authorised recipient name

Mail addressed to a name not on the authorised recipient list may be refused or held indefinitely.
For RLadies+, the authorised list should include the org's legal name, common abbreviations (RLadies+, R-Ladies Global, etc.), and any past or present officer names that may still appear on legacy correspondence.

1. **Settings** → **Authorised Recipients** → **Add**.
2. Enter the name.
3. Save.

Walk the list every six months and prune anything stale.

## Recovery

2FA codes in 1Password; recovery email is the shared role mailbox, so Workspace access is the prerequisite.
If the AnytimeMailbox account itself is unreachable, the facility location can be contacted directly (location address and phone number in the portal) to confirm continued mail receipt while the account access is restored.

## Things to watch for

- **Pending mail accumulating.** A backlog of unactioned items is how genuinely important notices get missed. Set the weekly review cadence and stick to it.
- **Mail to former-officer names.** Five years on, old donor databases still occasionally send to founders or past treasurers. Either keep those names on the authorised list (so the mail is received and triaged) or accept it'll bounce.
- **Tax-season spikes.** January through April brings more government correspondence than any other quarter. Plan for it.
- **Subscription auto-renewal.** Billing notifications land on the role mailbox; forward to the finance-responsible leadership member for monthly reconciliation alongside Relay/Wise/PayPal.
- **Service-of-process documents.** If anything that looks like a legal summons arrives, route it to VCorp immediately (cross-link [VCorp]({{% relref "../vcorp" %}})) — there are statutory response windows on legal documents that don't pause for triage.
