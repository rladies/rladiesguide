# rladiesguide

<!-- badges: start -->

[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![Netlify Status](https://api.netlify.com/api/v1/badges/5c1de840-3687-4b5f-bbb2-8d65e9cf9728/deploy-status)](https://app.netlify.com/sites/r-ladies-guide/deploys)
[![Zenodo DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10659414.svg)](https://doi.org/10.5281/zenodo.10659414)

<!-- badges: end -->

The goal of rladiesguide is to consolidate RLadies+ Global organisational guidance & wisdom.

This is at the moment a Hugo website built with the [Hugo theme re-learn](https://mcshelby.github.io/hugo-theme-relearn/).

This repo is governed by [RLadies+ Code of Conduct](https://rladies.org/code-of-conduct/).

## Contributing with little git/GitHub/Markdown knowledge

Create a GitHub account, then [open an issue](https://github.com/rladies/rladiesguide/issues/new) and tell us what your idea is!

## Contributing with more efforts

### Pre-requisites

- You'll need to know a bit about [git and GitHub](https://happygitwithr.com/), in particular creating branches and pull requests for your changes. We're here to help, open an issue first if you need more help.

- You'll need to be familiar with [Markdown syntax](https://learn.netlify.app/en/cont/markdown/), and maybe, only maybe, with the [shortcodes of the Hugo theme we use](https://learn.netlify.app/en/shortcodes/) (magical shortcuts for formatting).

### How to edit files

Look at the current content of the content/ folder to see where to amend or add a file.
Each section (about, organizers) has a file called `_index.en.md` that is an intro, and then inside the section subsections are organized into leaf bundles i.e. their own directory with `index.en.md` containing the text, and potentially images.

### How to add or edit a communications template

Text that somebody copies verbatim into an email, a Slack message or a Meetup
page lives in **`static/templates/<name>.md`** — one file, rendered wherever it
is needed:

```
{{< template-file "chapter-onboarding-welcome" >}}
```

**Edit the file under `static/templates/`, never the rendered copy on a page.**
Two things depend on that:

- The same text often appears on more than one page. The group description is
  shown both in the Meetup instructions and on the chapter accounts page; they
  cannot drift because they read the same file.
- [jinx](https://github.com/rladies/jinx) fetches these files from
  `https://guide.rladies.org/templates/<name>.md` when it writes to onboarding
  issues and sends chapter emails. Editing the guide updates what the bot says,
  with no software release. A copy pasted into a page is a copy that will
  quietly diverge from what we actually send.

Fill-in slots use `<<UPPER_SNAKE>>`, for example `<<FIRST_NAME>>`, `<<CITY>>`,
`<<MEETUP_URL>>`. Keep to that spelling: a human filling one in by hand can see
what it wants, and jinx substitutes them mechanically.

If you name a template that does not exist, the site build fails with the name
you asked for — it will not quietly render an empty block.

Not every code block belongs here. Front matter samples, mermaid diagrams,
directory listings and shortcode examples illustrate the prose around them and
should stay inline. The test is simple: **does somebody copy this text into a
message?** If yes, it is a template.

### How to translate files

Make sure the language is supported. Only English and Spanish are at the moment but open an issue to discuss further potential language.

To translate a file, add a file with the same name minus `.en` that becomes e.g. `.es`.

### How to view edits online

Open a PR and enjoy the preview!

### How to view edits locally

Painful part, but not too hard thanks to binaries: You'll need to install [Hugo](https://gohugo.io/getting-started/installing/), and download the repo with its submodules (where the theme is).

```sh
git clone --recursive https://github.com/rladies/rladiesguide.git
```

From there easier: Then from the directory of the book run `hugo server`.

### Acknowledgements

Thanks to all contributors to RLadies+ guidance, here and in its previous homes.
Thanks to the [R Consortium](https://www.r-consortium.org/) for funding this project.
