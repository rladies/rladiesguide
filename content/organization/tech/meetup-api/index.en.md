---
title: "Meetup API Credentials"
linkTitle: "Meetup API"
weight: 9
---

The `meetup_archive` repo connects to the Meetup.com API every 12 hours to archive chapter and event data.
It uses JWT-based authentication, which requires three secrets.

## Secrets

| Secret | What it is |
|---|---|
| `JWT_TOKEN` | A JWT signing key (RSA private key) registered with the Meetup API |
| `JWT_ISSUER` | The Meetup user ID associated with the API application |
| `CLIENT_KEY` | The OAuth client key from the Meetup API application |

These are stored as repo-level secrets on `rladies/meetup_archive`.

## How authentication works

The [meetupr](https://github.com/rladies/meetupr) R package handles the authentication flow:

1. It signs a JWT using `JWT_TOKEN` and `JWT_ISSUER`  
2. It exchanges that JWT for an OAuth access token using `CLIENT_KEY`  
3. It uses the access token to call the Meetup GraphQL API  

This is tied to the R-Ladies Meetup Pro account, which has access to all R-Ladies chapter data.

## Creating new credentials

If the credentials need to be regenerated:

1. Log in to [meetup.com](https://www.meetup.com) with the R-Ladies Pro account  
2. Go to the [API applications page](https://www.meetup.com/api/oauth/list/)  
3. Either edit the existing application or create a new one  
4. Generate a new RSA key pair for JWT signing if needed  
5. Note the Client Key and Member ID (used as `JWT_ISSUER`)  

Then update the secrets:

```bash
gh secret set JWT_TOKEN --repo rladies/meetup_archive < private_key.pem
gh secret set JWT_ISSUER --repo rladies/meetup_archive --body "<meetup-user-id>"
gh secret set CLIENT_KEY --repo rladies/meetup_archive --body "<client-key>"
```

## Troubleshooting

**`401 Unauthorized` in archive workflow**
: The JWT token or client key has expired or been revoked.
Regenerate the credentials following the steps above.

**Data is incomplete or chapters are missing**
: Check that the Meetup Pro account still has admin access to the R-Ladies Pro network.
Chapters removed from the Pro network won't appear in API responses.
