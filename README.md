# rladiesguide

<!-- badges: start -->

[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![Netlify Status](https://api.netlify.com/api/v1/badges/5c1de840-3687-4b5f-bbb2-8d65e9cf9728/deploy-status)](https://app.netlify.com/sites/r-ladies-guide/deploys)
[![Zenodo DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10659414.svg)](https://doi.org/10.5281/zenodo.10659414)

<!-- badges: end -->

The goal of rladiesguide is to consolidate RLadies+ Global organisational guidance & wisdom.
It is published at <https://guide.rladies.org>.

This is a [Hugo](https://gohugo.io/) website built with the
[Hugo theme Relearn](https://mcshelby.github.io/hugo-theme-relearn/).

This repo is governed by the [RLadies+ Code of Conduct](https://rladies.org/code-of-conduct/).

## Contributing

**The contribution guidance now lives in the guide itself**, where it is easier to read,
searchable, and available in the same place as everything else we document:

- [Contributing to the guide](https://guide.rladies.org/guide/contributing/) — how to propose a
  change, house style, deploy previews, and building the site locally.
- [Content structure](https://guide.rladies.org/guide/content-structure/) — what each `content/`
  section is intended to house, how pages and images are laid out, front matter, aliases,
  translations, and the shortcodes in use.
- [Communications templates](https://guide.rladies.org/guide/templates/) — the shared text under
  `static/templates/` that both the guide and [jinx](https://github.com/rladies/jinx) read.

**Fast path:** if you know something is missing, wrong or out of date and do not want to wrangle
Git, just [open an issue](https://github.com/rladies/rladiesguide/issues/new) and tell us.
That is a real contribution.

Either way, please add yourself to [`.zenodo.json`](.zenodo.json) so you are credited on the
acknowledgements page and in the next Zenodo release.

### Building locally

The theme is a Git submodule, so clone recursively:

```sh
git clone --recursive https://github.com/rladies/rladiesguide.git
cd rladiesguide
hugo server
```

## Acknowledgements

Thanks to all contributors to RLadies+ guidance, here and in its previous homes.
Thanks to the [R Consortium](https://www.r-consortium.org/) for funding this project.
