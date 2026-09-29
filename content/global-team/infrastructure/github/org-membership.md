---
title: "Organisation membership"
linkTitle: "Org membership"
weight: 10
---

The `rladies` GitHub org follows a familiar nonprofit pattern: a small group of leadership members hold the **Owner** role, a larger group of contributors hold **Member**, and almost everything sensitive is gated behind team membership rather than direct role checks.
There is no SSO, no SCIM, no enterprise tier — just the standard Team-plan member list and the org's own teams.

To run any of the admin actions on this page you need the **Owner** role on the `rladies` org.
Member is not enough — Members can't see the people list with role filters, can't invite or remove anyone, and can't view the audit log.
A handful of `gh` commands below assume the [GitHub CLI](https://cli.github.com/) is installed and authenticated as your Owner account (`gh auth status` will tell you which account is active).

## Roles

- **Owners** — full admin on every repo, can change org settings, can manage billing, can invite or remove anyone.
  The convention is that leadership members hold this role, mirroring how leadership has admin access in 1Password and Google Workspace.
  TODO: list current GitHub org owners.
- **Members** — regular contributors, scoped by team membership.
  A Member's effective permissions on any given repo come from the teams they belong to, not from their org role.
- **Outside collaborators** — kept to a minimum.
  Where they exist, they should be on a specific repo for a specific reason, with a note in the relevant runbook.

The runbooks elsewhere in this section refer in places to "someone on the leadership team with org-owner permissions" without naming names — that's deliberate.
The owner list changes with leadership terms and shouldn't be hardcoded into every page that depends on it.

To see who currently holds which role, open `github.com/orgs/rladies/people` and filter by **Role → Owner** (or **Member**, **Outside collaborator**).
The page also shows two-factor status, last activity, and team membership at a glance — useful when running the [Review cadence](#review-cadence) below.

## Teams

The org uses GitHub teams to grant repo permissions in batches rather than per-person.
At least one team is referenced from the existing runbooks: `rladies/global`, which the [`GLOBAL_GHA_PAT`]({{% relref "../github-pat" %}}) uses for membership lookups in `hello.yaml` on `rladies.github.io`.
Other teams exist for chapter onboarding routing, Meetup management, and the website team — these are referenced indirectly by the email templates and onboarding instructions.
TODO: list current teams and what each one controls.

The full team list lives at `github.com/orgs/rladies/teams`.
Click into a team to see its members, the repos it grants access to, and any nested child teams.
From the command line with `gh` authenticated, `gh api orgs/rladies/teams --paginate` returns the same list as JSON — handy for diffing membership over time.

When a workflow needs to check "does this person belong to RLadies+?", the check should go through a team membership lookup, not a hard-coded username list.

## Invitations

New members are invited through the `global-team` repo's onboarding workflows, which use the [`ADMIN_TOKEN`]({{% relref "../github-admin-token" %}}) to act on the org:

- `onboarding-01-invite.yml` sends the GitHub org invitation.
- `onboarding-02-check-invite.yml` polls to see whether the invited person accepted.
- `onboarding-03-create-issue.yml` opens a tracking issue and assigns the new member to the right teams.

The trigger for all three is the chapter onboarding flow described in [Onboarding new chapter organizers]({{% relref "/global-team/onboarding" %}}).

## Offboarding

When someone leaves leadership or a chapter goes dormant, the [`global-team`](https://github.com/rladies/global-team) repo's `offboarding-finalise.yml` workflow removes them from the org teams.
That workflow uses [`GLOBAL_GHA_PAT`]({{% relref "../github-pat" %}}) — `admin:org` is one of the scopes that PAT carries.

What the automation does _not_ do:

- Revoke owner-level access — owners must be demoted to Member before offboarding.
  To demote: open `github.com/orgs/rladies/people`, find the person, click the `⋯` menu next to their name, choose **Change role**, select **Member**, and confirm.
  The offboarding workflow can then remove them like any other member.
- Rotate PATs or SSH keys that the departing person personally created.
  If the person was the account behind [`GLOBAL_GHA_PAT`]({{% relref "../github-pat" %}}) or [`ADMIN_TOKEN`]({{% relref "../github-admin-token" %}}), those tokens need rotating immediately — see the linked runbooks for the rotation steps.
  The [SSH deploy keys]({{% relref "../ssh-deploy-keys" %}}) are not tied to a personal account, so they don't need touching when a person leaves — only when a key itself expires or leaks.
- Remove the person from chapter-level resources that live outside GitHub (Slack, Meetup, email).
  Those have their own offboarding paths.
- Remove them as an outside collaborator on any repo where they were added directly.
  To check, open `github.com/orgs/rladies/outside-collaborators` and remove any entry for the departing person.

## Review cadence

Every 6 to 12 months, walk through the owner list and the team membership of every team that grants repo-write or org-admin permissions.

Concretely, that means opening four pages in order:

1. `github.com/orgs/rladies/people?role=owner` — is everyone on this list still active and still in a role that needs Owner?
2. `github.com/orgs/rladies/teams` — for each team that grants write access, is every member still active?
3. `github.com/orgs/rladies/people/pending_invitations` — any invitations more than a few months old probably won't be accepted; revoke them.
4. `github.com/organizations/rladies/settings/audit-log` — scan the last few months for unexpected `org.add_member`, `team.add_member`, or `repo.access` events.
   Anything that doesn't have an obvious owner gets followed up.

Also check the branch protection rulesets on the core repos for drift while you're here — see [Branch protection settings review]({{% relref "branch-protection#settings-review" %}}).

TODO: confirm who owns this review and how often it runs in practice.
