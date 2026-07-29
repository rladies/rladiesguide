---
title: "GitHub Organisation"
linkTitle: "GitHub"
weight: 80
chapter: false
---

The `rladies` organisation on GitHub hosts every public artefact the project ships — the website, the chapter directory, the blog feed, this guide, the bot, the branding files.
It runs on the **GitHub Team** plan, paid for through a leadership member's faculty account, with no SSO and a small group of leadership owners.
This subsection documents how the org itself is wired together: who has which role, what the core repos do, how `main` is protected, and which apps and bots are installed.

If the faculty billing relationship behind the Team plan ever lapses — the account holder leaves, the institution withdraws the perk, the card on file fails — the org drops to the Free plan.
Private repos in the org would become read-only until billing is restored or someone takes over the plan from another faculty or sponsor account.
The shortest path back is to either restore billing on the same account or transfer the plan to a different faculty account holder, both of which are done from `github.com/organizations/rladies/billing`.
This is the single largest single-point-of-failure in the org's day-to-day operations, which is part of the open Team-vs-Free-for-OSS question below.

The individual credentials those workflows depend on each have their own runbook — see [GitHub PAT]({{% relref "../github-pat" %}}), [Admin Token]({{% relref "../github-admin-token" %}}), and [SSH Deploy Keys]({{% relref "../ssh-deploy-keys" %}}).
This section is about the layer above those secrets: the org configuration that makes them necessary in the first place.

## What this section covers

- [Organisation membership]({{% relref "org-membership" %}}) — owners, teams, member invites, offboarding.
- [Core repositories]({{% relref "core-repositories" %}}) — the eight or so repos that matter for day-to-day operations.
- [Branch protection and merge policy]({{% relref "branch-protection" %}}) — what `main` enforces and how automation works around it.
- [Apps and bots]({{% relref "apps-and-bots" %}}) — Jinx, Dependabot, and the deliberately short list of installed integrations.

## A note on credentials

RLadies+ is part-way through a slow migration from personal access tokens to a GitHub App.
[Jinx]({{% relref "/global-team/jinx" %}}) is the App, and new workflows mint short-lived installation tokens through it.
The older workflows still ride on two PATs (`GLOBAL_GHA_PAT` and `ADMIN_TOKEN`) and three SSH deploy keys, all of which are tied to a personal GitHub account.
That's progress, not a finished migration.
Every time someone touches a workflow that still uses a PAT, the right question to ask is whether Jinx could do the job instead.

## Open decision: stay on Team or apply for Free for OSS?

The org currently pays for **GitHub Team** through a faculty account.
GitHub also runs a [Free for OSS Organizations](https://docs.github.com/en/billing/managing-the-plan-for-your-github-account/free-for-open-source-organizations) programme that gives nonprofit open source projects most Team-tier features at no cost.
RLadies+ plausibly qualifies — the org is a nonprofit, the work is community-driven, and the codebases are public — but applying means an extra review process and surrendering the Team plan's billing relationship in exchange for the upgraded one.

The tradeoffs as we understand them:

- **Staying on Team** keeps the current billing arrangement and avoids a procurement detour.
  The cost is real, but it is already absorbed through the faculty account.
  The plan is stable and the feature set is what we already use.
- **Applying for Free for OSS** removes the dependence on one person's faculty billing relationship — which is its own quiet single-point-of-failure — and frees up whatever budget would otherwise go to GitHub.
  The downsides are application overhead, possible scrutiny of nonprofit eligibility, and a non-zero risk of being downgraded later if GitHub changes the programme rules.

This is an open question for leadership.
This page lays out the shape of the decision; it does not make a recommendation.

## Cadence

Org-owner and admin lists should be reviewed every 6 to 12 months — who has owner, who has admin, who has left the project.
Most of this work is already automated through `global-team` onboarding and offboarding workflows (cross-linked from [Org membership]({{% relref "org-membership" %}})), but the human review is what catches the cases the workflows miss.
The literal checklist for that review lives in [Org membership — Review cadence]({{% relref "org-membership#review-cadence" %}}).

TODO: confirm the current cadence in practice and which leadership role owns running the review.
