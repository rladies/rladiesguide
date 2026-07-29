---
title: "Calendly"
linkTitle: "Calendly"
weight: 110
---

When a chapter organiser books a slot in the shared Zoom account, the link they used is a Calendly event type sitting under a single RLadies+ account.
That same account backs every other bookable session the Global Team runs — mentoring, office hours, interviews — and is one of the line items the grant pool pays for under "global operations".

## Plan and billing

The Calendly account runs on a paid tier; TODO: confirm whether we are on Standard or Teams and what the renewal cadence is.
Billing goes to the card associated with the role mailbox, the same payment surface used for the other paid services in this section.
Invoices land in that mailbox; forward them to the Global Team finance contact rather than acting on them inside Calendly's billing console.

## How the account is structured

Calendly's model is one account, one billing relationship, and a set of "members" underneath it — each owning their own event types and availability.
We use per-member seats rather than a single shared booking link: a shared link forces every bookable hour through whoever happens to be on duty, calendars collide, and the audit trail blurs ("who actually took that mentoring call last March?").
Per-member seats give each Global Team member their own slots without conflict, and bookings show up on their own calendar with their name on them.
The tradeoff is a slightly higher seat cost, which for a team of our size has been the right trade.

TODO: enumerate current member seats — names, roles, and which event types each one owns.

## Event types

The event-type inventory is the thing that changes most often, so this section is the one to keep current.
At time of writing the configured event types include the shared Zoom booking link used from the `#online_meetups` channel in the [R-Ladies Organizer Slack](/comm/slack/) (referenced in the [organiser online events page](/organizers/events/online/)).

TODO: list every event type currently configured, who owns it, what its duration is, and where the link is shared.
Likely candidates beyond the Zoom slot: mentoring sessions linked from the [chapter mentoring]({{% relref "/global-team/mentoring" %}}) signup, leadership office hours, prospective-organiser interviews.

## Integrations

Three integrations matter; the rest can be ignored.

**Zoom** is mandatory for any event type that should auto-attach a video link to the confirmation email.
OAuth it from **Integrations → Zoom → Connect**, signed in as the role mailbox so the connection is tied to the shared mailbox rather than any one member.
TODO: cross-link to a dedicated Zoom infrastructure page once one exists; relevant credentials live in [1Password]({{% relref "/global-team/infrastructure/1password" %}}) under the Shared vault.

**Google Workspace Calendar** is mandatory for invitee-side auto-add and for host-side conflict detection.
This one is per-member: every Calendly member connects their own Google account under **Account → Calendar Connection**, using the Workspace identity they actually use day-to-day.
A booking will not write to a member's calendar until that member has connected it.

After the role mailbox Workspace password is rotated (see the [Google Workspace runbook]({{% relref "/global-team/infrastructure/google-workspace" %}})), every member's Calendar OAuth grant can drop silently — bookings keep coming in but stop landing on the host's calendar.
Each affected member reconnects from their own seat: **app.calendly.com → Account → Integrations → Google Calendar → Connect**.
This is per-member, not org-wide; an admin cannot do it on someone else's behalf.

**Slack notifications** push booking events into a Global Team channel.
TODO: confirm whether this is currently enabled, which channel it posts to, and whether it uses the Calendly Slack app or a webhook.

**Webhooks.** If a custom RLadies+ workflow consumes booking events directly, it is wired up at **app.calendly.com → Integrations → Webhooks → New webhook**.
TODO: confirm whether Calendly fires webhooks for any RLadies+ workflow today; if not, leave this section as the pointer for whoever wires the first one.

## Who has access

The Calendly super-admin login is the role mailbox; password and TOTP seed live in [1Password]({{% relref "/global-team/infrastructure/1password" %}}) under the Shared vault.

Each Global Team member who needs a bookable link gets their own seat, signed up with their `@rladies.org` address, and authenticates as themselves rather than via the shared login.
The shared the role mailbox login is only used for admin work: adding and removing members, billing, account-wide settings.

TODO: list current members with active bookable links.

When a Global Team member rotates off, suspend their seat rather than letting it linger.
A dormant member with a still-public booking link is a quiet way for someone outside the team to land on a calendar that nobody is watching.

## Operational tasks

### Adding a new team member with their own booking link

1. Log in at `https://calendly.com/` as the role mailbox using the credentials from [1Password]({{% relref "/global-team/infrastructure/1password" %}})
2. Click your avatar in the top right, then **Admin Center**
3. In the left-hand nav, click **Users**, then **Add Users**
4. Enter the new member's `@rladies.org` email and assign them to the appropriate group (TODO: confirm group structure)
5. Choose the seat type — a full member seat is required for them to create their own event types
6. Click **Send Invitation**; the member completes setup by clicking the link in the invitation email, choosing a password, and connecting their Google Calendar
7. Confirm with them that their personal Calendly page (`calendly.com/<their-handle>`) loads and that their calendar connection shows as active under **Account → Calendar Connection**

### Removing a departed team member

Suspending a user freezes their bookings but keeps the seat; deleting releases the seat and removes their event types.
The order matters:

