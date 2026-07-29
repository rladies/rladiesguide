---
title: "Slack workspaces"
linkTitle: "Slack admin"
weight: 75
---

RLadies+ runs two Slack workspaces — one for chapter organisers and Global Team, one for the broader community — and the admin paths are identical at the Slack level even though the membership flows differ.
This page covers both: plans, admin credentials, invite paths, and click trails for the operational tasks that come up often enough to write down.

## The two workspaces

The organisers workspace lives at [rladies.slack.com](https://rladies.slack.com) and is for chapter organisers and the Global Team.
New organisers get an invite as part of the [organiser onboarding flow]({{% relref "/global-team/onboarding" %}}) — a step in that runbook, not a self-serve form.
Conversations here lean operational: cross-chapter coordination, Global Team work, organiser support, and the cross-org programme channels (`#new_chapters`, `#posit-cloud`, and friends).

The community workspace is the broader space for anyone in the RLadies+ community who identifies as a woman or gender minority and works with R.
TODO: confirm the exact workspace URL.
Members arrive through the public sign-up at [rladies.org/form/community-slack](https://rladies.org/form/community-slack/) rather than through onboarding.
The form lands in an Airtable base that posts to a Cloudflare Worker, which posts an Invite button into the organisers workspace for a human to click — see [Jinx Airtable invites]({{% relref "/global-team/jinx/airtable-invites" %}}) and the [Community Slack Invites base]({{% relref "/global-team/airtable/community-slack" %}}).

Running two workspaces rather than one with locked channels is a deliberate trust-boundary choice: the organiser space can be candid about incidents, sponsors, and organiser support without exposing any of it to a fully-open community space.

## Plans

Both workspaces are on Slack Free.
TODO: confirm this is still true for both, particularly the community workspace.
Free means a 90-day visibility window on messages and file uploads; older content is hidden, not deleted, and Slack restores it on upgrade.
Channels, members, and integrations are unlimited on Free, which is what actually matters day to day.

Slack also runs a [Pro for Nonprofits](https://slack.com/help/articles/204368833) discount that would unlock full message history, voice huddles, and unlimited integrations.
Whether to pursue it is an open question, shaped like the [GitHub Team vs Free for OSS decision]({{% relref "/global-team/infrastructure/github" %}}#open-decision-stay-on-team-or-apply-for-free-for-oss): full history is genuinely useful for organisers looking up a year-old thread, but the cost is an application, an eligibility review, and an administrative relationship the all-volunteer Global Team has to keep current.
TODO: confirm whether anyone has previously evaluated Pro for Nonprofits.

## Who has admin access

The convention is leadership-as-workspace-owners, mirroring [1Password]({{% relref "/global-team/infrastructure/1password" %}}) and Google Workspace.
At least two owners per workspace, always — Slack requires a single Primary Owner, and a single anything is a single point of failure waiting to surface at the worst moment.

Each workspace keeps its own Owner list.
Being a Workspace Owner on the organisers workspace does not grant any admin role on the community workspace, and the reverse is also true.
Promoting someone to admin on one workspace is two distinct actions if they need the role on both.

The admin login for each workspace is tied to the shared the role mailbox mailbox.
Credentials, recovery codes, and TOTP seeds live in [1Password]({{% relref "/global-team/infrastructure/1password" %}}).
When a leadership admin needs the elevated role they sign in with their own Slack account first; we do not share Slack passwords person-to-person.

Joining the community workspace as an admin is a two-step path.
A new leadership member first joins as a regular member through the Airtable invite flow described below, and an existing community Slack Workspace Owner then promotes them via **Manage members → ⋯ → Change account type**.
Without the promotion step they are just another member with no admin surface.

TODO: enumerate the current Workspace Owner and Workspace Admin lists for both workspaces.

## How members get invited

For the organisers workspace, an invite goes out as part of [onboarding]({{% relref "/global-team/onboarding" %}}).
The onboarding team member opens [rladies.slack.com/admin/invites](https://rladies.slack.com/admin/invites) using the credentials from [1Password]({{% relref "/global-team/infrastructure/1password" %}}), pastes the new organiser's email, and sends.

For the community workspace the path is the Airtable-driven flow.
A member fills the [sign-up form](https://rladies.org/form/community-slack/), the submission lands in the Community Slack Invites Airtable base, Airtable POSTs to the Cloudflare Worker, and the worker posts an Invite button into `#new-invitee` on the organisers workspace.
An organiser skims chapter and email, clicks Invite, and the worker calls `admin.users.invite` and marks the record processed.
The two-step "form → button → human click" exists because auto-inviting from a public form would be a spam vector — see [the Jinx airtable-invites page]({{% relref "/global-team/jinx/airtable-invites" %}}).

## Channel conventions

Chapter channels follow `#rladies-cityname` in both workspaces (lowercase, hyphenated).
Topic channels use whatever separator they were first named with — `#new_chapters`, `#posit-cloud`, `#how_to_slack`, `#organizers`, `#random`, `#team-community_slack`, `#new-invitee`.
The only firm rule is that chapter channels start with `#rladies-`.

Who can create channels is a workspace setting, not a Slack default.
The path is [rladies.slack.com/admin/settings](https://rladies.slack.com/admin/settings) → **Permissions → Channel Management → Set permissions**.
On Free the default is **Everyone except guests**, which is what both workspaces currently run.

That default trades channel sprawl for ease of community organising — a new chapter or topic channel does not need a ticket — and the cost is a longer channel list with occasional duplicates.
The [community Slack guidelines]({{% relref "/community/slack" %}}) and the [chapter Slack guide]({{% relref "/organizers/online-presence/slack" %}}) nudge organisers to combine or archive quiet channels, which is the lighter-weight intervention than locking the permission down.

TODO: enumerate the current admin-only or restricted channels for both workspaces.

## Installed apps

[Jinx]({{% relref "/global-team/jinx" %}}) is the major installed app, present in both workspaces.
It handles `/jinx` slash commands, DMs and @-mentions, the welcome message on team-join, and the Airtable invite webhook flow.
The full picture — Cloudflare Worker, GitHub Actions dispatch, secret inventory — is in [Jinx for developers]({{% relref "/global-team/jinx/for-developers" %}}).

TODO: enumerate the other installed apps in each workspace.
The canonical list lives at [rladies.slack.com/apps/manage](https://rladies.slack.com/apps/manage) under **Installed Apps**; the review path is in the operational tasks below.

## Operational tasks

Click paths for the things that come up often enough to write down.
All start from the workspace at [rladies.slack.com](https://rladies.slack.com) (or the community equivalent), signed in as a workspace owner or admin.

**Promote someone to admin or owner.**
Click the workspace name in the top-left, choose **Settings & administration → Manage members**, find the person, click the **⋯** menu, and pick **Change account type → Workspace Admin** (or Owner).
Only existing owners can promote to owner.

**Deactivate a departed member.**
**Manage members → ⋯ → Deactivate account**.
Reversible; preserves history; removes them from channels.
Do this as part of offboarding so the member list does not drift.

**Rename or archive an old channel.**
Open the channel, click its name in the header, go to the **Settings** tab, and pick **Rename channel** or **Archive channel** at the bottom.

**Review installed apps and their permissions.**
Open [rladies.slack.com/apps/manage](https://rladies.slack.com/apps/manage) and switch to **Installed Apps**.
Click any app to open its detail page, then the **Permissions** tab — that is where the actual scope of access lives, not the listing page.
A Workspace Owner can revoke the app entirely from the button at the bottom of the same page.
Apps installed once and forgotten are a quiet risk surface; a six-monthly skim is enough to keep the list honest.

**Export workspace data.**
The path is [rladies.slack.com/services/export](https://rladies.slack.com/services/export): pick a date range, request the export, and Slack emails a download link when the archive is ready.
On Free this only covers public-channel messages and metadata — private channels and DMs are not included and there is no option to request them.
On Pro a Workspace Owner can request a full export (often called Standard or Corporate Export) that includes DMs and private channels; this is gated behind a multi-day delay and Slack's review, not a checkbox.
Treat any export zip as sensitive and delete it when you are done with it.

**Configure message retention.**
The path is [rladies.slack.com/admin/settings](https://rladies.slack.com/admin/settings) → **Message Retention & Deletion**.
On Free this is a single workspace-wide policy with no per-channel or per-DM overrides; Pro unlocks both.
The Free-plan 90-day visibility window is separate — visibility hides older messages, retention set here deletes them.
TODO: confirm the current setting for each workspace.

## Recovery and security

Both workspaces should have 2FA enforced for admins at minimum, ideally for all members.
The path is [rladies.slack.com/admin/settings](https://rladies.slack.com/admin/settings) → **Authentication → Two-Factor Authentication**, and the choice is between **Required for all members** and **Required for admins only**.
The setting applies immediately; people without 2FA are prompted to set it up on their next sign-in.

Slack supports SMS and authenticator-app 2FA as separate options.
Authenticator apps (1Password, Authy, Google Authenticator) are meaningfully safer than SMS — SIM swap attacks are the standard way SMS 2FA fails — and should be the recommendation for any admin enrolling for the first time.
For the shared the role mailbox admin login the TOTP seed lives in [1Password]({{% relref "/global-team/infrastructure/1password" %}}) alongside the password.

If the admin login itself is the problem — the role mailbox Slack password no longer works, the TOTP code is rejected — [1Password]({{% relref "/global-team/infrastructure/1password" %}}) is the source of truth, including the recovery codes saved alongside the password.
If 1Password itself is the blocker, reach out to another leadership admin rather than opening a Slack support ticket cold.

The harder failure mode is the workspace having no reachable Owner at all.
If the only Workspace Owner has left or is unreachable, any remaining Workspace Admin can be promoted to Owner — Slack support can perform that promotion when no one inside the workspace can.
If there is no Workspace Admin either, Slack support is the only path back, and they will verify control of the workspace by emailing the registered admin address.
That address is the role mailbox, so the recovery only works as long as Google Workspace access to that mailbox is also intact; lose both at once and the workspace is effectively orphaned.
The prevention is the two-Owner convention above, applied to both workspaces and re-checked at every leadership rotation rather than assumed to still hold.

For suspicious activity — an unexpected admin promotion, an app no one remembers installing, a member added through a channel nobody recognises — the audit log lives at [rladies.slack.com/admin/audit_logs](https://rladies.slack.com/admin/audit_logs) and is visible to Workspace Owners only.
Slack writes one regardless of plan; on Free it only retains the last 90 days, so anything older than that is gone and a quiet incident can age out before it is noticed.
If activity looks unauthorised, deactivate the actor first, then read the log to understand the blast radius before reversing anything.

## Troubleshooting

_"I never got my invite."_
For organisers: check the chapter's onboarding issue and resend from **Manage members → Invitations**.
For community sign-ups: open the Community Slack Invites Airtable base, find the record by email, and confirm the Invite button was clicked in `#new-invitee`; if not, click it.
The invite email is often filtered to spam — ask the person to check there before resending.

_"Jinx isn't responding."_
The Cloudflare Worker is the first place to check; see [Jinx troubleshooting]({{% relref "/global-team/jinx/troubleshooting" %}}) and [the Cloudflare runbook]({{% relref "/global-team/infrastructure/cloudflare" %}}) for accessing the Worker logs.

_"Older messages have disappeared."_
The Free-plan 90-day visibility window is the most likely cause; messages are hidden rather than deleted, and upgrading restores them.

## Things to watch for

Admin lists drift faster than anyone expects.
A leadership rotation that does not include a Slack admin review leaves the previous cohort with full workspace ownership; bake the Manage members walk into the offboarding checklist alongside [1Password]({{% relref "/global-team/infrastructure/1password" %}}) and Google Workspace.

Deactivated members linger in channels until someone removes them by hand — mostly cosmetic, but it muddies later audits.
Apps installed once and forgotten accumulate the same way; a six-monthly skim of **Installed Apps** catches them.

Slack periodically nudges the workspace to upgrade through banners and emails to the owner address.
Treat those as conversations to forward to leadership, not buttons to click on instinct.
Staying on Free is a deliberate choice; moving off it should be too.
