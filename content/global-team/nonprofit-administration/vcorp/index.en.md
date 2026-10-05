---
title: "VCorp (California compliance and filings)"
linkTitle: "VCorp"
weight: 10
---

Every annual Statement of Information, every Form 990 filing, every notice from the California Franchise Tax Board lands first with VCorp — who scan it, email it, and prepare the response for leadership to approve.
The arrangement is what lets RLadies+ stay in good standing as a California nonprofit even though no leadership member has to physically open mail at a California address or remember when the next biennial filing is due.

## What VCorp does for us

VCorp Services ([vcorpservices.com](https://www.vcorpservices.com)) is the registered agent and compliance partner.
Specifically:

- **Registered agent service.** California law requires every corporation (including 501(c)(3) nonprofits) to designate an in-state agent for service of process. VCorp's California address is the official one for RLadies+, and any legal documents (subpoenas, regulatory notices, lawsuit service) arrive there first.
- **Statement of Information filings.** California requires a biennial Statement of Information from nonprofits, listing officers and the principal address. VCorp tracks the deadline, drafts the filing, and submits to the Secretary of State after leadership review.
- **Form 990 and IRS filings.** The annual Form 990 (or 990-EZ for smaller orgs) goes to the IRS. VCorp prepares the draft from financial data we provide; leadership reviews and approves; VCorp files.
- **Franchise Tax Board correspondence.** California's FTB sends periodic notices — exemption renewals, address verifications, occasional questionnaires. VCorp surfaces them and recommends responses.
- **Document forwarding.** Anything official that arrives at the registered-agent address gets scanned and emailed within a working day.

## Plan and billing

TODO: confirm the annual VCorp engagement cost and renewal month.
The fee typically covers registered-agent service plus a set number of filings; additional filings or amendments may incur per-event fees.

## Who has access

- The VCorp customer portal admin login is in the [1Password]({{% relref "/global-team/infrastructure/1password" %}}) Shared vault.
- VCorp correspondence arrives at the role mailbox, plus the named primary contact in the VCorp account (TODO: confirm who).
- Leadership members with finance or compliance responsibilities should have their own VCorp portal logins; admin operations stay with the admin login.

## Operational tasks

### Signing in

1. Open the VCorp portal — TODO: confirm exact URL (typically [vcorpservices.com](https://www.vcorpservices.com) → Client Login, or a direct portal subdomain).
2. Sign in with credentials from 1Password.
3. The dashboard shows pending filings, deadlines, and any forwarded documents awaiting acknowledgement.

### Acknowledging a forwarded official notice

When VCorp scans and forwards a piece of mail:

1. Open the PDF from the email or from the portal's Documents tab.
2. Share with the leadership Slack channel (TODO: confirm which — `#leadership`? `#finance`?) with a one-line summary of what it is and what it appears to need.
3. Decide who actions it — VCorp can usually file the response on our behalf once we've decided the substance.
4. Save the original to the compliance folder in the Workspace Shared Drive (see [Shared Drive]({{% relref "/global-team/infrastructure/google-workspace/shared-drive" %}})) for the audit trail.

### Approving a Statement of Information draft

The biennial Statement of Information arrives as an emailed draft from VCorp:

1. Open the draft. Verify the listed officers match current leadership, the principal address is correct, and the agent information is accurate.
2. If anything is wrong, reply to VCorp with the corrections.
3. If correct, reply approving the filing.
4. VCorp files with the Secretary of State and emails confirmation; save the confirmation to the Shared Drive.

### Approving the annual Form 990

Form 990 needs financial data we provide (from [Relay]({{% relref "/global-team/finance/relay" %}}), [Wise]({{% relref "/global-team/finance/wise" %}}), and [PayPal]({{% relref "/global-team/finance/paypal" %}}) exports):

1. Provide VCorp the year's financial data — typically a year-end summary of income (donations by category, grants, in-kind), expenses by category, and balance sheet snapshots.
2. VCorp drafts the Form 990 and emails a review copy.
3. Leadership (typically the treasurer role or the named primary contact) walks the draft line-by-line and confirms accuracy.
4. Approve; VCorp e-files with the IRS and emails the filing confirmation plus the public copy.
5. The public Form 990 also gets published to the website (TODO: confirm the publication location).

### Downloading the 501(c)(3) determination letter

Donors, partners, and grant applications regularly ask for the IRS determination letter as proof of nonprofit status:

1. Sign in to the VCorp portal → Documents tab.
2. Find the most recent IRS determination letter (the original 501(c)(3) approval; should remain valid unless the org's exempt status has been re-evaluated).
3. Download and share.

A copy should also live in the Shared Drive's compliance folder for quick access without needing to sign in to VCorp.

## Annual calendar

TODO: confirm the dates that matter for RLadies+'s fiscal year. The typical California nonprofit calendar:

- **January-April**: tax season; Form 990 due 4.5 months after fiscal year end (for a calendar-year nonprofit, due May 15)
- **Biennial month (TODO: confirm RLadies+'s)**: Statement of Information due
- **Annually (TODO: confirm month)**: California FTB exemption renewal
- **Quarterly or as-needed**: any federal or state form changes; VCorp surfaces these

Putting the deadlines in a shared calendar that leadership all see (rather than relying on VCorp's reminders) catches the case where a VCorp email gets missed.

## Recovery

If the role mailbox becomes unreachable, VCorp customer service can be contacted directly using the support phone number in the portal to re-route notices and trigger a portal-access reset.
The recovery email itself remains the same role mailbox, though, so restoring Workspace access is the real first step — cross-link [Google Workspace admin recovery]({{% relref "/global-team/infrastructure/google-workspace/admin-console" %}}#troubleshooting).

## Things to watch for

- **Deadline emails.** Missing a Statement of Information triggers a $50-$250 penalty plus eventual loss of good standing; missing a Form 990 risks IRS revocation of 501(c)(3) status after three consecutive years of non-filing. Treat VCorp deadline notices as load-bearing.
- **Annual fee renewals.** VCorp's annual renewal usually arrives 60-90 days before the term ends. Confirm the budget covers it and process payment from [Relay]({{% relref "/global-team/finance/relay" %}}).
- **VCorp_files but we_decide.** VCorp drafts filings using information we provide; if leadership composition changes mid-cycle without VCorp being told, the next Statement of Information will list the wrong officers. Update VCorp at the time leadership rotates, not at filing time.
- **State-of-California audit risk.** California occasionally audits nonprofit filings. Keep the Shared Drive compliance folder current; if an audit notice arrives, route it to VCorp immediately rather than attempting to respond directly.
