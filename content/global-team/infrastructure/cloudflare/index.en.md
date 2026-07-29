---
title: "Cloudflare"
linkTitle: "Cloudflare"
weight: 60
aliases:
  - /website/admin_guide/cloudflare/
  - /website/admin_guide/domain-management/
  - /website/admin_guide/dns/
---

Every DNS record that points anyone at `rladies.org` resolves through a single Cloudflare account, and Jinx's front door is a Worker living in that same account.
That keeps the surface small: one login, one audit trail, one place to look when something on the public internet stops behaving.

## What we use it for

Cloudflare is both the registrar and the authoritative DNS host for `rladies.org`.
Running both at the same provider is a deliberate choice for a small org — there is no transfer drift, no second portal to remember, and every change shows up in one audit log.
The tradeoff is concentration risk, which we mitigate by keeping the admin account tightly scoped (see [Who has access](#who-has-access) below).

Beyond DNS, Cloudflare hosts the runtime for [Jinx]({{% relref "/global-team/jinx" %}}) — Workers for the request handling, Vectorize for the RAG index, and Workers AI for embeddings and answer generation.
[Cloudflare Web Analytics](#web-analytics) sits on top of the site for traffic stats.
We are on the Free tier across all of it.

## Jinx services on Cloudflare

The Jinx pages cover each of these in detail.
This table is the map.

| Service    | What it does for RLadies+                                                                                                                                         | Where it's documented                                                   |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Workers    | `rladies-jinx.workers.dev` — front door for Slack events, slash commands, and the Airtable webhook                                                                | [Slash commands]({{% relref "/global-team/jinx/slash-commands" %}})     |
| Workers    | Same Worker handles DM / @-mention / Assistant Q&A                                                                                                                | [Asking Jinx]({{% relref "/global-team/jinx/ask-jinx" %}})              |
| Workers    | Same Worker handles the Airtable invite webhook                                                                                                                   | [Airtable invites]({{% relref "/global-team/jinx/airtable-invites" %}}) |
| KV         | Worker-side state (allowlists, pending invites, dedup keys) — TODO: enumerate namespaces, see [for-developers]({{% relref "/global-team/jinx/for-developers" %}}) | [For developers]({{% relref "/global-team/jinx/for-developers" %}})     |
| Vectorize  | `rladies-content` index backing Jinx's RAG answers                                                                                                                | [Content indexer]({{% relref "/global-team/jinx/indexer" %}})           |
| Workers AI | BGE embeddings + Llama-3.1 answer generation                                                                                                                      | [Content indexer]({{% relref "/global-team/jinx/indexer" %}})           |

If Jinx goes quiet in Slack but GitHub Actions are healthy, the Worker is the first place to check.

## Who has access

The Cloudflare login is a shared leadership role mailbox rather than any one person's address.
The password and the TOTP seed both live in 1Password (see [1Password]({{% relref "/global-team/infrastructure/1password" %}})), so anyone with the shared vault can complete 2FA without depending on a single person's phone.

The pattern has the obvious downside: a shared password is a shared password, and rotating it means rotating it for everyone who currently holds it.
The upside is continuity.
When a Global Team member rolls off, the account does not roll off with them, and no one has to discover six months later that the only person who could log in is no longer reachable.
We keep the holder list short and rotate the password when anyone with access leaves.

If 2FA stops working — phone lost, TOTP seed missing from 1Password, codes rejected — recover before the situation becomes urgent.
The recovery codes generated when 2FA was set up are stored alongside the password in 1Password; use one to sign in, then regenerate the seed under **My Profile → Authentication** in the Cloudflare dashboard and re-save it to 1Password.
If neither the seed nor a recovery code is available, Cloudflare account recovery requires email verification at the role mailbox plus a support ticket — start that conversation before the next deploy, not during one.

## Tokens and secrets

`CLOUDFLARE_API_TOKEN` is the one Cloudflare credential that lives outside the account itself.
It is stored as an org-level secret on `rladies` and used in two places: `wrangler deploy` to push Worker updates, and the indexer workflow to write vectors into the `rladies-content` index.
See [Jinx for developers]({{% relref "/global-team/jinx/for-developers" %}}) for the full secret inventory and the [GitHub PAT page]({{% relref "/global-team/infrastructure/github-pat" %}}) for the general pattern we use for repo-level credentials.

To create or rotate the token, log in as the role mailbox and go to **My Profile → API Tokens → Create Token**.
Use the "Edit Cloudflare Workers" template, scope it to the RLadies+ account, and add the **Vectorize: Edit** permission so the indexer workflow can write to `rladies-content`.
Save the new value with `gh secret set CLOUDFLARE_API_TOKEN --org rladies --visibility selected --repos jinx`, then verify by triggering `infra-deploy-worker.yml` from the Actions tab and watching it run green — `gh workflow run "Infra · Deploy Worker" --repo rladies/jinx`.
Once the new token is confirmed working, revoke the old one in the same Cloudflare dashboard page.

Rotate the token when:

- A Global Team member with Cloudflare access leaves
- A deploy fails with an auth error in the `infra-deploy-worker.yml` or `bot-index-content.yml` workflow
- The token value has been exposed (a CI log, a screenshot, a paste in the wrong channel) — revoke immediately at **My Profile → API Tokens** before issuing the replacement
- It has been a year — set a calendar reminder when you create one

Worker runtime secrets (the Slack signing secret, Slack bot tokens, Airtable PAT and webhook secret, the GitHub App private key) are set with [Wrangler](https://developers.cloudflare.com/workers/wrangler/) rather than as environment variables.
They live encrypted at rest, scoped to the Worker, and never appear in the dashboard or in CI logs.
That is meaningfully nicer than treating them as GitHub Actions env vars: a leaked GHA log can echo an env var; a Worker secret does not exist outside the Worker's runtime.

The full inventory of Worker secrets lives in [Jinx for developers]({{% relref "/global-team/jinx/for-developers" %}}#cloudflare-worker-secrets) — `SLACK_SIGNING_SECRET`, `SLACK_ORGANIZER_BOT_TOKEN` and `SLACK_COMMUNITY_BOT_TOKEN`, `SLACK_ORGANIZER_TEAM_ID` and `SLACK_COMMUNITY_TEAM_ID`, `AIRTABLE_PAT`, `AIRTABLE_WEBHOOK_SECRET`, plus `JINX_APP_ID` and `JINX_PRIVATE_KEY`.
To set or rotate one, install Wrangler (`npm install -g wrangler`, or call everything through `npx`), authenticate against the RLadies+ account once with `npx wrangler login`, then from the `jinx` repo root:

```bash
npx wrangler secret put SLACK_SIGNING_SECRET
```

Wrangler prompts for the value, encrypts it, and binds it to the Worker.
The secret is never logged or echoed; if you need to read the current value, you cannot — you have to rotate it.
List what is currently set with `npx wrangler secret list`, and delete a stale binding with `npx wrangler secret delete <NAME>`.

## Web analytics

Cloudflare Web Analytics is enabled on `rladies.org`.
It tracks page views, top referrers, and a country breakdown — the basics, no funnels, no session replay.
There are no cookies and no personally identifying data collected, which is the reason we picked it over the usual alternatives.

To view it, log in as the role mailbox and pick **Analytics & Logs → Web Analytics** from the left-hand navigation in the Cloudflare dashboard, then select the `rladies.org` site.
If you want a metric the Web Analytics view does not surface, it is probably not there — the privacy posture is the point, not a limitation we plan to work around.

## Things to watch for

DNS changes are silent and high-blast-radius.
A typo'd `A` record can take the site offline without anything else lighting up.
Review the records once a quarter under **Websites → rladies.org → DNS → Records**, and skim the audit log at **Manage Account → Audit Log** — it's faster to notice an unexpected change at three months than at three years.

When Jinx stops responding in Slack and the GitHub Actions runs all look green, the Worker is the next place to look.
Open the Worker at **Compute (Workers) → rladies-jinx → Logs** in the Cloudflare dashboard for live tailing, or run `npx wrangler tail` from the `jinx` repo for the same stream in your terminal.
Look for signature-verification or rate-limit failures; the [Jinx troubleshooting]({{% relref "/global-team/jinx/troubleshooting" %}}) page has the runbook for the specific failure modes we have actually hit.

If billing or plan email arrives for the role mailbox, forward it to the Global Team finance contact before acting on it.
We are on the Free tier deliberately, and any nudge to upgrade should be a conversation, not a click.
If Cloudflare flags an actual billing problem (a payment method on file expiring, for instance), the dashboard shows it at **Manage Account → Billing**; resolve it from there rather than through the email link.
