---
title: "Workspace Admin Console"
linkTitle: "Admin Console"
weight: 10
---

When someone needs a new chapter mailbox, an old organiser's account suspended, or a forgotten password reset, the work happens at [admin.google.com](https://admin.google.com).
Signing in there requires the super-admin credentials for the role mailbox, which live in the shared 1Password vault — see [1Password]({{% relref "/global-team/infrastructure/1password" %}}).

## What's there for our footprint

Workspace's admin console exposes dozens of sections; most of them are dark for us because we don't run the corresponding services.
The handful that matter day-to-day:

- **Directory → Users** — every `@rladies.org` mailbox, including chapter and personal aliases. This is where you provision, suspend, or rename accounts.
- **Directory → Groups** — distribution lists and shared mailboxes. We keep this list short on purpose; check before creating a new one. TODO: enumerate the groups currently in use and what each one is for, so a new admin can recognise them at a glance.
- **Billing → Subscriptions** — confirms we're on the Workspace Nonprofit plan and shows the (zero-cost) subscription tied to the Google for Nonprofits programme.
- **Security → Authentication** — 2-step verification enforcement and recovery options for the super-admin.
- **Account → Admin roles** — who else, if anyone, holds elevated roles beyond the role mailbox super-admin.

If you find yourself wandering through sections like Devices, Apps, or Vault, you're almost certainly off the well-trodden path.
Confirm with the team before changing anything there.

## The role of the shared super-admin mailbox

The role mailbox is the only mailbox with full super-admin privileges, and it is shared through 1Password rather than assigned to a specific person.
That choice is the whole reason the org doesn't have a single-point-of-failure when a volunteer steps back.
Whoever needs admin access opens 1Password, signs in, does the work, and signs out — no transfer of ownership required.

Resist the temptation to grant an individual's personal `@rladies.org` account super-admin rights "just for convenience."
That re-introduces the dependency the role mailbox exists to eliminate.

## Provisioning a new chapter mailbox

This is the most-frequent admin task.
A new chapter onboards, the onboarding team asks the email team to create `cityname@rladies.org`, and the literal click path is:

