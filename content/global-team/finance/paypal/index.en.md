---
title: "PayPal (donations)"
linkTitle: "PayPal"
weight: 30
---

Most one-off donations to RLadies+ land in PayPal — community members clicking the donate link, conference attendees sending small amounts, anyone for whom PayPal is the convenient option.
The balance never accumulates there for long; it gets swept to [Relay]({{% relref "../relay" %}}) on a regular cadence so the operating account remains the source of truth.

## Account type

RLadies+ holds a PayPal nonprofit account, which qualifies for the reduced transaction-fee rate that PayPal offers verified 501(c)(3) organisations.
TODO: confirm whether the account is also enrolled in PayPal Giving Fund (PPGF), which lets donors give without the standard 2.99% fee in exchange for a slightly delayed disbursement cadence.

The 501(c)(3) verification on file is what unlocks the lower rate; if the IRS determination letter ever needs to be re-uploaded, the most recent copy lives in the Shared Drive's compliance folder (alongside what VCorp keeps).

## Who has access

Admin tied to the role mailbox, credentials in the [1Password]({{% relref "/global-team/infrastructure/1password" %}}) Shared vault.
Additional team members are added via PayPal's user-permissions system rather than sharing the admin login.

TODO: list the current PayPal team members and their assigned permissions.

## Operational tasks

### Signing in

1. Open [paypal.com](https://paypal.com).
2. Sign in with your individual PayPal user credentials, or the admin login from 1Password for admin operations.
3. Complete the 2FA challenge.

### Viewing donations

1. Click **Activity** in the top nav.
2. Filter by date range, or filter by type **Payments received**.
3. Each row shows the donor, amount, fee, and net.

For automated reporting, **Reports** → **Activity download** exports a CSV.

### Issuing a refund

Donors sometimes mis-click or change their mind. Refund within PayPal directly:

1. Find the transaction in **Activity** → click it.
2. Click **Issue a refund**.
3. Choose full or partial; PayPal usually returns the original transaction fee to the org as well.
4. Confirm.

The donor sees the refund in their PayPal account within minutes (or back to their card within a few business days).

### Issuing a tax-deductible receipt

PayPal can auto-send receipts that include the org's 501(c)(3) status and EIN — useful for US donors claiming deduction.

1. Sign in as admin → **Account Settings** → **Notifications**.
2. Enable the **Donation receipt** notification (TODO: confirm exact menu name).
3. The receipt template can be customised under **Account Settings** → **Customer notifications** → **Donation receipt template**.

The template should include the legal RLadies+ name, the EIN, the donation date and amount, and the standard "no goods or services were provided" language required by the IRS for full deductibility.

TODO: confirm RLadies+ has this template configured and reviewed by VCorp.

### Sweeping the balance to Relay

PayPal balances should not accumulate — they're not insured the way a bank balance is, and the source of truth for bookkeeping is the Relay account.

1. **Wallet** in the top nav.
2. **Transfer money** → **Transfer to your bank**.
3. Select the linked Relay account.
4. Enter the full available balance (or leave a small buffer if you expect refunds).
5. Confirm.

Standard transfers take 1-3 business days; instant transfers cost a fee and aren't worth it for routine sweeps.
A weekly or bi-weekly cadence works well.

### Adding a team member

1. **Account Settings** → **Account Access** → **Manage Users**.
2. **Add a user** → enter their name and email.
3. Choose the permissions: typical donor-management roles need **View transactions**, **Issue refunds**, and **Download reports**. Admin operations stay with the admin login.
4. They accept the invitation and complete their own login setup.

### Removing a departed team member

1. **Account Settings** → **Account Access** → **Manage Users** → click the departing user.
2. **Remove user**.
3. If they had admin login access, rotate the admin password in 1Password.

## Recovery

Recovery codes for 2FA in 1Password.
PayPal's account recovery falls back to email verification at the role mailbox; Workspace access remains the prerequisite.

If a donor reports a chargeback or PayPal flags a transaction as suspicious, respond through the **Resolution Center** rather than letting it auto-resolve against the org's interest.

## Things to watch for

- **PayPal periodically requests re-verification of 501(c)(3) status.** These notifications land at the role mailbox; acting on them quickly keeps the reduced-fee rate.
- **Funds held on a newly-large transaction.** PayPal's risk system sometimes holds disbursement on an unusually large donation. If you know a large donation is coming (a sponsor commitment, a memorial fund), email PayPal Business Support beforehand to pre-empt the hold.
- **Donor-fee absorption policy.** PayPal lets donors optionally cover the transaction fee. Decide and document whether the donate-page widget asks the donor to do this (recommended for transparency) or whether RLadies+ absorbs the fee silently.
- **Failed sweeps to Relay.** A broken bank link can leave the PayPal balance growing without notice — confirm sweeps actually arrive at Relay in the days after initiating.
- **Currency conversion on non-USD donations.** PayPal auto-converts at its own rate, which is worse than Wise. For donations large enough to matter and where the donor can choose, direct them to the Wise account details instead.
