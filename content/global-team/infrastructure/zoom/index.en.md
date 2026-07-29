---
title: "Zoom"
linkTitle: "Zoom"
weight: 100
---

Every RLadies+ chapter event that runs online runs on a single shared Zoom account — one account, one set of host licenses, one bill, and a couple of hundred chapters routing their meetups through it.
The organiser-facing flow for booking and running a meeting lives at [Online events with Zoom]({{% relref "/organizers/events/online" %}}); this page is the Global Team side.

## Plan and billing

The paid Zoom plan is funded out of the grant pool that pays for the rest of our shared global operations (currently the R-Consortium and Posit grants) and billed annually to the role mailbox, the same shared mailbox that owns Cloudflare and Google Workspace.
Credentials live in the Shared vault — see [1Password]({{% relref "/global-team/infrastructure/1password" %}}).

TODO: confirm exact plan tier and per-year cost, and the renewal month.

If Zoom billing email arrives for the role mailbox, forward it to the finance contact before clicking anything.
A prompt to upgrade is a conversation, not a click.

## Host licenses

We keep the host-license count small on purpose.
A handful of named volunteers hold licenses; everyone else uses them via the booking flow.
Every additional seat is recurring grant money that does not go to chapters, and our "request a host" model already absorbs that friction without breaking.

