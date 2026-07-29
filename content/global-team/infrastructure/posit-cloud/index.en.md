---
title: "Posit Cloud"
linkTitle: "Posit Cloud"
weight: 120
---

When a chapter wants to run an R workshop with everyone on the same packages and the same dataset, they fill in the Posit Cloud Request form.
Someone on the RLadies+ Posit Cloud Team reads the request, opens [posit.cloud](https://posit.cloud) in a browser, creates a Shared Space for the chapter, and adds the workshop lead as an admin.
The space sits there for the event, then gets cleaned up two weeks after.

This page is the runbook for the people doing the reading, the opening, and the cleaning up.
The organiser-facing side of the same workflow — what chapters submit and what their space admin does once invited — lives on [tech infrastructure for chapters]({{% relref "/organizers/tech/accounts" %}}#posit-cloud).
Cross-link, don't duplicate.

## What we have

RLadies+ holds a donated Posit Cloud license — most likely through the [Posit nonprofit programme](https://posit.co/about/nonprofit-program/), possibly via an R-Consortium grant.
TODO: confirm the donation source and record whoever currently corresponds with Posit about the license, plus any renewal cadence.

Full license terms live on the [organiser-facing accounts page]({{% relref "/organizers/tech/accounts" %}}#r-ladies-posit-cloud-license-terms).
The shape worth remembering operationally: seats for 10 instructors and 440 students, unlimited shared spaces and projects, unlimited aggregate compute hours.
The per-project ceilings are real, though — 16 GB RAM, 4 CPU, 48 hours of execution per project, 96 hours per job.
We can run plenty of workshops in parallel without worrying about account-level totals, but a single workshop doing heavy computation can still hit the per-project caps.
If a chapter's workshop description involves "large dataset" or "long-running model fit", flag that during request review rather than at runtime.

## The RLadies+ Posit Cloud Team

A small group of Global Team members holds admin access on the RLadies+ Posit Cloud account.
TODO: list current Posit Cloud Team members.

Keeping admin in a small dedicated team rather than spreading it across all of leadership is deliberate.
The platform's admin model rewards continuity — the same hands setting up spaces means consistent naming, consistent cleanup, and accumulated knowledge of the platform's quirks.
Diluting admin across leadership leads to the kind of drift where nobody is sure which spaces are still in use and nobody remembers how to add a member without re-reading the docs.

New Team members are added through the [Global Team onboarding flow]({{% relref "/global-team/onboarding" %}}) — TODO: confirm whether Posit Cloud is explicitly part of onboarding today or an ad-hoc conversation.
Either way, granting access is documented under [granting additional Team members admin access](#granting-additional-team-members-admin-access) below.

## Where requests land

The `posit-cloud` channel in the organisers Slack is the queue.
A chapter submits the [Posit Cloud Request form](https://rladies.org/form/posit-cloud-request), the form output lands in the channel (TODO: confirm the routing — Slack webhook? Airtable base relay? plain email forwarded into Slack?), a Posit Cloud Team member claims the request in-thread, reviews the date and chapter details, then proceeds with space creation.

Claim-in-thread matters.
Without it, two team members can start setting up the same space in two browser tabs, and you end up with `rladies-london-shiny-workshop` and `rladies-london-shiny-workshop-1` in the spaces list, plus a confused workshop lead with two invitations.
React to the request message or reply in-thread before opening posit.cloud.

## Processing a new request

The runbook for turning a form submission into a working space:

1. Confirm the event is at least two weeks out.
   If it is closer than that, reply in-thread acknowledging the tight timeline and proceed anyway — accommodation is fine, just track it in case the rule needs revisiting.
2. Log in to [posit.cloud](https://posit.cloud) using the credentials from [1Password]({{% relref "/global-team/infrastructure/1password" %}}) under the Shared vault entry for the RLadies+ Posit Cloud account.
3. Click **+ New Space** at the top-right of the spaces dashboard.
4. Name the space `chapter-title` — for example, `rladies-london-shiny-workshop` or `rladies-buenosaires-tidymodels-intro`.
   Chapter name first, short event title second, no spaces, no capitals, no dates.
5. Assign the space to the R-Ladies organisation.
6. Click into the new space, then **Members** (or **Settings → Members**, depending on Posit Cloud's current layout).
7. Click **Add Member**, enter the workshop leader's email exactly as it appeared on the form, set the role to **Admin**.
8. Reply in the same `posit-cloud` Slack thread you claimed.
   Confirm the space exists, the admin invite is out, and the invite expires in 7 days if not accepted.
   Ask the lead to flag in-thread once they have accepted.

The chapter admin handles the About section, project creation, and sharing-link configuration — see the [organiser-facing runbook]({{% relref "/organizers/tech/accounts" %}}#posit-cloud-use).

## Granting additional Team members admin access

When a new Posit Cloud Team member joins, give them admin on the organisation rather than admin on individual spaces:

1. Log in to posit.cloud with the shared credentials
2. Go to the **R-Ladies organisation** page
3. Open **Members**
4. Click **Add Member**, enter their email, set role to **Admin**
5. Confirm with them in Slack once the invite has gone out

Org-level admin gives them access to every shared space without needing to be added to each one individually.
That is the whole reason the team exists at this scope.

## Removing a departed Team member

Same path, in reverse, plus a sweep of any spaces they were added to directly:

1. Posit.cloud → R-Ladies organisation → Members
2. Click the **⋯** next to the departing member's name → **Remove from organisation**
3. Walk the **Spaces** dashboard and, for any space that has them as a direct member, open **Members → ⋯ → Remove**
4. Note the removal in the offboarding record alongside the other accounts being revoked

The org-level removal is the important one; the per-space sweep matters because workshop spaces sometimes have direct member adds that survive an org-level removal.

## License utilisation check

Periodically — every couple of months, and definitely before a known busy season — check how much of the 10-Instructor / 440-Student license is currently in use.

TODO: confirm the exact menu path.
At time of writing the most likely route is posit.cloud → R-Ladies organisation → **Settings** or **Licensing** tab.
If neither surfaces a utilisation summary, count instructor seats from the org Members list and student seats from the active spaces.

If utilisation is trending toward the cap, reach out to the Posit nonprofit programme contact (TODO: name and email) to request a license increase before it bites during a workshop.

## Space lifecycle and cleanup

Two weeks after an event, spaces should be either deleted or, for repeat workshops the requester flagged on the form, kept after confirming in Slack that the space is still needed.
There is no monetary cost to leaving spaces around, but the spaces list is the only easy way to see what is active — a list cluttered with dead workshops makes it harder to spot the live ones.

The cleanup path: posit.cloud → Spaces dashboard → for each space whose event date is more than two weeks past, click **⋯ → Delete space**.
TODO: confirm whether Posit Cloud also supports Archive separately from Delete — if so, Archive is the safer choice for repeat-workshop spaces between runs.

Make it a calendar habit.
Every two weeks, a Team member walks the spaces list and prunes.
Skip the walk for a year and you have a half-day project nobody wants to start.

## Troubleshooting

_"The chapter admin says they didn't get the invite."_
Open the space → **Members** → find the pending entry → **⋯ → Resend invite**.
If a second resend produces nothing, confirm the email is correct — Posit Cloud sends invites to the address entered at invite time and you cannot edit it afterwards.
Remove the pending entry and re-add with the corrected email.

_"The chapter is hitting the 25 compute-hour cap on their personal account."_
That cap applies to personal accounts only.
Work inside an RLadies+ shared space does not count against it.
Confirm the user is in the shared space, not in a copy that has ended up in their personal account.
If they are genuinely hitting our org-space caps — 16 GB RAM, 4 CPU per project — that is a separate conversation about whether the workshop design fits Posit Cloud at all.

_"Posit Cloud is unreachable or slow during a live workshop."_
Live-event fallbacks are covered in the chapter event docs — see the organiser-facing accounts page.
Status updates: [status.posit.co](https://status.posit.co).

_"Workshop needs a much larger dataset than the recommended 500 MB."_
Posit Cloud supports up to 20 GB of project storage but performance is poor with files over 500 MB.
What actually works is a randomised subset for the workshop session, with the full file shared separately for participants who want to wrangle it locally afterwards.

## Recovery and security

The Posit Cloud admin login is the role mailbox (TODO: confirm — this matches the pattern used for Cloudflare and Google Workspace, but verify against the actual account before relying on it).
Credentials live in the [Shared 1Password vault]({{% relref "/global-team/infrastructure/1password" %}}).

If the login itself is the blocker — password failing, 2FA broken, mailbox unreachable — talk to leadership before triggering Posit's account-recovery flow.
Posit's recovery email goes to the role mailbox, which is the same mailbox sitting behind several other services.
If Workspace is the underlying problem, attempting recovery here just adds noise.
See the [Google Workspace recovery notes]({{% relref "/global-team/infrastructure/google-workspace" %}}) for the order of operations.

## Things to watch for

License utilisation tends to creep upward across the year, especially around conference seasons and the run-up to International Women's Day.
A quarterly check is enough to spot it before it becomes a problem.

Spaces accumulating past the two-week post-event window is the most common drift.
If the cleanup walk has been skipped for a few months, expect to spend an hour catching up rather than the usual fifteen minutes.

The donated-license relationship is the load-bearing assumption underneath everything on this page.
If the Posit nonprofit programme changes terms — seat caps drop, the application has to be renewed, the programme is sunset — RLadies+ would need to either renegotiate or move to a paid tier.
Watch for renewal and programme emails to the role mailbox, and forward anything that looks like a policy change to leadership rather than acting on it solo.

A quiet `posit-cloud` Slack channel can mean either of two things: workshops are running smoothly with no friction, or no one is monitoring requests and chapters have given up asking.
Audit the channel periodically — if it has been quiet for more than a month, post a check-in.
