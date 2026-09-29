---
title: "Shared Drive and meeting notes"
linkTitle: "Shared Drive"
weight: 30
---

Most of what the Global Team produces collaboratively — meeting agendas, retrospective notes, the occasional draft document — lives in Google Drive under the `rladies.org` tenant, not in any chapter's personal Drive.
Keeping it there means the documents survive a volunteer rotation: when someone steps down, the notes stay with the org rather than disappearing into a personal Google account.

## Where things live

TODO: Confirm and link the actual Shared Drive name and URL where Global Team meeting notes live.

In broad strokes, Global Team meeting notes are kept in a Shared Drive (not "My Drive") so ownership belongs to the tenant rather than any single user.
A document created in a Shared Drive cannot be silently lost when its creator's account is suspended, which is the failure mode we're explicitly avoiding.

## Who can access

Access is granted by the role mailbox super-admin or by an existing manager of the relevant Shared Drive.
The default expectation is that any current Global Team member with an `@rladies.org` personal alias can read meeting notes; write access is scoped to the people who run the meetings.

TODO: Document the exact group or membership pattern used to grant access (e.g. whether membership is a Workspace group, a Shared Drive ACL, or both).

### Granting a new Global Team member access

When someone joins the Global Team and needs to read or edit meeting notes, the literal path is:

1. Open [drive.google.com](https://drive.google.com) signed in as either the role mailbox or your own `@rladies.org` alias if you already have manager rights on the Drive in question.
2. In the left sidebar, click **Shared drives** and open the Drive you're granting access to. TODO: name the specific Shared Drive(s) Global Team members typically need (meeting notes, retrospectives, etc.) and link them here once confirmed.
3. Click **Manage members** (the people icon near the top right of the Drive view).
4. In the **Add people and groups** box, type the new member's `@rladies.org` alias.
5. Pick the right role:
   - **Viewer** — read-only access. Fine for anyone who just needs to follow along.
   - **Commenter** — read plus inline comments. Useful for reviewers.
   - **Contributor** — can edit existing files but can't move or delete them. The usual default for a working Global Team member.
   - **Content manager** — full edit, move, and delete on files. Reserve for people who actively curate the Drive.
   - **Manager** — full control including membership. Reserve for the small set of people who run the relevant function.
6. Untick the _Notify people_ box if you've already told them out-of-band; otherwise let Google send the notification.
7. Click **Send** (or **Share** depending on the current UI label).

If the person doesn't have a personal `@rladies.org` alias yet, provision that first via the [admin console]({{% relref "admin-console#provisioning-a-personal-rladiesorg-alias" %}}) — Shared Drives won't accept a personal Gmail address for our tenant.

## Naming and retention

TODO: Confirm the team's actual naming convention for meeting notes (date prefix, meeting name, etc.) and the retention practice — whether anything is archived, deleted, or moved after a certain period.

In the absence of a documented convention, the safe default is to keep notes indefinitely.
Drive storage on the Nonprofit plan is generous, and old notes have repeatedly turned out to be useful when a question resurfaces a year later about why a decision was made.

## When to use Drive vs somewhere else

Drive is for working documents — agendas, drafts, notes that the team needs to edit together.
Once a decision or policy is finalised, it usually wants to migrate _out_ of Drive and into a more durable home: this guide, a repo README, or an Airtable record, depending on what kind of thing it is.
Documents that only ever live in Drive tend to get forgotten; documents that get promoted into the public guide get maintained.