1. From **Admin Center → Users**, find the departing member and click the **⋯ menu → Suspend user** — this immediately stops new bookings against their event types and prevents sign-in
2. Reassign any event type that should outlive the member to another member, or recreate it under the shared account
3. Sweep their pending bookings: from **Scheduled Events**, filter by host = the departing member, and for every upcoming booking either reassign it to another member or cancel with a note to the invitee — removing the user does not auto-cancel what is already on someone else's calendar
4. Return to the user record and choose **⋯ menu → Remove** (the same menu also offers **Delete user**) to free the seat

Delete before reassigning and the event types go with the user — the booking links 404.

### Creating or modifying a shared event type

For an event type that should live under the shared account rather than one member's seat — the chapter Zoom booking link is the canonical example — work as the role mailbox:

1. From the Calendly home page, click **Event Types** in the top nav
2. Click **+ New Event Type** and choose **One-on-One** for a single-host booking, or **Collective** if multiple Global Team members need to be on the call
3. Set the name, duration, and the URL slug — the slug is the bit that ends up in the public booking link, so pick something the team will recognise
4. Under **Availability**, set the days and times the slot should be bookable; for the chapter Zoom booking we leave this open 24/7
5. Under **Event Link Options → Location**, pick **Zoom** so the confirmation email carries an auto-generated meeting link
6. Under **Confirmation Page** and **Email and Workflow Notifications**, set the confirmation message — see "Confirmation message hygiene" below
7. Save, then test the public link in an incognito window before sharing it

To modify an existing event type, click it from the **Event Types** list and edit in place; changes apply to all future bookings but do not retroactively change confirmations already sent.

### Connecting a member's calendar

Per-user, done by the member themselves the first time they sign in:

1. Sign in at `https://calendly.com/` with their `@rladies.org` address
2. Click avatar → **Account Settings → Calendar Connection**
3. Click **Connect Calendar**, choose **Google**, and complete the OAuth flow against their Workspace identity
4. Confirm both checkboxes: check the calendar for conflicts _and_ add events to the calendar

If this step is skipped, bookings still send confirmations but never touch the host's calendar, which is how double-bookings happen.

### Pulling a bookings report

From the Calendly home page, click **Scheduled Events** in the top nav, choose the date range, and use the **Export** button to download a CSV.
For an account-wide view across all members, do the same thing from **Admin Center → Activity** instead.

## Best practices to apply

**Confirmation message hygiene.**
The confirmation email lives at **app.calendly.com → Event Types → click the event type → Notifications and Workflows → Confirmation email → Edit**.
Every event type's confirmation should carry a clear cancellation and reschedule policy plus a link to the [RLadies+ Code of Conduct](https://rladies.org/coc/) — that link is not optional, it goes in every confirmation.
The confirmation is the most likely thing the invitee will read about the booking; setting expectations there is cheaper than enforcing them after.

**Rotate OAuth grants when `accounts@` rotates.**
A password rotation on the role mailbox leaves the Calendly account signed in but can drop anything OAuth'd from that mailbox — most visibly Zoom on shared event types.
After every rotation, sweep **Integrations → Zoom** on the shared account and reconnect if disconnected, and remind every member to re-verify their own Google Calendar connection.

## Troubleshooting

**"Bookings aren't creating Zoom links."**
The Zoom integration on the relevant event type has dropped.
Sign in as the role mailbox, go to **Integrations → Zoom**, and reconnect; then on the event type itself, confirm **Event Link Options → Location** is still set to **Zoom** (sometimes it falls back to a generic location after a reconnect).

**"The booking confirmation email looks wrong."**
Confirmation copy lives on the event type, not the account.
Edit at **Event Types → click the event type → Notifications and Workflows → Confirmation email → Edit**.
Existing confirmations are not regenerated, so the fix only applies to future bookings.

**"Double-booked time slot."**
The root cause is almost always a host whose Google Calendar is not connected or has lost its OAuth grant — Calendly could not see the conflict at booking time.
First, reconnect: the host walks through **Account Settings → Calendar Connection**.
Then recover the already-booked slot rather than leaving the invitee stranded.
Open **Scheduled Events**, click the conflicting booking, choose **Cancel → "I have a conflict"**, write a short personal note explaining the clash, and use the reschedule link included in the cancellation email to offer alternative times.

**"Calendar integration broke after a password change."**
Expected, see "Rotate OAuth grants when `accounts@` rotates" above.
Reconnect from **Account Settings → Calendar Connection** (per-member) or **Integrations → Zoom** (account-wide) and the bookings resume immediately.

## Things to watch for

Integrations break silently — the first signal is usually a confused invitee on the other end, not a Calendly alert.
Skim **Integrations** on both the shared account and each member seat once a quarter, and always after an the role mailbox password rotation.

A per-member-seat plan has a fixed seat count, and adding a new Global Team member when the count is full means a billing change rather than a free invite.
TODO: record the current seat count and the renewal date so the team knows when to expect either invoice or renewal pressure.
