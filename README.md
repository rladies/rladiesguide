# rladiesguide

<!-- badges: start -->
[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![Netlify Status](https://api.netlify.com/api/v1/badges/5c1de840-3687-4b5f-bbb2-8d65e9cf9728/deploy-status)](https://app.netlify.com/sites/r-ladies-guide/deploys)
<!-- badges: end -->

The goal of rladiesguide is to consolidate R-Ladies Global organisational guidance and wisdom.

This is at the moment a Hugo website built with the [Hugo theme learn](https://learn.netlify.app/en/).

This repo is governed by [R-Ladies Code of Conduct](https://rladies.org/code-of-conduct/).

## Contributing with little git/GitHub/Markdown knowledge

Create a GitHub account, then [open an issue](https://github.com/rladies/rladiesguide/issues/new) and tell us what your idea is!

## Contributing with more efforts

### Pre-requisites

* You'll need to know a bit about [git and GitHub](https://happygitwithr.com/), in particular creating branches and pull requests for your changes. We're here to help, open an issue first if you need more help.

* You'll need to be familiar with [Markdown syntax](https://learn.netlify.app/en/cont/markdown/), and maybe, only maybe, with the [shortcodes of the Hugo theme we use](https://learn.netlify.app/en/shortcodes/) (magical shortcuts for formatting).

### How to edit files

Look at the current content of the content/ folder to see where to amend or add a file. 
Each section (about, organizers) has a file called `_index.en.md` that is an intro, and then inside the section subsections are organized into leaf bundles i.e. their own directory with `index.en.md` containing the text, and potentially images.

### How to translate files

Make sure the language is supported. Only English and Spanish are at the moment but open an issue to discuss further potential language.

To translate a file, add a file with the same name minus `.en` that becomes e.g. `.es`.

### How to view edits online

Open a PR and enjoy the preview!

### How to view edits locally

Painful part, but not too hard thanks to binaries: You'll need to install [Hugo](https://gohugo.io/getting-started/installing/), as well as [Go and git because the website uses Go modules](https://gohugo.io/hugo-modules/use-modules/).

From there easier: Then from the directory of the book run `hugo server`.

### Acknowledgements

Thanks to all contributors to R-Ladies guidance, here and in its previous homes.
Thanks to the [R Consortium](https://www.r-consortium.org/) for funding this project.
