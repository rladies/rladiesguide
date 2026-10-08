---
title: "Contributing to the guide"
linkTitle: "Contributing"
weight: 2
---

This guide is written by volunteers, which means you.
There are two paths in, and the first one is a perfectly good contribution.

## The low-effort path: open an issue

If you know something is missing, wrong, or out of date and you do not want to wrangle Git, that is fine.
[Open an issue](https://github.com/rladies/rladiesguide/issues/new) and say what you know.
Someone with write access will write it up.

Telling us what is wrong is genuinely useful work.
A guide nobody corrects becomes a guide nobody trusts.

## The higher-effort path: a branch and a pull request

### What you need to know

- A little [Git and GitHub](https://happygitwithr.com/) — enough to make a branch and open a pull request. If you are not there yet, open an issue and ask; we would rather walk you through it than lose the contribution.
- [Markdown](https://www.markdownguide.org/basic-syntax/). That is nearly all of it.
- Occasionally a theme shortcode, which are listed in [Content structure]({{% relref "guide/content-structure" %}}).

### Where to put the file

Read [Content structure]({{% relref "guide/content-structure" %}}) first.
It has a table of what each top-level section is intended to house and how the files in a section are laid out.

Getting the section right matters more than getting the prose right.
Prose gets edited; a page filed in the wrong section stays lost.

### House style

- One sentence per line. It makes diffs readable and review comments precise.
- Write **RLadies+**. Not "R-Ladies", not "RLadies". Use `rladies` only where a plus sign is not allowed, such as usernames.
- Real alt text on every image, describing what the image shows. A CI check flags images added without it.
- No bare URLs as link text — link the words that say where the link goes.

### Preview your change

Every pull request gets a Netlify deploy preview.
A bot comments the preview URL on the PR once the build finishes, so you can read your page as it will be published before anyone reviews it.
A pull request also runs a Hugo build check, an alt-text check, a link check, and the i18n completeness check.

### Build it locally

Install [Hugo](https://gohugo.io/getting-started/installing/) and clone the repo **recursively** — the theme is a Git submodule, and a plain clone leaves you with an empty `themes/` directory and a build that fails:

```sh
git clone --recursive https://github.com/rladies/rladiesguide.git
cd rladiesguide
hugo server
```

If you already cloned without `--recursive`:

```sh
git submodule update --init --recursive
```

`hugo server` serves the site at <http://localhost:1313> and rebuilds as you save.

## Credit for your contribution

Add yourself to [`.zenodo.json`](https://github.com/rladies/rladiesguide/blob/main/.zenodo.json) in the same pull request.
The welcome bot reminds you of this on your first issue or pull request, and it is not a formality:

- The guide's acknowledgements page is regenerated weekly from `.zenodo.json`, so that file is the only thing that puts your name on it.
- `.zenodo.json` is also the author list on the guide's [Zenodo record](https://doi.org/10.5281/zenodo.10659414), which means a citable, permanent credit for the work.

An ORCID is optional but welcome; it links your name to the rest of your work.
A CI check validates `.zenodo.json`, so a malformed entry is caught before merge rather than after.

## Reviewing and merging

Pull requests are reviewed by the Global Team.
Be patient with us — we are all volunteers.
A nudge on the PR after a week is entirely reasonable.

Merges to `main` deploy to <https://guide.rladies.org> automatically.
