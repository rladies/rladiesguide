---
title: "Google Workspace"
linkTitle: "Google Workspace"
weight: 70
chapter: false
---

Every RLadies+ chapter has an email address ending in `@rladies.org`, and every one of those mailboxes lives in a single Google Workspace tenant.
The tenant runs on the [Google for Nonprofits](https://www.google.com/nonprofits/) programme on the Workspace Nonprofit plan, with `rladies.org` as the only domain we manage.

## What we actually use

In practice, we use Workspace for two things: email and the occasional shared document.
Email is the load-bearing piece — chapter mailboxes (`yourcity@rladies.org`) plus personal aliases (`yourname@rladies.org`) for leadership and Global Team members issued since March 2019.
A handful of Global Team meeting notes live in Drive.
Everything else Workspace offers — Chat, Meet, Sites, Vault — is essentially untouched at the Global Team level.

That is intentional rather than accidental.
Every surface we don't use is a surface we don't have to secure, train new volunteers on, or migrate when policies change.
The smaller our footprint, the less drama when an organiser rotates off the team.

## Who administers it

The Workspace super-admin is a shared leadership role mailbox.
Credentials live in the shared 1Password vault — see [1Password]({{% relref "/global-team/infrastructure/1password" %}}) for how to reach them.

Routing admin through a role mailbox rather than a specific person's account is deliberate.
Volunteers come and go; the org keeps running.
When a single human owns the super-admin, their departure becomes a recovery operation rather than a handover.

## Pages in this section

- [Admin Console]({{% relref "admin-console" %}}) — how to reach `admin.google.com`, what lives there, and how to recover if you're locked out.
- [Chapter emails]({{% relref "chapter-emails" %}}) — the Global Team view of how chapter mailboxes get provisioned. The login and day-to-day usage guides live in the organiser-facing [tech accounts]({{% relref "/organizers/tech/accounts" %}}) and [chapter email]({{% relref "/organizers/tech/email" %}}) pages.
- [Shared Drive]({{% relref "shared-drive" %}}) — where Global Team meeting notes live and who can read them.

## When the Nonprofit plan stops being enough

The Nonprofit tier covers our needs comfortably today: pooled storage across users, standard Gmail and Drive, basic security controls.
The two things that would push us off it are running out of storage or wanting features the plan does not include — Vault retention, advanced endpoint management, more aggressive phishing protection.
If either pressure shows up, the upgrade path is to a paid Workspace tier through the same admin console; nothing about email addresses or accounts changes.

For now, staying on Nonprofit keeps the org's recurring software bill at zero for the thing most chapters interact with daily.
