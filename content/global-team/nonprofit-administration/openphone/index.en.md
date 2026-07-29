---
title: "OpenPhone (shared US phone number)"
linkTitle: "OpenPhone"
weight: 20
---

A donor calling the number on RLadies+'s 501(c)(3) paperwork, an event venue confirming a booking, a sponsor following up on an email — they all dial one US number.
The call lands in a shared OpenPhone inbox that any leadership member can pick up, from their laptop or their phone, with no need to give out anyone's personal number.

## What OpenPhone does

OpenPhone ([openphone.com](https://openphone.com)) is a cloud phone service designed for teams.
The RLadies+ setup uses it for:

- **One shared US phone number** — the official RLadies+ contact number, printed on tax documents, the website's contact page, and grant applications. TODO: confirm the actual number (it's in 1Password and on the website; decide whether to print it here too).
- **Voicemail with transcription** — voicemails arrive in the inbox with a text transcript, so a quick scan tells you whether to listen or not.
- **Shared team inbox for SMS** — texts to the number land in a thread any team member can reply to.
- **Desktop and mobile apps** — leadership can pick up calls from anywhere; outgoing calls show the RLadies+ number as caller ID rather than the personal number.

## Plan

TODO: confirm whether RLadies+ is on OpenPhone's Starter or Business tier, and whether the nonprofit pricing programme applies.
Starter covers a single number; Business adds analytics, scheduled messages, and integration with other tools.

## Who has access

- Admin tied to the shared leadership role mailbox; credentials in the [1Password]({{% relref "/global-team/infrastructure/1password" %}}) Shared vault.
- Leadership members are added as Workspace Members with their own logins.
- TODO: list current OpenPhone members.

## Operational tasks

### Signing in

1. Open [my.openphone.com](https://my.openphone.com) or download the desktop or mobile app.
2. Sign in with your individual OpenPhone credentials (or the admin login from 1Password for admin operations).
3. Complete 2FA.

### Listening to a voicemail

1. Open the OpenPhone app or web client.
2. Click **Phone** → **Inbox**.
3. New voicemails appear at the top with a transcript preview.
4. Click to read the transcript, or play the audio.
5. Mark the message as resolved (or reply) so it doesn't dangle in the shared inbox.

### Returning a call

1. From a voicemail or a missed call, click the number → **Call back**.
2. Or **Phone** → **+ New call** → select the RLadies+ number as caller ID → dial.
3. The recipient sees the RLadies+ number, not your personal one.

### Replying to an SMS

1. **Messages** in the left sidebar.
2. Click the thread.
3. Type the response and send.
4. Other team members see the thread; coordinate via the conversation's internal-note feature (visible to team only, not the recipient) to avoid duplicate replies.

### Adding a new team member

1. Sign in as admin → **Settings** → **Members** → **Invite member**.
2. Enter their email and assign a role (typically Member).
3. They accept the invite and complete login setup.
4. Walk them through Inbox and Messages so they know where things land.

### Removing a departed team member

1. **Settings** → **Members** → click the departing member → **Remove**.
2. Any threads they were the most-recent replier on are now "owned" by the team generally; check Inbox for dangling threads and reassign or close.

### Configuring call routing and business hours

1. **Settings** → **Phone numbers** → click the RLadies+ number → **Hours and routing**.
2. Define business hours (e.g., weekdays 9am-5pm Pacific) and what happens outside them (typically straight to voicemail with a greeting).
3. Within business hours, decide whether all members ring simultaneously (loud), only one member rings (quiet but reliant on that one person), or nobody rings and calls go straight to voicemail (which is honestly fine for a volunteer org — voicemails get triaged within a working day).

### Setting up the voicemail greeting

1. **Settings** → **Phone numbers** → click the number → **Voicemail**.
2. **Record** in the browser, or **Upload** a pre-recorded file.
3. The greeting should identify RLadies+ and set expectation: "You've reached RLadies+. Leave a message and we'll get back to you within a few business days, or email us at info@rladies.org for a faster response." TODO: confirm whether `info@` is the right address to direct callers to, or whether it should be `accounts@` / `contact@`.

### Configuring call filtering for spam

1. **Settings** → **Phone numbers** → click the number → **Call filtering** (or similar).
2. Enable spam-call blocking — OpenPhone uses a community-maintained database; most telemarketers get filtered before ringing.
3. Optional: send unknown callers to voicemail to filter further (downside: some legitimate callers won't leave a message).

## Recovery

2FA recovery codes saved in 1Password are the first option.
OpenPhone's account recovery falls back to the role mailbox for email verification — Workspace access remains the prerequisite.

If the OpenPhone account itself is the blocker (port-out fraud, suspicious sign-in), contact OpenPhone support immediately from a separate trusted email and freeze the number.

## Things to watch for

- **Unanswered voicemails.** Even "just telemarketers" voicemails should be skimmed within a working week — occasionally a real opportunity hides among the noise. Build the habit of checking the Inbox at the same cadence as Slack DMs.
- **Spam call volume.** US numbers attract robocalls; if the rate becomes disruptive, tighten the call filtering settings.
- **Shared-inbox confusion.** Without a convention, two members may reply to the same SMS independently and confuse the recipient. Agree on "claim before you reply" or use OpenPhone's internal-notes feature to coordinate.
- **Account billing notifications.** They land on the role mailbox; forward to finance for the monthly reconciliation.
- **Port-out risk.** The phone number itself has value; an attacker who compromises a US carrier account can sometimes port the number elsewhere. The role-mailbox recovery email plus 2FA on the OpenPhone account are the controls; review them periodically.
