---
title: "SSH Deploy Keys"
linkTitle: "SSH Deploy Keys"
weight: 8
aliases:
  - /organization/tech/ssh-deploy-keys/
---

The website build process needs to clone two private repos — `directory` and `awesome-rladies-blogs` — and push directly to a protected branch.
Regular `GITHUB_TOKEN` permissions don't stretch that far, so we use SSH deploy keys.

## Current keys

Three deploy keys are stored as secrets in `rladies.github.io`:

| Secret name | Deploy key on repo | Purpose | Write access? |
|---|---|---|---|
| `ssh_directoryy_repo` | `rladies/directory` | Clone directory data during website builds | No (read-only) |
| `RLADIES_BLOGS_KEY` | `rladies/awesome-rladies-blogs` | Clone blog feed data during website builds | No (read-only) |
| `push-to-protected` | `rladies/rladies.github.io` | Push Airtable team data updates to the protected `main` branch | Yes (write) |

Yes, `ssh_directoryy_repo` has a typo — two `y`s.
Renaming it would require updating the workflow files that reference it, so we've left it as-is.

## How deploy keys work

A deploy key is an SSH key pair where:

- The _public_ key is added to the target repository under **Settings > Deploy keys**  
- The _private_ key is stored as a secret in the repository that needs access  

Each key grants access to exactly one repo.
This is more secure than a PAT, which grants access to everything the user account can see.

## Creating a new deploy key pair

When a key needs to be rotated or recreated:

1. Generate a new SSH key pair locally:

```bash
ssh-keygen -t ed25519 -C "rladies-deploy-key" -f ./deploy_key -N ""
```

This creates `deploy_key` (private) and `deploy_key.pub` (public).

2. Add the **public** key to the target repository:
   - Go to the target repo's **Settings > Deploy keys > Add deploy key**  
   - Paste the contents of `deploy_key.pub`  
   - Check "Allow write access" only if the key needs to push (currently only `push-to-protected` needs this)  

3. Add the **private** key as a secret in `rladies.github.io`:

```bash
gh secret set <SECRET_NAME> \
  --repo rladies/rladies.github.io \
  < deploy_key
```

4. Delete the local key files — they should not be kept anywhere else:

```bash
rm deploy_key deploy_key.pub
```

## Which workflows use them

The keys are loaded via the `webfactory/ssh-agent` action in these workflows:

- `build-production.yaml` — the scheduled production build (runs every 12 hours)  
- `build-preview.yaml` — preview builds triggered by `directory` or `awesome-rladies-blogs` PRs  
- `global-team.yml` — weekly Global Team data sync from Airtable  

## Troubleshooting

**`Permission denied (publickey)`**
: The private key secret is missing, expired, or doesn't match the public key on the target repo.
Recreate the key pair following the steps above.

**`remote: Repository not found`**
: The deploy key was removed from the target repo.
Re-add the public key under the target repo's **Settings > Deploy keys**.
