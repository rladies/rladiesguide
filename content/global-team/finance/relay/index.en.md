---
title: "Relay (business banking)"
linkTitle: "Relay"
weight: 10
---

Every USD-denominated transaction RLadies+ makes — paying for Zoom, reimbursing a chapter for venue costs, receiving a grant disbursement, sweeping a PayPal balance — flows through a single Relay account.
Relay is the operating bank: the place statements come from, the place cards are issued from, the place the routing and account number on every wire-transfer instruction point at.

## Why Relay

Relay ([relayfi.com](https://relayfi.com)) is a US business banking platform aimed at small businesses and nonprofits.
The fit for RLadies+ is straightforward: no monthly account fees on the standard tier, unlimited virtual debit cards (useful for scoping subscription payments), clean transaction exports for bookkeeping, and a UI that treats multiple human users as a first-class concept rather than an awkward add-on.
The alternative — opening a traditional US business bank account from outside the US, with leadership scattered globally — would be a lot more friction for the same end state.

TODO: confirm whether RLadies+ is on Relay's free tier or Relay Pro (Pro adds wire transfers and automated rules).

## Who has access

The Relay admin account is tied to the role mailbox and credentials live in the Shared vault in [1Password]({{% relref "/global-team/infrastructure/1password" %}}).
Leadership members are added as individual Relay users with their own logins rather than sharing the admin credentials — Relay supports per-user roles (Admin, Bookkeeper, Approver, View Only) and per-user roles make audit trails meaningful.

TODO: list the current Relay user roster and their assigned roles.

## Operational tasks

### Signing in

1. Open [app.relayfi.com/login](https://app.relayfi.com/login) in your browser.
2. Sign in with your individual Relay user credentials (not the shared admin login unless you specifically need admin operations).
3. Complete the 2FA challenge using the authenticator app you set up at first login; recovery codes are in 1Password under your Relay item if you've stored them there.

### Viewing transactions and exporting for bookkeeping

1. Click **Transactions** in the left sidebar.
2. Set the date range using the filter at the top right.
3. Click **Export** → CSV.
4. Upload the CSV to the finance folder in the Workspace Shared Drive (see [Shared Drive]({{% relref "/global-team/infrastructure/google-workspace/shared-drive" %}})).

Doing this monthly keeps reconciliation manageable and gives [VCorp]({{% relref "/global-team/nonprofit-administration/vcorp" %}}) clean source data for the annual Form 990.

### Creating a virtual card for a subscription

Virtual cards let you cap a subscription's monthly limit and revoke it without touching the physical card. Use one per service so a leak or runaway charge is contained.

1. Click **Cards** in the left sidebar.
2. Click **+ New card** → choose **Virtual** → **Subscription**.
3. Name the card after the service it pays (e.g., `Zoom subscription`, `Canva Pro`).
4. Set the monthly limit just above the expected charge.
5. Copy the card number into the 1Password entry for the service that will use it.
6. Use the card details to set up payment in the service's billing settings.

### Issuing an ACH transfer

1. Click **Payments** → **Send payment**.
2. Choose **ACH** as the method.
3. Enter recipient details (routing, account number, name on account).
4. Set the amount and the description (descriptions show up on the bank statement — make them meaningful for bookkeeping).
5. Schedule for the next business day or a specific date.

For international payments, use Wise instead — see [Wise]({{% relref "../wise" %}}).

### Adding a new Relay user

1. Sign in as an Admin.
2. Click **Settings** → **Team** → **Invite member**.
3. Enter the new user's email (their personal email is fine; their Relay login is separate from any other login).
4. Choose a role (Admin, Bookkeeper, Approver, View Only) appropriate to the work they will do.
5. They accept the invitation via email and complete their own 2FA setup.
6. Confirm they can see the expected accounts and transactions.

### Removing a departed user

1. **Settings** → **Team** → click the departing user → **Remove**.
2. Revoke any virtual cards they personally created (Cards → filter by creator → freeze or close).
3. If they had access to physical cards or the admin login, rotate the relevant credentials and update 1Password.

## Recovery

If 2FA breaks on a user's login, recovery codes saved in 1Password at first sign-in are the first option.
If the admin login is the blocker, Relay support can recover via the role mailbox — which means Google Workspace access is the real prerequisite to financial recovery.
Cross-link: [Google Workspace admin recovery]({{% relref "/global-team/infrastructure/google-workspace/admin-console" %}}#troubleshooting).

## Things to watch for

- **Subscription auto-renewals on virtual cards.** A virtual card with a $50/month limit won't catch a service that quietly raises to $100/month — review transactions monthly.
- **Stale user access.** Departing leadership rarely flag their own offboarding; the six-monthly admin review (see [1Password]({{% relref "/global-team/infrastructure/1password" %}})) should include a Relay user-list walk.
- **Monthly statement to the role mailbox.** Make sure someone in finance actually reads it; treat it as evidence rather than noise.
- **Tax-relevant transactions.** Large incoming grants and outgoing honoraria both have 1099 / W-9 implications; flag them to VCorp at the time of the transaction, not at year-end.