1. Sign in to [admin.google.com](https://admin.google.com) as the role mailbox using the credentials from 1Password.
2. In the left sidebar, open **Directory → Users**.
3. Click **Add new user** at the top of the user list.
4. Fill in the form:
   - **First name** — the chapter's display name, e.g. `R-Ladies Buenos Aires`.
   - **Last name** — leave blank, or use a single character if the form requires it.
   - **Primary email** — `cityname@rladies.org`, lowercased, no spaces or accents (e.g. `buenosaires@rladies.org`, not `Buenos-Aires@rladies.org`). Match whatever pattern the rest of the directory already uses.
   - **Secondary email** — leave blank.
5. Under **Manage user's password**, leave the default _Generate password_ option selected, and tick **Ask for a password change at the next sign-in**.
6. Under **Send the new account details**, enter the email address of one of the new chapter organisers so the welcome email lands where the chapter can actually read it (the mailbox you're creating obviously can't receive its own welcome email yet).
7. Click **Add new user**.
8. Confirm in the **Directory → Users** list that the new mailbox shows as _Active_.

Record the action somewhere durable — the onboarding GitHub issue is the right place, since it's already tracking the chapter's setup steps.
The organiser-facing walkthrough for what the new chapter does with that welcome email lives at [Chapter Email Access]({{% relref "/organizers/tech/email" %}}); don't duplicate it here.

## Provisioning a personal @rladies.org alias

Since March 2019, personal `name@rladies.org` addresses are issued to Leadership and Global Team members — not to chapter organisers in general.
When someone joins the Global Team and the onboarding flow flags them for an alias, the admin steps are the same as a chapter mailbox with two differences: the address is a person's name, and there's a real human inbox on the other end to receive the welcome email.

1. Sign in as the role mailbox and open **Directory → Users → Add new user**.
2. Fill in the form:
   - **First name** and **Last name** — the person's actual name.
   - **Primary email** — agree the address with them first (`firstname@rladies.org`, `firstname.lastname@rladies.org`, an initialism — whatever they prefer that doesn't collide with an existing address).
   - **Secondary email** — their personal address, so the welcome email goes somewhere they can already read.
3. Leave _Generate password_ selected and tick **Ask for a password change at the next sign-in**.
4. Click **Add new user**.
5. Let them know to look for the welcome email; the first-time login flow walks them through the password change.

If the requester isn't on the Global Team or Leadership, push back before provisioning — the alias inventory stays small on purpose.

## Suspending or deleting a departing account

When a chapter goes dormant or a Global Team member steps down, the account stops being used but the question is which lever to pull.
Three options exist and they aren't interchangeable:

- **Suspend** — the mailbox stops accepting new sign-ins but data is preserved and the address is reserved. Use this for chapters that might come back, or for departures where you're not yet sure whether the inbox holds anything the org still needs. Reversible.
- **Delete** — the account and its data go away. Use this for permanent shutdowns where nothing in the inbox or Drive matters to the org. Not reversible after Google's grace period (usually 20 days).
- **Transfer data, then delete** — Google's delete flow offers to transfer the user's Drive files to another `@rladies.org` account first. Use this when the departing account was a personal alias with Drive content (meeting notes, documents) that needs to outlive the person.

Default to **Suspend** when in doubt.
A suspended account is cheap to keep around and trivial to restore; a deleted one is gone.

The literal click path for suspending:

1. Sign in as the role mailbox and open **Directory → Users**.
2. Find the user (the search box at the top of the list is faster than scrolling).
3. Click the user's name to open their profile.
4. Click **More options** (or the three-dot menu, depending on the current UI) and choose **Suspend user**.
5. Confirm the suspension.

For deletion, use the same path but choose **Delete user**.
Google then prompts you to transfer the user's Drive files, calendar events, and so on; pick a destination account (usually a current Global Team alias) if there's anything worth keeping, or skip the transfer if there isn't.

Record the action — including which option you chose and why — alongside the offboarding or chapter-retirement issue.

## Resetting a forgotten password

Chapter organisers occasionally lose track of their mailbox password.
The reset happens entirely in the admin console:

1. Sign in as the role mailbox and open **Directory → Users**.
2. Find and click the user whose password needs resetting.
3. On their profile, click **Reset password**.
4. Leave _Automatically generate a password_ selected (or set a temporary one yourself), and tick **Ask for a password change at the next sign-in**.
5. Click **Reset**.
6. Google displays the temporary password on screen.
   Copy it.

Send the temporary password to the organiser through a channel _other_ than the mailbox you just reset — Slack DM, their personal email, whichever is already verified for them.
Emailing the temp password to the mailbox itself defeats the entire point.

## Review cadence

Roughly every six months, someone on the Global Team should sign in as the role mailbox and:

1. Open **Account → Admin roles** and confirm the list of admins matches who should currently hold elevated access.
2. Open **Directory → Users** filtered to suspended accounts and clear out any that have been suspended long enough to safely delete.
3. Open **Security → Alert center** to skim any flagged sign-ins or policy issues from the last six months.
4. Open **Billing → Subscriptions** to confirm the Workspace Nonprofit subscription is still active. TODO: confirm whether Google for Nonprofits requires periodic re-verification and where the renewal notice lands; document the renewal flow once the cadence is known.

Six months is a reasonable beat for a volunteer org our size — frequent enough to catch drift, infrequent enough that nobody dreads it.
The Workspace admin console surfaces most of what you need without leaving the page.

## Troubleshooting

**"I can't sign in to the admin console"**
: First, confirm you're using the role mailbox credentials from 1Password — _not_ your personal `@rladies.org` account, which won't have admin rights.
If 1Password itself is the blocker (you don't have access yet, or the vault entry looks stale), reach out to leadership on Slack rather than trying to recover the account through Google's flow.

**"I'm signed in but the menu items above are missing"**
: You're probably signed in with a regular `@rladies.org` account that lacks admin privileges. Sign out, then sign back in as the role mailbox.

**"Google is asking for a recovery code or backup phone"**
: Stop and check 1Password before tapping any of the recovery options.
The recovery numbers and codes for the role mailbox are stored alongside the password.
Triggering a recovery flow without them risks locking the org out of its own admin console.
