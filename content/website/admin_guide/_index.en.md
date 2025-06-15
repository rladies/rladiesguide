---
title: Website Admin Guide
weight: 99
chapter: true
menuTitle: Website Admin Guide
---

## Maintaining the site

Maintaining the R-Ladies site depends a lot on the team currently in charge and what they want to do with it.
Tasks can range from re-structuring whole sections of the site, to doing code reviews on incoming Pull requests.

### Handling Pull requests

This first section describes how the website team manages PR and merges into the main website.

The website has [GitHub branch protection rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule) set up for the main branch, to ensure we don't accidentally break the site.
We also have some GitHub actions that run checks on the PR to ensure the site can be built before we merge.
All PR's, even PR's from other website team members, should be reviewed by another member before merging.
If no other team member is available, ask for a review from leadership.

General rules for merging PRs:

- All checks run on the PR have to **pass** (Have a green check mark) before merging.
  - If they do not have this, and you don't know how to aid the PR author in fixing that, tag another team member for help (or as in the `#team-website` slack channel).
- Always review the files and changes done in files.
  - Make sure to Approve, Request changes or Reject through a [Code Review](https://linearb.io/blog/code-review-on-github).
- Whoever approves the PR, may also merge it.
  - We generally do `Rebase` merges, but if a `squash and merge` is requested, this is also fine to do.

Every PR should be auto-assigned one of the website team members.
It is that members responsibility to review the PR when they are able.
If you have been assigned a PR you are unable to complete (either because it alters code you are unsure of the purpose and consequence of, or because you are generally unavailable for the task currently), it is your responsibility to ensure that another team members can take over the assignment so the PR can be handled.
When in doubt, grab the opinion of more team members, or even other global team members.
