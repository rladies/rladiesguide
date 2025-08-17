---
title: Website Admin Guide
weight: 99
chapter: true
menuTitle: Website Admin Guide
---

Maintaining the R-Ladies site depends a lot on the team currently in charge and what they want to do with it.
Tasks can range from re-structuring whole sections of the site, to doing code reviews on incoming Pull requests.

The website repo comes with a project `.Rprofile` that has some settings for the website if you use blogdown to work with locally.
Do not alter the project `.Rprofile` but supplement it with your own profile if you like.
The settings in this file makes it possible to work with blogdown.
It is set up so that creating new posts and pages use the correct settings for the website (like knitting to markdown, rather than html, bundling pages etc).

## The website team

The website team is a group of R-Ladies global members who are responsible for maintaining the R-Ladies website.
The team is responsible for managing the website, reviewing pull requests, and ensuring that the site is up to date and functioning correctly.
The team is also responsible for managing the website's content, including adding new pages, updating existing pages, and ensuring that the site is accessible and user-friendly.


## GitHub Setup

The website team uses GitHub to manage the website repository.
The repository is set up with branch protection rules to ensure that changes to the main branch are reviewed and approved before they are merged.
Noone is allowed to push directly to the main branch, and all changes must be made through pull requests (PRs).
The website team uses GitHub issues to track tasks and bugs, and to communicate with each other about the website.

### Branch Protection Rules

The main branch of the website repository is protected by GitHub branch protection rules.
These rules ensure that:
- All changes to the main branch must be made through pull requests.
- All pull requests must be reviewed and approved by at least one other team member before they can be merged.
- All checks must pass before a pull request can be merged.

The website team is set up as a GitHub team, and all members of the team have admin access to the website repository.
This means that the website team members have access to work on branches of the website repository, and can create pull requests to merge changes into the main branch.
I.e. the website team members **do not** need to fork the repository to work on it, they can work directly on branches of the repository.

### Communication

The website team uses the `#team-website` channel in the R-Ladies Slack workspace for communication.
This channel is used to discuss tasks, bugs, and other issues related to the website in a more informal way.
However, all official tasks and issues should be tracked in GitHub issues.

### Codeowners

The website repository has a [CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners) file in the root directory.
This file specifies the team that is responsible for reviewing pull requests and issues in the repository.

While the website team is responsible for the website, it is not the only team that can review and approve pull requests.
Other R-Ladies global team members can also review and approve pull requests, and are encouraged to do so.
Additionally, certain content is also managed by other teams, to ensure working groups have control over their content.

- **Content Translation**  
  - `/content/` – [@rladies/translation](https://github.com/orgs/rladies/teams/translation)
  - `/i18n/` – [@rladies/translation](https://github.com/orgs/rladies/teams/translation)

- **News and Updates**  
  - `/content/news` – [@rladies/global](https://github.com/orgs/rladies/teams/global)
  - `/content/news` – [@rladies/leadership](https://github.com/orgs/rladies/teams/leadership)

- **Community Blog**  
  - `/content/blog` – [@rladies/blog](https://github.com/orgs/rladies/teams/blog)

- **Code of Conduct**  
  - `/content/coc` – [@rladies/coc](https://github.com/orgs/rladies/teams/coc)

- **Directory**  
  - `/content/directory` – [@rladies/directory](https://github.com/orgs/rladies/teams/directory)

- **Mentoring Activities**  
  - `/content/activities/mentoring` – [@rladies/mentoring](https://github.com/orgs/rladies/teams/mentoring)




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
