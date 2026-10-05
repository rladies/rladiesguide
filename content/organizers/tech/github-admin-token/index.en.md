---
title: "GitHub Admin Token (ADMIN_TOKEN)"
linkTitle: "GitHub Admin Token"
weight: 6
aliases:
  - /organization/tech/github-admin-token/
---

Three R-Ladies repos rely on a secret called `ADMIN_TOKEN` — a GitHub PAT with elevated permissions that lets workflows push to protected branches, invite new org members, and manage teams.
If it expires or the person who created it leaves, several automated processes stop working silently.

## What it does

The `ADMIN_TOKEN` handles operations that the standard `GITHUB_TOKEN` cannot — things that require org-level access or the ability to bypass branch protection.

| Repository | Workflow | What it does |
|---|---|---|
| `rladies.github.io` | `global-team.yml` | Pushes Airtable-sourced team data directly to the protected `main` branch |
| `rladies.github.io` | `merge-pending.yaml` | Auto-merges scheduled blog post PRs when their publish date arrives |
| `directory` | `01-purge-init.yml` | Pushes branch with rewritten git history for GDPR entry deletions |
| `global-team` | `onboarding-01-invite.yml` | Sends GitHub org invitations to new team members |
| `global-team` | `onboarding-02-check-invite.yml` | Checks whether an invited member has accepted |
| `global-team` | `onboarding-03-create-issue.yml` | Adds accepted members to the correct org teams |
| `global-team` | `remind-stale.yml` | Lists and comments on stale onboarding/offboarding issues |
| `global-team` | `actions-status-report.yml` | Reads workflow run status across all org repos |

## How it differs from GLOBAL_GHA_PAT

We maintain two PATs because their permission profiles are different.
`GLOBAL_GHA_PAT` handles cross-repo workflow triggers and comments.
`ADMIN_TOKEN` handles org membership management and pushing to protected branches — operations that need broader privileges.

Keeping them separate means we can rotate one without disrupting the other, and we limit the blast radius if either token leaks.

## Creating a new ADMIN_TOKEN

### Who should create it

Use a GitHub account belonging to someone on the leadership team with org owner permissions.
If that person leaves, the token must be rotated immediately.

### Scopes needed (classic PAT)

Go to [github.com/settings/tokens/new](https://github.com/settings/tokens/new) and select these scopes:

| Scope | Why |
|---|---|
| `repo` | Push to protected branches, read private repos, manage PRs |
| `admin:org` | Invite members, manage team memberships, read org data |
| `workflow` | Trigger and manage GitHub Actions workflows |

Set an expiration of up to one year and add a calendar reminder to rotate before it expires.

### Storing the secret

The `ADMIN_TOKEN` is stored as an org-level secret.
Using the [GitHub CLI](https://cli.github.com/):

```bash
gh secret set ADMIN_TOKEN \
  --org rladies \
  --visibility selected \
  --repos "rladies.github.io,directory,global-team"
```

Paste the token value when prompted.

You can also do this through the web UI at **github.com/organizations/rladies/settings/secrets/actions**.

### Rotation

When the token approaches expiry or the owning account changes:

1. Create a new PAT following the steps above  
2. Update the org secret — this overwrites the old value  
3. Revoke the old token at [github.com/settings/tokens](https://github.com/settings/tokens)  
4. Verify by manually triggering a workflow that uses it — `global-team.yml` in `rladies.github.io` is a good candidate  
