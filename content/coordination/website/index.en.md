---
title: Website maintenance
weight: 9
chapter: true
menuTitle: Website maintenance
---

## Maintaining the site

Maintaining the R-Ladies site depends a lot on the team currently in charge and what they want to do with it.
Tasks can range from re-structuring whole sections of the site, to doing code reviews on incoming Pull requests.

### Handling Pull requests

This first section describes how the website team manaages PR and merges into the main website.

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

## Website build action

{{< mermaid >}}
graph TB

A[Checkout repository] --> B[Get Hugo version]
B --> C[Install cURL Headers]
C --> D[Setup R]
D --> E[Setup renv]
E --> F["Populate untranslated pages\n(scripts/missing_translations.R)"]

subgraph Site Data
F --> G["Get directory data\n(rladies/directory)"]
F --> H["Meetup\n(rladies/meetup_archive)"]
F --> I["Get blogs list\n(rladies/awesome-rladies-blogs)"]
G --> J["Clean cloned repos"]
J --> K["Merge chapter and meetup\n(scripts/get_chapters.R)"]
end

H --> J
I --> J
K --> L[Setup Hugo]
L --> M[Build]

M -->|Production| N[Deploy]

M -->|Preview| O[Install netlify cli]
O --> P[Deploy Netlify]
P --> Q["Notify PR about build"]
{{< /mermaid >}}

## Hugo setup

This section contains information about the internal workings of the website.
It should provide an idea of how it works, and how you may contribute to it.

This hugo site is set-up as made possible within the [hugo](https://gohugo.io/) framework.
The following information should give you a general idea of how the website is configured and set-up.

### Configuration

The site is configured using a [config directory](https://gohugo.io/getting-started/configuration/#configuration-directory) rather than a single config-file.
This makes is easier to create large custom configurations but keep the structure clean.

In particular, the config directory makes it easier to keep the menu definitions for the different site languages distinct.

### Content

The content folder has all the content for the website.
Unlike Hugo content made from R-packages, this site does not use `Rmd` as content source.

The content also uses [page bundles](https://gohugo.io/content-management/page-bundles/) to make it easier to keep files organised together neatly.
A bundled page means that the page has all its content in a folder (where the folder name is the page name/slug), an `index.md` which is the main content file and all secondary files (images etc.) are nested within this page.
This makes it possible to use relative paths in the `index.md` to the secondary files, while also keeping a neat file structure.

**Note**
This repo comes with a project `.Rprofile` that has some settings for the website if you use blogdown to work with locally.
Do not alter the project `.Rprofile` but supplement it with your own profile if you like.
The settings in this file makes it possible to work with blogdown (when blogdown starts supporting config-folders).
It is set up so that creating new posts and pages use the correct settings for the website (like knitting to markdown, rather than html, bundling pages etc).

### Data

The site used hugo [data templates](https://gohugo.io/templates/data-templates/#readout) for several purposes.

- Event data
- Chapter information
- Month name translations

Hugo data template source data are found in the `data` folder, where we have chosen to use `.json` files for this purpose.

Of these, only the mentoring program has a file that is updated manually by R-Ladies global team members working with the mentoring program.

The month names are also created by us, but once created should not need changes as long as they are correct. The month name jsons are used when site language is switched, for displaying the correct month names in the chosen language.

### Static

In the static folder, all files that should be globally accessible to the content files can be placed.
Currently, this only contains images.
If a file is specifically used for a single content file, it should be bundled with the page, rather than placed in static.
If is a more general purpose file (like R-ladies logo etc) to be used in multiple files, it is best to place it in static and refer to it by its relative path to the base-url of the page.

```
/images/logo.png  # Looks for the image as placed in static/images/logo.png
images/logo.png   # Looks for the image as relative to the content index.md file
```

### Theme

The theme for the website is an adaptation of the [hugo-initio](https://miguelsimoni.github.io/hugo-initio-site/) theme ,specifically for this website.

The themes custom adaptations include:

- Maps in home and event page made with [amcharts](https://www.amcharts.com/docs/v4/tutorials/)
- Custom directory grid
- Custom global team grid
- Events calendar with [Toast UI calendar](https://ui.toast.com/tui-calendar)
- Multilingual mode
