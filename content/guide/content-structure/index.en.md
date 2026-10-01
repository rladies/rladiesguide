---
title: "Content structure"
linkTitle: "Content structure"
weight: 1
---

Every page in this guide lives under `content/`, in one of a small number of top-level sections.
Each section has an audience and a remit.
Knowing which is which is the difference between a page people find and a page people trip over.

## What each section is for

The sections are listed here in the order they appear in the sidebar menu.

| Section | URL | Who it is for | What it houses | Example of a page that belongs here |
|---|---|---|---|---|
| `about` | `/about/` | Anyone | What RLadies+ is and the rules everyone agrees to: mission, code of conduct, who the Global Team are, partner programmes | "What our code of conduct covers" |
| `community` | `/community/` | Anyone, and volunteers running a cross-chapter programme | Programmes and initiatives that run across the whole community rather than inside one chapter: the Slack, International Women's Day, conference participation, abstract review, the R Contributors working group, the member directory | "How abstract review works" |
| `organizers` | `/organizers/` | Chapter organisers | Running a chapter: the rules chapters agree to, starting up, planning and running events, chapter tech and accounts, online presence, reusable resources | "How to run a hybrid meetup" |
| `global-team` | `/global-team/` | Global Team volunteers | The coordination machinery behind the community, most of which needs an account or a permission an organiser does not have: Airtable, finances, Meetup Pro administration, chapter monitoring, chapter onboarding, social media management, the directory, code of conduct handling, and the jinx bot | "How to add a chapter to Meetup Pro" |
| `rocur` | `/rocur/` | WeAreRLadies curators and the volunteers who administer the rota | The rotating curation account: what it is, the curator guide, the admin guide, and the FAQ | "What a curator posts in their week" |
| `branding` | `/branding/` | Anyone making something that carries the RLadies+ name | Visual identity and the files that implement it: palette, typography, accessibility rules, logo and hex-logo sources, Canva and Affinity templates, slide decks | "Making a chapter hex logo" |
| `website` | `/website/` | Website contributors, and the website team | The RLadies+ global website at [rladies.org](https://rladies.org): the pages directly under `/website/` are the contributor-facing how-to, and `/website/admin_guide/` is how the site is actually built and deployed | "Adding a blog post to the website" |
| `guide` | `/guide/` | Anyone contributing to this guide | This guide itself: content structure, how to contribute, and the shared communications templates | The page you are reading |
| `ally` | `/ally/` | Anyone | Nothing new. It is a stub that points at [Get Involved](https://rladies.org/about-us/get-involved/) on the website, kept so old links still resolve | Nothing — write ally material on the website instead |

Two more directories under `content/` are not sections in this sense:

- `content/_index.en.md` is the front page of the guide.
- `content/acknowledgements/` is generated, not written. `_index.en.qmd` is rendered weekly by the `render_acknowledgements` workflow from `.zenodo.json`, which overwrites `_index.en.md`. Edit `.zenodo.json`, never the rendered file.

`content/comm/` and `content/coordination/` are leftovers from an earlier layout, kept because `community` and `global-team` alias their old URLs.
Do not add new pages to either.

## Choosing a section

Two questions settle almost every case.

**Who needs this to do their job?**
A chapter organiser, a Global Team volunteer, or anyone at all.
That picks between `organizers`, `global-team`, and `about`/`community`.

**Does it need an account or permission most people do not have?**
If yes, it is `global-team` material even when it is about chapters.
"How to onboard a new chapter" is Global Team work; "how to run your first event" is organiser work.

If a page would genuinely fit in two sections, put it where its audience will look for it and link to it from the other.
Duplicating a page is how two versions of the truth get born.

## How content files are laid out

Content is organised as [Hugo page bundles](https://gohugo.io/content-management/page-bundles/).

- A **leaf bundle** is a directory containing `index.en.md` — one page, plus any images it uses, sitting together.
- A **branch bundle** is a directory containing `_index.en.md` — a section or subsection, whose `_index.en.md` is the intro page shown above the list of its children.

So a new page in the organiser section is `content/organizers/my-new-page/index.en.md`, and a new group of related pages is `content/organizers/my-new-group/_index.en.md` plus a leaf bundle per page inside it.

Images live in the bundle next to the page that uses them and are referenced by filename alone, with no path:

```markdown
![RLadies+ organigram](RladiesStructure.png)
```

That is what makes a bundle movable: rename or move the directory and the image reference still works.
Always write real alt text — the repo has a CI check that flags images added without it.

### Front matter

These are the fields actually in use in this repo.

| Field | What it does |
|---|---|
| `title` | The page title, used as the heading and in breadcrumbs |
| `linkTitle` / `menuTitle` | A shorter label for the sidebar menu when the title is long. `linkTitle` is the more common one here |
| `weight` | Sort order among siblings — low numbers first. Sections use 1 to 8; `ally` uses 200 to sit at the bottom |
| `chapter` | `true` on the `_index.en.md` of a top-level section, which makes the Relearn theme render it as a section landing page |
| `aliases` | Old URLs this page should also answer at |
| `build` | Hugo build options. Only `content/acknowledgements/` uses this, to keep the generated file out of the menu |

A typical leaf page:

```yaml
---
title: "How to run a hybrid meetup"
linkTitle: "Hybrid meetups"
weight: 20
---
```

### Keeping old URLs alive with `aliases`

When a page moves or a section is renamed, add the old path to `aliases` on the page that replaces it:

```yaml
---
title: Global Team
aliases:
  - /coordination/
---
```

Hugo then publishes a redirect at `/coordination/` pointing at the new location.
This guide has been reorganised more than once and old links are in people's bookmarks, in Slack, and in issues, so moving a page without an alias quietly breaks them.

## Translations

Two languages are configured in `config.toml`, under `[Languages]`: English (`en`, the default) and Spanish (`es`).

A translation sits beside the original with the language code swapped in the filename:

```text
content/organizers/rules/index.en.md    English
content/organizers/rules/index.es.md    Spanish
```

The repo runs an i18n completeness check on every pull request that touches `content/`, `i18n/`, or `config.toml`.
It compares the `en` and `es` trees and reports what is missing.
Adding an English-only page is normal and allowed — the check tells us what still needs translating, it is not a gate that demands you translate your own page.

Interface strings — menu labels, button text, anything the theme shows rather than the content — live in `i18n/en.toml` and `i18n/es.toml`.

If you want to add a third language, open an issue first.
A language needs people willing to keep it current, not just a first pass.

## Shortcodes in use

Most pages are plain Markdown.
These are the shortcodes genuinely used in this repo; the [Relearn theme's shortcode reference](https://mcshelby.github.io/hugo-theme-relearn/shortcodes/) has the rest if you need something unusual.

**Notices**, for a callout that should not be read as ordinary prose:

```markdown
{{%/* notice warning */%}}
Chapters must not charge for events.
{{%/* /notice */%}}
```

`info`, `tip`, `note` and `warning` are all in use.

**`relref`**, for linking to another page in the guide:

```markdown
See the [branding overview]({{%/* relref "branding" */%}}) for the palette.
```

Use `relref` rather than a hand-written path.
It resolves at build time, so a link to a page that does not exist fails the build instead of shipping a 404.

**`figure`**, where an image needs a caption:

```markdown
{{</* figure src="claim-host.png" alt="The claim host button in the Zoom interface" caption="Claiming host" */>}}
```

**`mermaid`**, for diagrams:

```markdown
{{</* mermaid */>}}
graph LR
  A[Issue] --> B[Pull request]
{{</* /mermaid */>}}
```

**`template-file`**, which is local to this repo rather than part of the theme.
It pulls in a shared communications template — see [Communications templates]({{% relref "guide/templates" %}}).
