---
title: "Airtable API Key"
linkTitle: "Airtable"
weight: 7
aliases:
  - /organization/tech/airtable/
---

R-Ladies uses Airtable as the backend for directory submissions and Global Team membership data.
Two repos connect to Airtable through a shared API key stored as `AIRTABLE_API_KEY`.

## Airtable bases

We maintain separate Airtable bases for different purposes:

| Base ID | Purpose | Key tables | Used by |
|---|---|---|---|
| `appzYxePUruG9Nwyg` | Directory submissions | Submissions, Languages, Countries | `directory` |
| `appZjaV7eM0Y9FsHZ` | Global Team | Members, Teams, Alumni | `rladies.github.io` |

The `directory` repo pulls new member submissions every Friday via `airtable-update.yml` and deletes processed records after PRs merge via `airtable-delete.yml`.

The website repo pulls Global Team data weekly via `global-team.yml` to render the team page.

## How the API key works

Both repos use the [airtabler](https://github.com/bergant/airtabler) R package, which reads the key from the `AIRTABLE_API_KEY` environment variable.

The key is a _personal access token_ generated from an Airtable account — not the deprecated API key format.
Make sure the account that generates the token has at least Editor access on both bases listed above.

## Creating a new token

1. Log in to [airtable.com](https://airtable.com) with the R-Ladies Airtable account  
2. Go to [airtable.com/create/tokens](https://airtable.com/create/tokens)  
3. Create a new personal access token with these scopes:  
   - `data.records:read` — read records from tables  
   - `data.records:write` — create and update records  
   - `schema.bases:read` — read base schema (needed by `airtabler`)  
4. Under "Access", add both bases: the directory base and the Global Team base  
5. Copy the generated token  

## Storing the secret

The token is stored as an org-level secret:

```bash
gh secret set AIRTABLE_API_KEY \
  --org rladies \
  --visibility selected \
  --repos "directory,rladies.github.io"
```

Or through the web UI at **github.com/organizations/rladies/settings/secrets/actions**.

## Troubleshooting

**`AUTHENTICATION_REQUIRED` or `INVALID_PERMISSIONS_OR_MODEL_NOT_FOUND`**
: The token has expired or lacks the required scopes.
Generate a new one following the steps above.

**`TABLE_NOT_FOUND`**
: The token doesn't have access to the specific base.
Edit the token at [airtable.com/create/tokens](https://airtable.com/create/tokens) and add the missing base.

**Workflows succeed but no new entries appear**
: Check whether there are actually new submissions in the Airtable base.
The `airtable-update.yml` workflow only creates a PR when new or updated records exist.
