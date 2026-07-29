---
title: "Buffer and platform reconnections"
linkTitle: "Buffer"
weight: 10
---

Every RLadies+ post on Bluesky, Mastodon, LinkedIn, Instagram, and YouTube gets published through [Buffer](https://buffer.com/).
Volunteers schedule posts in Buffer, Buffer pushes them to the platforms, and almost nothing requires logging in to a platform admin interface directly.
The two situations where the underlying platform credentials matter are the initial connection setup and reconnecting Buffer when a platform-side authorisation breaks.
This page is mostly about that second case.

## Why we use Buffer

One scheduling tool, one calendar across platforms, one place to write a post and decide where it lands.
Handover to a new volunteer is "here is a Buffer account" instead of "here are five sets of credentials."

The cost is that Buffer becomes a single point of failure.
When a Buffer-to-platform connection breaks — token expired, platform changed its API, the account password rotated, 2FA was added — Buffer stops pushing silently until someone re-authorises.
The fallback is to sign in to the platform directly using the credentials from [1Password]({{% relref "/global-team/infrastructure/1password" %}}) and post manually.
Treat that as a manual override, not a workflow.

## The Buffer account

Login email: the role mailbox — the same shared role mailbox used as the admin login for [Google Workspace]({{% relref "/global-team/infrastructure/google-workspace" %}}) and [Cloudflare]({{% relref "/global-team/infrastructure/cloudflare" %}}).
Password lives in the shared vault in [1Password]({{% relref "/global-team/infrastructure/1password" %}}).

Plan tier: TODO — confirm whether the org is on Buffer's free tier or one of the paid tiers.
Buffer's free tier covers three channels per account; with five platforms connected the org is almost certainly on a paid plan, but the specific tier should be filled in here.

## Who has access

Volunteers are added as individual team members in Buffer rather than sharing one login.
That keeps the audit trail honest and lets us remove people cleanly when they roll off.

Current team members: TODO — list the volunteers currently in Buffer with their roles.

To add or remove members, sign in at [app.buffer.com](https://app.buffer.com/) and go to **Settings → Team**.
See the [adding](#adding-a-new-social-media-team-member) and [removing](#removing-a-departed-team-member) sections below for the literal flow.

## Connected platforms

These are the channels Buffer publishes to.
The [social media management page]({{% relref "/global-team/social-media-management" %}}) is the day-to-day reference for what to post and where; this table is just the connection inventory.

| Platform  | What Buffer publishes there                                                     | Last re-authorised |
| --------- | ------------------------------------------------------------------------------- | ------------------ |
| Bluesky   | Posts to [@rladies.org](https://bsky.app/profile/rladies.org)                   | TODO               |
| Mastodon  | Posts to [@RLadiesGlobal@fosstodon.org](https://fosstodon.org/@RLadiesGlobal)   | TODO               |
| LinkedIn  | Posts to the [RLadies+ Company Page](https://www.linkedin.com/company/rladies/) | TODO               |
| Instagram | Posts to [@rladies](https://www.instagram.com/rladies/) via Meta Business       | TODO               |
| YouTube   | Posts to [RLadies+ Global](https://www.youtube.com/@RLadiesGlobal) — see notes  | TODO               |

The [WeAreRLadies rotating curation]({{% relref "/rocur" %}}) on Bluesky has its own credential model.
TODO: confirm whether RoCur flows through this Buffer account at all or is fully separate.

## The channel health dashboard

Buffer keeps the connection inventory live at [app.buffer.com](https://app.buffer.com/) → **Channels** in the left sidebar.
Every connected platform shows up as a row, with a status of **Connected** or some flavour of warning state ("Reconnect required," "Token expired," a red dot).
A row that is silently in a warning state is the most common reason posts stop going out — Buffer rarely interrupts a workflow to tell you, it just lets the warning sit there.

An admin should open this page once a month, scroll the full list, and treat anything that is not plain "Connected" as a ticket to work that day.
This catches the slow failures (expired tokens, password rotations, platform-side permission changes) before they hit a scheduled post.

## Drafts, scheduled posts, and the queue

These three statuses look similar in the composer and behave very differently.
Misunderstanding them is the single most common cause of "I scheduled it but it didn't post."

A **draft** is unscheduled and lives under **Content → Drafts**.
A draft will not post, ever, until someone opens it and either schedules it for a specific time or adds it to the queue.
Use drafts for posts you are still wording or waiting on a sign-off for.

A **scheduled post** has a specific time attached and lives under **Content → Scheduled**.
Buffer will publish it at that time, full stop — no slot calculation, no queue logic.
This is what you want for time-sensitive posts (event reminders, "registration opens at 14:00 UTC today").

A **queued post** does not have its own time.
It slots into the next free time in that channel's posting schedule and lives under **Content → Queue**.
The queue is what makes Buffer a calendar tool rather than a glorified scheduler — drop posts in, Buffer spaces them out across the configured slots.
Use the queue for evergreen content where the exact minute does not matter.

If a post is "scheduled" but never went out, check whether it is actually in **Drafts**.
That is the answer nine times out of ten.

## Configuring a channel's posting schedule

Each channel has its own posting schedule — the days and times Buffer uses when it places queued posts.
A channel with an empty schedule will accept queued posts but never publish them, because there is no slot for the queue to land in.

Open the schedule at **Channels → click the channel → Posting Schedule** tab.
Existing slots are listed by day of week.
**Add time slot** adds a new one; clicking an existing slot lets you edit or delete it.

The schedule is per-channel, so Bluesky and LinkedIn can run on different cadences.
TODO: document the current schedule for each channel once leadership confirms what we want the cadence to be.
Until then, scheduled posts (with explicit times) work regardless of what is in the schedule.

## Re-authorising a broken Buffer connection

This is the page's reason for existing.
A connection looks broken when Buffer shows a red warning banner on the dashboard, when a channel page shows "disconnected" or "reconnect required," or when scheduled posts to one platform start failing while everything else publishes fine.

The general flow is the same for every platform:

1. Log in to Buffer at [app.buffer.com](https://app.buffer.com/) using the credentials from [1Password]({{% relref "/global-team/infrastructure/1password" %}})
2. Click **Channels** in the sidebar, or click the warning banner — Buffer surfaces broken connections at the top of the channels list
3. Click the broken channel, then **Reconnect** (sometimes labelled **Refresh authentication**)
4. Buffer opens an OAuth window for the platform; sign in using that platform's admin credentials from [1Password]({{% relref "/global-team/infrastructure/1password" %}})
5. Approve Buffer's requested permissions when the platform asks
6. Verify the connection by actually publishing a test post — see [Verifying a reconnect](#verifying-a-reconnect) below

That sequence covers the happy path.
The per-platform notes below cover the bits that differ.

### Verifying a reconnect

Buffer marking a channel as **Connected** is not proof the channel is publishing.
The OAuth handshake can succeed while the underlying permission is incomplete, especially on Instagram and LinkedIn.
Do not trust the green badge — test it.

Compose a low-stakes post in Buffer (a single emoji, or the literal text "RLadies+ Buffer reconnection test," no hashtags, no links).
Schedule it for one minute in the future on the reconnected channel.
Wait for the scheduled time, then open the platform in a separate tab and confirm the post is actually visible there — not just in Buffer's "Sent" view.
Once confirmed, delete the test post from both the platform and Buffer's history (**Content → Sent**, click the post, **Delete**).

If Buffer says the post was sent but the platform shows nothing, the connection is still broken — go back to step 3 of the reconnect flow.

### Bluesky

Bluesky uses app passwords rather than the main account password.
To regenerate one, sign in to [bsky.app](https://bsky.app/) as the `@rladies.org` account and go to **Settings → App passwords → Add App Password**.
Name it "Buffer," copy the value once (you cannot view it again), and paste it into Buffer's reconnect dialog.

App passwords do not expire on their own; they get revoked when the main Bluesky password is rotated, which is the common reason this connection breaks in practice.
Revoke any older "Buffer" app password after the new one is confirmed working.

### Mastodon (fosstodon.org)

The OAuth flow signs you into [fosstodon.org](https://fosstodon.org/) first — those credentials belong to the `@RLadiesGlobal` account and live in 1Password.
After you approve Buffer, the connection is bound to that account's authorised apps list, visible at [fosstodon.org/settings/applications](https://fosstodon.org/settings/applications).
Revoke an old Buffer authorisation from that page if needed.

### LinkedIn

Buffer connects to the RLadies+ Company Page, not to a personal LinkedIn profile.
The person doing the reconnect needs two things at the same time: a Buffer session signed in with the org's Buffer login, _and_ a LinkedIn session signed in as a Page Admin on the RLadies+ Company Page.
LinkedIn's OAuth screen will only offer Pages the signed-in personal account actually admins.

If you are not a Page Admin, you cannot complete this reconnect — ask social-media leadership to do it from a session that has both.
This is the most common reason LinkedIn reconnects stall.

### Instagram

Instagram connects through Facebook Business / Meta Business Suite rather than directly.
Reconnecting involves signing into the Facebook account that owns the Instagram Business account, then re-granting Buffer permission to publish via Meta Business Suite.

This connection is the most fragile of the lot.
Meta rotates app permissions and occasionally requires the connected Facebook account to re-confirm business details before posting will resume.
If the reconnect screen asks for a re-verification step (a phone code, an ID check, business information), check with social-media leadership before clicking through — those decisions belong to the account owner.

TODO: document the exact Facebook account that owns the Instagram Business account, and where its credentials live.

### YouTube

YouTube connects via the Google account that owns the RLadies+ YouTube Brand Account.
Reconnection signs into Google using the role mailbox (or whichever Google identity is the brand owner) and approves Buffer through Google's standard OAuth consent.
Use the credentials in 1Password; if 2FA prompts a code, the [Google Workspace page]({{% relref "/global-team/infrastructure/google-workspace" %}}) covers where the recovery options live.

TODO: confirm what we actually use YouTube posting through Buffer for — scheduled video uploads, community-tab posts, or both.

## Adding a new social-media team member

1. Add them to the Global Team shared vault in [1Password]({{% relref "/global-team/infrastructure/1password" %}}) so they can read the platform credentials when a reconnect is needed
2. Invite them to Buffer: [app.buffer.com](https://app.buffer.com/) → **Settings → Team → Invite**, enter their personal email and the appropriate role
3. They accept the email invite and set up their own Buffer login
4. Walk them through the **Channels** overview and the posting **Calendar**
5. Have them schedule and then delete a draft post as a first-run check

## Removing a departed team member

1. Remove them from Buffer: **Settings → Team**, click the ⋯ menu next to their name, then **Remove**
2. Remove them from the Global Team shared vault in [1Password]({{% relref "/global-team/infrastructure/1password" %}}) if they had access

This does _not_ affect the platform connections.
Each channel is bound to the org's Buffer account, not to the individual team member who originally set it up, so no platform reconnection is needed when someone leaves.
The exception is if the departing person was the named Page Admin on LinkedIn or the named brand owner on YouTube — those are platform-side admin roles that need to be reassigned separately, outside Buffer.

Posts the departing member already scheduled or queued remain in place after removal.
Buffer does not auto-cancel their pending posts, but it does keep showing them as authored by the removed member, which can be awkward when someone needs to edit or reschedule.
Walk through **Content → Scheduled** and **Content → Queue**, filter by the departing member's name, and decide post by post: leave it alone if the content is fine and timed correctly, reassign ownership by deleting and recreating from your own account if it needs editing, or delete outright if it is no longer relevant.
Do this before removing them from Buffer so the filter still works.

## When Buffer says posted but the platform is empty

A specific and infuriating failure mode: Buffer's **Sent** view shows the post as published, no warning, no red banner, but the post is nowhere on the platform.
Causes include silent token expiry, platform-side rate limits, an API change that broke one specific post type (Instagram Reels, LinkedIn carousels), or a moderation action the platform did not surface back to Buffer.

The diagnostic path:

1. **Content → Sent**, find the post, click it, look at the delivery status on the right — Buffer sometimes has a more detailed error message tucked there even when the top-level status reads "Sent"
2. Open the platform directly in a separate tab and look for the post under the org account
3. If Buffer claims delivery but the post is missing on the platform, treat the connection as broken even though Buffer has not flagged it
4. Go to **Channels → click the channel → Reconnect** and run the [re-authorisation flow](#re-authorising-a-broken-buffer-connection), including the [verification step](#verifying-a-reconnect)

Do not just resend the same post from Buffer hoping it goes through this time — a half-broken connection will silently swallow the second one too.

## Reposting an old post

Common operation for evergreen content (recurring events, "we run a mentoring programme" reminders, conference recaps that surface again).
Buffer has a built-in reuse flow rather than copy-paste.

Open **Content → All Posts**, find the post you want to reuse, click the ⋯ menu on the right, and choose **Reuse**.
Buffer opens a new post pre-filled with the same text, media, and channel selection; tweak whatever needs tweaking and schedule or queue it as usual.

Bluesky and Mastodon community norms generally discourage exact-duplicate reposts — they read as spam and tend to get less engagement than the original.
When reusing, rework the wording at least slightly: open with a different sentence, swap the hashtags, link a different supporting resource.
LinkedIn, Instagram, and YouTube are more tolerant of close repeats, but a small reword still helps the post feel current rather than recycled.

## Analytics

TODO: confirm whether the current Buffer plan includes analytics.
Buffer's free tier does not include channel analytics; the paid Essentials/Team/Agency tiers do, with different depth.

If the plan includes analytics, the path is **Analytics** in the left sidebar → choose a channel → set a date range → **Export** for a CSV.
Use this for the quarterly social-media review (which posts landed, which channels grew) rather than reading post-by-post engagement in real time — Buffer's analytics lag the native platform insights by a day or two.

If the plan does not include analytics, the fallback is each platform's own native insights, signed into directly as the org account.
This is more clicking and less comparable across platforms; flagging the gap here so leadership can weigh upgrading the Buffer plan against living with native insights.

## When Buffer itself is the problem

If Buffer is down, check [status.buffer.com](https://status.buffer.com/).
The posting cadence on the [social media management page]({{% relref "/global-team/social-media-management" %}}) has enough slack to absorb a short outage; for a longer one, fall back to posting manually from the platform using credentials in [1Password]({{% relref "/global-team/infrastructure/1password" %}}).

If you cannot log in to Buffer, credentials are in 1Password — if 1Password itself is the blocker, the [1Password page]({{% relref "/global-team/infrastructure/1password" %}}) covers recovery; if you are still stuck, reach out to leadership rather than guessing at password resets on a shared account.

If a Buffer billing email arrives, forward it to the Global Team finance contact before acting on it.
Plan changes and payment updates are a leadership decision, not a click in the email.

## Why platform admin credentials still matter

The platform admin credentials in [1Password]({{% relref "/global-team/infrastructure/1password" %}}) get used every time a connection breaks and every time a reconnect happens.
They are also the recovery-of-last-resort: if the org ever leaves Buffer, or a platform locks the Buffer connection out, the direct admin login is the only way back in.
Treat them as critical even though they are rarely touched.

The [social media management page]({{% relref "/global-team/social-media-management" %}}) is the day-to-day reference for posting; this page is the one to come back to when Buffer's connection to one of the platforms breaks.
The chapter-facing [social media guide]({{% relref "/organizers/online-presence/social-media" %}}) sits on the other side — chapters run their own platform accounts and do not touch the org's Buffer.

## Audit cadence

The monthly walk of [the channel health dashboard](#the-channel-health-dashboard) catches broken connections early.
On top of that, every six months walk each channel end to end: open it, confirm it is not in a "reconnect" state, schedule a draft post to confirm the channel still accepts it, and update the last-re-authorised column in the [connected platforms](#connected-platforms) table.
Silent failures are how a quiet platform becomes a stale platform.
