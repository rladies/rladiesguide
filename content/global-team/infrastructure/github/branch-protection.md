---
title: "Branch protection and merge policy"
linkTitle: "Branch protection"
weight: 30
---

A protected `main` is the cheapest insurance policy a small org has against accidental damage.
On the core RLadies+ repos, `main` is protected: changes go through pull requests, pull requests need a review, force-pushes are off, and direct commits to `main` are blocked.
The convention applies to `rladies.github.io`, `directory`, `awesome-rladies-blogs`, `global-team`, `jinx`, and `rladiesguide`.

TODO: walk each of the six core repos at `github.com/<repo>/settings/rules` and record the exact ruleset configuration here — what the convention describes below is the working assumption, but the source of truth is the GitHub Settings UI, and the two have drifted before.

## What the convention covers

For the core repos, the working assumption is:

- **All changes via PR.** Direct pushes to `main` are blocked.
- **At least one approving review** before merge.
  On smaller repos this is often the same person approving and merging from a different role; the structural requirement still adds a deliberate pause.
- **Required status checks.** Where a repo has CI (build, lint, link-check, structure-check), those checks must pass.
- **No force-pushes** to `main`.
- **Admin bypass** is enabled — without it, the scheduled and dispatched workflows below could not push at all.

This setup costs almost nothing in day-to-day friction and catches the obvious classes of mistake: rebasing onto the wrong branch, pushing a half-finished commit, an automated process running away.

## Setting it up on a repo

To set up or modify branch protection on a repo:

1. Open the repo on github.com.
2. Click **Settings** in the top tab bar (only visible to repo admins).
3. In the left sidebar, click **Rules → Rulesets** under "Code and automation".
4. Click **New ruleset → New branch ruleset** (or **Edit** on the existing `main` ruleset if one is already in place).
5. Set "Target branches" to `Include default branch`, then enable the rules listed in [What the convention covers](#what-the-convention-covers) above under "Branch rules".
6. Under "Bypass list", add the leadership team (or specific owners) if scheduled workflows running under one of their PATs need to push to `main` — see [Where automation has to bypass](#where-automation-has-to-bypass).
7. Set "Enforcement status" to **Active** and save.

The ruleset is per-repo, so the same configuration needs applying to each core repo individually.
There is no org-wide branch-protection template on GitHub at the time of writing — the closest substitute is documenting the convention here and reviewing it on each repo during the [periodic membership audit]({{% relref "org-membership#review-cadence" %}}).

GitHub still supports the older "Branch protection rules" UI under **Settings → Branches**.
New rules should use rulesets; the older UI is being phased out and is left in place only for repos whose protection predates rulesets.

## Where automation has to bypass

A handful of workflows legitimately push to `main` without going through a PR.
This is where the cost of branch protection shows up: every one of these flows needs an identity with bypass permission, and the credentials for that identity are the most sensitive secrets in the org.

The two known bypass identities, both documented in their own runbooks:

- [`ADMIN_TOKEN`]({{% relref "../github-admin-token" %}}) — used by `global-team.yml` on `rladies.github.io` to push the Airtable-sourced Global Team data straight to `main`, and by `merge-pending.yaml` to auto-merge scheduled blog posts.
- `push-to-protected` SSH deploy key — used by the same `global-team.yml` workflow on its push step; see [SSH Deploy Keys]({{% relref "../ssh-deploy-keys" %}}).

The reason these workflows bypass protection rather than open a PR is mostly cadence.
A 12-hour scheduled sync of team data doesn't gain much from a human review step — there's no human watching at 3am to approve it.
A scheduled blog merge whose only job is to flip a date-gated PR from "pending" to "merged" is similarly mechanical.

When the bypass identity goes away — the PAT expires, the SSH key is revoked, the person who created the PAT leaves the org — the symptom is the same in both cases: the scheduled workflow fails at the push step with `protected branch` or `permission denied`.
The fix is to rotate the credential (see the linked runbooks) and, if necessary, re-add the new identity to the ruleset's bypass list at `github.com/<repo>/settings/rules`.

## Why this is operational debt, honestly

Every workflow that bypasses branch protection is a workflow that depends on a high-privilege secret.
That secret has to be rotated, scoped tightly, and audited periodically.
Two such secrets is a manageable number.
Five would not be.

Where possible, new workflows that need to land changes on `main` should open a PR and let [Jinx]({{% relref "/global-team/jinx" %}}) auto-approve or auto-merge it under the App's identity rather than mint a token with bypass.
That keeps the "who can write to `main` outside of review" list short.

## Settings review

When walking the org owner / admin review (see [Org membership]({{% relref "org-membership" %}})), also check the branch protection rules on the core repos for drift — particularly that admin bypass hasn't been silently extended to other workflows, and that no new "Allow specified actors to bypass" entries have crept in.
