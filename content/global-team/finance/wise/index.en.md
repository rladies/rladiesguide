---
title: "Wise (international transfers)"
linkTitle: "Wise"
weight: 20
---

When RLadies+ pays a speaker in Europe, reimburses a Latin American chapter for venue costs, or receives a donation in EUR or GBP, Wise is the rail.
A traditional international wire would charge a flat fee plus an opaque FX margin on top; Wise quotes the actual mid-market rate and a small explicit fee, which over a year of cross-border transfers adds up to real money.

## What we use Wise for

- **Outgoing payments to non-US recipients** — speaker honoraria, chapter reimbursements, contractor payments.
- **Receiving international donations** — Wise issues RLadies+ a local account number in major currencies (USD, EUR, GBP, AUD, others), so a donor in Germany can send a SEPA transfer rather than navigate an international wire.
- **Holding multi-currency reserves** — useful when there's a known upcoming payment in a particular currency and the rate is favourable today.

For USD-to-USD payments, use [Relay]({{% relref "../relay" %}}) — it's the operating account, and keeping USD flow in one place makes reconciliation simpler.

## Plan

TODO: confirm whether RLadies+ is on Wise Business standard or a tier with negotiated nonprofit rates.
Wise has periodically offered programmes for registered nonprofits; worth checking at renewal.

## Who has access

Admin tied to the role mailbox, credentials in the [1Password]({{% relref "/global-team/infrastructure/1password" %}}) Shared vault.
Additional leadership members can be added as users with their own logins and per-user permissions.

TODO: list the current Wise user roster.

## Operational tasks

### Signing in

1. Open [wise.com/login](https://wise.com/login).
2. Sign in using your individual Wise login (or the admin login from 1Password if you specifically need admin operations).
3. Complete the 2FA challenge — Wise supports SMS, app-based TOTP, and security key; the authenticator app is the recommended path.

### Sending an international payment

1. Click **Send** in the top nav.
2. Choose **Send money**.
3. Enter the recipient — either select an existing recipient or click **Add a new recipient** and enter their name, country, currency, and banking details.
4. Enter the amount in either the source or destination currency; Wise shows the FX rate and fee before you commit.
5. Add a reference the recipient will see (e.g., "RLadies+ speaker honorarium — useR! 2026").
6. Review and **Confirm**.
7. If funding from a Wise balance, the transfer completes within minutes for major currencies; if funding from a linked bank, allow 1-3 business days.

### Adding a recipient

1. **Recipients** in the top nav → **+ Add recipient**.
2. Enter their email so Wise can collect their own bank details directly (cleaner than typing them yourself and getting an IBAN digit wrong).
3. Alternatively enter the details directly: name, country, account type, IBAN or local account/routing number.
4. Save the recipient for re-use.

### Receiving international donations

1. **Balances** → click the currency you want to receive in (or add a new currency from the **+** button).
2. Click **Get account details**.
3. Share the displayed account number, routing/sort code, and (where applicable) IBAN/SWIFT with the donor.

These details are donor-facing and can also be added to the website's donation page for the currencies we most commonly receive in.

### Pulling a statement for bookkeeping

1. Click **Activity** in the top nav.
2. Set the date range.
3. Click **Export statement** → choose CSV or PDF.
4. Upload to the finance folder in the Shared Drive alongside the Relay export.

### Sweeping balances to Relay

International donations and any leftover reserves should be swept to Relay periodically so the USD operating account reflects the org's actual funds.

1. **Balances** → click the currency to sweep.
2. **Convert** to USD (Wise shows the live rate; check it's reasonable for the amount).
3. Once converted into the USD balance, **Send** to the Relay account using the RLadies+ Relay routing and account number (stored as a Wise recipient).

Monthly is a sensible cadence; more frequent if balances are growing quickly.

## Recovery

2FA recovery codes saved in 1Password are the first option.
Wise's account recovery flow falls back to the role mailbox for email verification — Google Workspace access remains the prerequisite.

## Things to watch for

- **Balances accumulating in currencies you don't actively need.** Sweep them to Relay rather than holding them — FX rate movement between today and the eventual sweep is unbookable risk.
- **KYC re-verification requests.** Wise periodically asks for updated nonprofit documentation; these emails land at the role mailbox. Ignoring them eventually freezes the account.
- **Recipient saved with stale details.** A speaker who changed banks last year will still be saved with the old account — verify with the recipient before sending a non-trivial amount.
- **FX rate volatility on scheduled payments.** If you schedule a transfer for next week instead of today, the rate at execution will differ. For small transfers it doesn't matter; for a $5,000 honorarium it can.
