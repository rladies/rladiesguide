---
title: "1Password"
linkTitle: "1Password"
weight: 90
---

Every shared credential the RLadies+ Global Team uses lives in one 1Password account, and every member of leadership is an admin on it.
That is the whole point: when someone steps back or goes on holiday, nothing critical leaves with them.

## The plan we are on

RLadies+ runs on the [1Password Teams Open Source](https://github.com/1Password/1password-teams-open-source) plan, which 1Password gives free of charge to qualifying open source and nonprofit projects.
Our application has been approved and is now permanent — there is no annual reapply step at present.

The OSS plan is the right fit for a small volunteer-run nonprofit for two reasons.
The first is practical: per-seat pricing on the commercial plans would burn through donation income that should be paying for the services we actually run.
The second is honest alignment: RLadies+ _is_ an open source community organisation, which is exactly the kind of project the plan exists to support.
The social contract is light — we run an active public project under the rladies GitHub organisation, and that continues to be true.

## Who has access

A leadership member acts as the current primary account holder; the named contact is recorded in 1Password's own account-holder settings rather than enumerated in this guide.
Beyond that, _all of leadership has admin access_ on the account.
That is deliberate.
A password manager with one administrator is a single point of failure dressed up as a security control — if the primary holder is unreachable for any reason, any other leadership admin can provision or revoke access without waiting.

The convention of multiple admins is standard practice for any team vault; we just apply it strictly.
The trade-off is that anyone with admin rights can also do damage, so admin status is scoped to leadership and reviewed when leadership changes.

## What is stored in it

The vault holds credentials for the systems the Global Team operates on behalf of the organisation as a whole.
Categories, not specifics:

- Login for the role mailbox, the shared mailbox that is the admin login for Cloudflare, Google Workspace, and several other services that only allow a single owner account
- The shared Airtable account used to administer the [Global Team Airtable bases]({{% relref "/global-team/airtable" %}})
- Social media credentials for the org-level accounts managed by the [social media team]({{% relref "/global-team/social-media-management" %}})
- API tokens and service credentials that do not live in GitHub Actions secrets — for example, the Airtable personal access tokens documented on the [Airtable API page]({{% relref "/global-team/airtable/api" %}})

Specific passwords, tokens, and recovery codes are not reproduced anywhere in this guide.
The vault is the source of truth; this page is a map to it.

Per-person logins beat shared logins wherever the underlying service supports them.
GitHub is the clearest example: every Global Team member has their own GitHub account and joins the `rladies` org as themselves, rather than logging in as a shared identity.
We only fall back to a shared credential when the service forces us to — the role mailbox exists because Cloudflare and Google Workspace tie ownership to a single mailbox, not because we wanted a shared login.

## Vaults

- **Shared** — the main vault, referenced from the [Airtable API guide]({{% relref "/global-team/airtable/api" %}}) and other Global Team pages.
  Contains the categories listed above.

TODO: list current vaults and what each contains.
At time of writing the "Shared" vault is the one consistently documented elsewhere; any additional vaults (per-team, per-project) should be added here once enumerated.

## The admin console

The admin console lives at `https://my.1password.com/` once signed in to the RLadies+ account.
TODO: confirm the team slug — the full team URL is of the form `https://my.1password.com/teams/<team-slug>/`.

The sections an admin uses regularly are **People** (invite, suspend, remove members; revoke device sessions), **Vaults** (create vaults, manage per-vault access), and **Groups** (bundles of people for shared permissions; we keep this simple and grant directly).
Account-wide settings — billing, recovery policy, primary account holder — live under the gear icon at the top right.

## Provisioning a new admin

When a new Global Team member needs access:

1. In the admin console, go to **People → Invite people**, enter their email, and assign them to the **Administrators** group
2. In **Vaults**, open each vault they need (usually "Shared" plus any team-specific vault) and grant access with the appropriate permissions
3. After they complete their first sign-in (next section), confirm with them that the expected vaults appear

Offboarding reverses the same path: suspend the member in **People**, confirm they no longer appear under each vault, then remove them.
This is one of the items on the [offboarding checklist]({{% relref "/global-team/onboarding" %}}).

## What a new admin actually sees

A new admin's first sign-in generates two secrets that cannot be recovered later, so it is worth walking through:

1. An invitation email arrives from 1Password with an activation link — click it
2. Choose a **master password** (long, memorable, not reused) on the setup page and confirm it
3. 1Password generates a **Secret Key** and offers to download an **Emergency Kit** PDF containing it; save the PDF somewhere safe and offline _before_ continuing (see Recovery below)
4. The browser app signs in and shows the vaults the inviting admin assigned
5. Install the desktop and mobile apps from `https://1password.com/downloads/` and sign in using the email, Secret Key, and master password from the Emergency Kit

The account still works if the Emergency Kit step is skipped — right up until the next sign-in from another device.
The inviting admin should confirm the kit has been saved before treating onboarding as complete.

## Getting a credential out

Day-to-day usage, for the first-task case:

1. Open the desktop or mobile app (or `https://my.1password.com/`) and unlock with the master password
2. Switch to the **Shared** vault from the vault selector at the top of the sidebar — Shared is not the default if a personal vault is also on the account
3. Search by service name, for example the role mailbox or `Cloudflare`
4. Use the copy icon next to the password or token field; the clipboard clears itself after about 90 seconds

The browser extension does the same thing inline on a login page once installed and signed in.

## Recovery

1Password authentication uses a master password _and_ a Secret Key.
Both are required to unlock the account from a new device.
If an admin loses either one and has no Emergency Kit saved, the account cannot be recovered — not by 1Password support, not by another admin.
The design is end-to-end encrypted on purpose.

Every admin should save their Emergency Kit offline: printed and kept in a physical safe, or stored in another encrypted vault that is _not_ the same 1Password account.
This is the one operational habit that genuinely matters for this system.

**Lost or stolen device.** Another admin opens the affected member's entry in **People**, finds the device under their session list, and chooses **Deauthorise**.
The member then signs in on a replacement using email, master password, and Secret Key from their Emergency Kit.
No need to suspend the whole account unless the master password is also at risk.

**Forgotten master password.** If the admin is still signed in on at least one device, they change it under **Settings → Security → Change master password**.
If not, and no Emergency Kit exists, another admin removes them from **People** and re-invites them with fresh credentials.
Anything they alone stored in a personal vault on the account is lost — one more reason shared credentials belong in the Shared vault.

**Suspected compromise of a vault item.** Order matters: rotate the credential _at the source service_ first (change the password, regenerate the token, revoke the leaked one), then update the Shared vault entry with the new value and a dated note explaining the rotation, then update any other copies (GitHub Actions secrets, Netlify environment variables).
Source-first closes the window where the old credential still works.

**Transferring the primary account holder.** The primary account holder is a single-person role with billing and account-deletion rights.
The transfer happens under **Settings → Account → Primary Owner**: the current holder selects another existing admin and confirms.
The new holder should verify they can see the billing and account-recovery sections that were previously hidden.
TODO: cross-link to a leadership handover checklist once one exists.

## How this differs from chapter security

The [chapter organiser security guide]({{% relref "/organizers/tech/security" %}}) recommends that individual organisers use a _personal_ password manager — KeePass, Bitwarden, 1Password, anything — for chapter credentials and their own accounts.
That guidance still stands and is independent of this page, which is specifically about the _Global Team_ shared vault.
Chapter credentials should not be put into the Shared vault, and Global Team credentials should not be left in any one organiser's personal vault.

## Things to watch for

- **Audit cadence.** Once every six months or so, a leadership admin should walk the member list: who is still active, who has stepped back, what entries are stale
- **Departing admins.** When anyone with admin access leaves leadership, remove them from the account _and_ rotate any credentials they had unique knowledge of, including the role mailbox password
- **The Emergency Kit habit.** New admins occasionally skip the Emergency Kit step because the account works fine without it — right up until it does not.
  The inviting admin should confirm it has been saved