TODO: list the current host-license count and who holds the seats, and review alongside the [1Password access audit]({{% relref "/global-team/infrastructure/1password" %}}#things-to-watch-for).

Chapter organisers do not get their own Zoom seat.
They book a slot through Calendly, the booking creates a meeting on a host-license holder's calendar, and the organiser claims host using a rotating host key — full passcode-and-host-key flow at [Online events with Zoom]({{% relref "/organizers/events/online" %}}).
The convention is to use Zoom's _alternative host_ feature rather than sharing the host password: cleaner audit trail, no constant rotation, and a compromised personal device does not compromise the shared account.

### Adding an alternative host to a scheduled meeting

To give a named co-host the ability to start the meeting without the host key:

1. Sign in at [zoom.us](https://zoom.us) as the host-license holder whose calendar owns the booking (not as the role mailbox).
2. Open **Meetings**, click the meeting title, click **Edit**.
3. Scroll to **Options**, expand **Show** if collapsed, find **Alternative Hosts**.
4. Enter the co-host's email; Zoom autocompletes eligible users.
5. Click **Save**.

The alternative host must already have a paid Zoom seat on the same Pro or Business plan — Zoom rejects free or differently-tiered accounts at this step.
In practice that means another host-license holder; chapter organisers without a seat go through the host-key flow.

## Recordings

The recommended path for chapters that want to keep a recording (for the [R-Ladies YouTube channel]({{% relref "/organizers/events/youtube" %}}) or otherwise) is _local_ recording on the host's machine.
Cloud recording is enabled as a fallback but accumulates in the account's storage quota.

TODO: confirm the cloud-recording retention policy in force.
The intent is a fixed 90-day window with explicit preservation for anything the chapter wants kept; without one, recordings accumulate forever and become a privacy liability nobody is curating.

Enforce the window at **Account Management → Account Settings → Recording → Auto delete cloud recordings after days** — toggle on, set 90, save.
Prefer this over a manual sweep through **Recordings → Cloud Recordings**; a setting that runs itself does not lapse when the volunteer curating recordings rotates off.

Speakers and attendees must be told the session is being recorded before recording starts.
Cloud-recording pull requests come through the `#online_meetups` Slack channel; a host-license holder downloads from **Recordings → Cloud Recordings** in the Zoom web portal.

## Integrations

- **Calendly** — chapter-facing booking link.  
  An organiser picks a slot and Calendly creates a Zoom meeting on a host-license holder's calendar.  
  TODO: confirm which Calendly account owns the integration and which host's calendar is the booking target.
- **Meetup** — organisers paste the Calendly-issued Zoom link into the Meetup event page.  
  Do _not_ use Meetup's "connect your Zoom" feature; it swaps the link for one auto-generated from whichever organiser is logged in, breaking the shared-account flow.
  To disable, sign in to [meetup.com](https://www.meetup.com) as the chapter and go to **Settings → Integrations → Zoom → Disconnect**.
  If the chapter keeps it enabled, verify after each event that the join URL is the Calendly-issued one.
- **Google Workspace** — Zoom meeting invites land on the Google Calendar of the host-license holder Calendly is writing to.  
  Nothing further is configured.

## Who has access

The Zoom super-admin login is the role mailbox, same pattern as the rest of our shared services; password and TOTP seed live in the Shared vault in [1Password]({{% relref "/global-team/infrastructure/1password" %}}).
Host-license holders each sign in with their own `@rladies.org` alias (see [Workspace admin console]({{% relref "/global-team/infrastructure/google-workspace" %}})), which Zoom treats as a separate licensed user — so a volunteer rotating off only requires revoking their seat, not rotating the super-admin password.

Rotate the super-admin password when anyone with access to the Shared vault leaves leadership, or any time the credential might have been exposed.

## Operational tasks

The admin actions below all start from [zoom.us](https://zoom.us), signed in as the role mailbox.

### Adding a new host license

1. Sign in and open **Account Management → User Management → Users** from the left sidebar.
2. Click **Add Users** at the top right.
3. Enter the volunteer's `@rladies.org` alias, set **User Type** to _Licensed_, submit.
4. The volunteer accepts the activation link from their `@rladies.org` mailbox.
5. Confirm in the Users list that they show as _Licensed_, not _Basic_.
6. Add them to the rota documentation and the `#online_meetups` Slack channel.

### Transferring a host slot when a volunteer rotates off

1. Open **Account Management → User Management → Users**, find the departing volunteer, click the three-dot menu.
2. Choose **Unlink** (removes them from the account, leaves their personal Zoom data alone) by default; **Delete** only if you need to free the email for re-use.
3. Add the replacement via [Adding a new host license](#adding-a-new-host-license).
4. Update Calendly so bookings route to the replacement's calendar.
5. Verify it took effect: in an incognito window, sign in to [zoom.us](https://zoom.us) as the departing volunteer (or have them check from their own device) — expect "this account is no longer licensed" or a Basic-tier dashboard with no host privileges.
   The Users list alone is not enough; Zoom can leave a stale session active, so an _unlinked_ admin view sometimes sits alongside a user who can still start meetings until that session expires.

### Configuring default meeting settings

Account-wide defaults live at **Account Management → Account Settings → Meeting**.
The current defaults are the ones the organiser guide assumes:

- **Security → Waiting Room** — on
- **Schedule Meeting → Mute all participants when they join a meeting** — on
- **In Meeting (Basic) → Screen sharing → Who can share?** — _Host Only_
- **In Meeting (Basic) → File transfer** — off

Do not loosen these without coordinating with the team that maintains the organiser guide — the anti-trolling advice there depends on them.

### Pulling a participant report after an event

1. Open **Reports → Usage** from the left sidebar.
2. Set the **From** and **To** dates to cover the event, click **Search**.
3. Find the meeting row by topic or meeting ID, click the count in the **Participants** column.
4. Click **Export** to download the CSV of attendees with join/leave timestamps.
5. Share with the requester via Slack DM, not a public channel — attendee lists are personal data.

## Troubleshooting

**The host link does not work for the chapter event.**
Most often the organiser used Meetup's auto-Zoom integration and Meetup swapped the Calendly-issued link.
Confirm they are using the link from the Calendly confirmation email.
If they are and it still fails, the host-license holder whose calendar owns the booking may have moved, deleted, or expired the meeting.

**Mid-event panic mode: the link is dead, the event starts in two minutes.**
Stop debugging and stand up a replacement.

1. Sign in at [zoom.us](https://zoom.us) as a host-license holder (ideally the one whose calendar owned the original booking).
2. Open **Meetings → Personal Room → Start**, copy the join URL from **Participants → Invite → Copy Invite Link**.
3. Paste it into the Meetup event comments, the chapter Slack channel, and anywhere else the event was announced.
4. Post in the Global Team `#help` Slack channel so someone tracks down the underlying issue.

**The recording was not saved.**
If local recording was used, the file lives on the organiser's machine and nothing on the Zoom side can recover it.
If cloud recording was used, check **Recordings → Cloud Recordings** with the date range set to the event; processing sometimes takes an hour.
If it is past the retention window, it is gone.

**Zoom is asking for billing.**
Do not enter a card.
Forward to the finance contact, check **Account Management → Billing** to see what Zoom thinks the problem is, and resolve through the finance route — usually the grant-funded payment method needs updating.

## Things to watch for

- **License utilisation.** Once a quarter, check **Account Management → Reports → Usage Reports**. Consistently low means we are over-provisioned; Calendly turning chapters away means under-provisioned. Either signal is worth raising before renewal.
- **Annual renewal.** Calendar reminder a month ahead of the renewal date (TODO: fill in) so the finance conversation happens before the auto-charge.
- **Security advisories.** Zoom's [trust centre](https://www.zoom.com/en/trust/) publishes security bulletins; when something high-severity lands, check whether it affects the meeting settings we rely on.
- **Audit cadence.** Walk the Users list every six months alongside the [1Password audit]({{% relref "/global-team/infrastructure/1password" %}}#things-to-watch-for) — anyone Licensed who is no longer on the rota should be unlinked.
