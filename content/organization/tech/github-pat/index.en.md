---
title: "GitHub Personal Access Token (GLOBAL_GHA_PAT)"
linkTitle: "GitHub PAT"
weight: 5
---

Several R-Ladies workflows need to reach _across_ repositories — triggering a website preview build from the directory repo, posting a comment back on a PR in a different repo, or checking whether someone belongs to an org team.
The standard `GITHUB_TOKEN` can't do any of this.
That's what `GLOBAL_GHA_PAT` is for.

## Which repositories use it

| Repository | Workflow | What the PAT does |
|---|---|---|
| `directory` | `airtable-update.yml` | Triggers a website preview build on `rladies.github.io` |
| `directory` | `retrigger-website.yml` | Re-triggers a website preview build on `rladies.github.io` |
| `awesome-rladies-blogs` | `trigger-website.yaml` | Triggers a website preview build on `rladies.github.io` |
| `rladies.github.io` | `hello.yaml` | Checks whether a PR/issue author belongs to the `rladies/global` team |
| `rladies.github.io` | `build-preview.yaml` | Posts build status comments to the originating repo and downloads artifacts from `directory` |
| `global-team` | `offboarding-finalise.yml` | Removes departing members from organization teams |

## Creating a new PAT

### Who should create it

Use a GitHub account belonging to someone on the leadership team who is unlikely to leave soon.
If that person leaves, the PAT must be rotated — see [Rotating the token](#rotating-the-token) below.

{{% notice warning %}}
The PAT is tied to a _personal_ GitHub account, not the organization.
When that person's account loses org access, every workflow that depends on this token breaks.
{{% /notice %}}

### Fine-grained PAT (preferred)

Go to [github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new).

**Basic settings:**

| Field | Value |
|---|---|
| **Token name** | `rladies-org-global-pat` |
| **Expiration** | Up to 1 year — set a calendar reminder to rotate before it expires |
| **Resource owner** | `rladies` |

**Repository access** — select "Only select repositories" and add:

- `rladies/directory`  
- `rladies/rladies.github.io`  
- `rladies/awesome-rladies-blogs`  
- `rladies/global-team`  

**Repository permissions:**

| Permission | Access level |
|---|---|
| **Actions** | Read and write |
| **Contents** | Read and write |
| **Issues** | Read and write |
| **Pull requests** | Read and write |
| **Metadata** | Read (auto-selected) |

**Organization permissions:**

| Permission | Access level |
|---|---|
| **Members** | Read and write |

{{% notice note %}}
If the org requires approval for fine-grained PATs, the token enters a _pending_ state after creation.
An org owner must approve it at **github.com/organizations/rladies/settings/personal-access-tokens/active**.
Once approved, go back to **github.com/settings/personal-access-tokens** to copy the token value.
{{% /notice %}}

### Classic PAT (fallback)

If fine-grained PATs are not enabled for the org, create a classic token at [github.com/settings/tokens/new](https://github.com/settings/tokens/new) with these scopes:

| Scope | Why |
|---|---|
| `repo` | Cross-repo comments, repository dispatch, artifact downloads |
| `workflow` | Trigger `workflow_dispatch` events across repos |
| `admin:org` | Manage org team memberships (offboarding) |
| `read:org` | Check org team membership |

## Storing the secret

The PAT is stored as an org-level Actions secret scoped to specific repositories.
Using the [GitHub CLI](https://cli.github.com/):

```bash
gh secret set GLOBAL_GHA_PAT \
  --org rladies \
  --visibility selected \
  --repos "directory,rladies.github.io,awesome-rladies-blogs,global-team"
```

Paste the token value when prompted.

You can also do this through the web UI at **github.com/organizations/rladies/settings/secrets/actions** — click "New organization secret", name it `GLOBAL_GHA_PAT`, paste the value, and select the four repositories above.

## Rotating the token

When the token is approaching expiry or the person who created it is leaving:

1. Create a new PAT following the steps above  
2. Update the org secret — this overwrites the old value  
3. Revoke the old token at [github.com/settings/personal-access-tokens](https://github.com/settings/personal-access-tokens)  
4. Verify by checking that a `directory` or `awesome-rladies-blogs` PR can trigger a preview build on `rladies.github.io`  

## Troubleshooting

**`HTTP 403: Resource not accessible by personal access token`**
: The token has expired or is missing a required scope.
Create a new one following this guide.

**Fine-grained PAT shows as "Pending"**
: An org owner needs to approve it at **github.com/organizations/rladies/settings/personal-access-tokens/active**.

**Workflows fail only on cross-repo steps**
: The secret might be missing from one of the four repos.
Re-run the `gh secret set` command above to ensure all repos are included.
