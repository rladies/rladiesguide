---
title: "Reviewing translated content"
menuTitle: "Translation reviews"
weight: 8
---

If you speak Spanish, Portuguese, or French — or you are happy to translate from English into one of those — the site needs you.

The English content is reviewed and translated.
The Spanish, Portuguese, and French copies are mostly machine-generated placeholders waiting for human review.
A reviewed page becomes the page that ships in production once the language is enabled there; an unreviewed one stays auto-translated and shows the orange banner explaining as much.

This page is the workflow for finding a page, fixing the auto-translation, and getting your work merged.

## What we are looking for

A good review is more than swapping words.

Accuracy of meaning, not literal translation.
The English source is sometimes idiomatic.
A literal translation can sound awkward or miss the point.
Translate the intent, not the letters.

Clarity.
The translated text should read naturally in the target language and be grammatically correct.
If a sentence is clearer in two short sentences than in one long one, write two short sentences.

Inclusivity and gender neutrality.
RLadies+ values inclusive language.
Where the target language has gender-neutral options that fit the prose, prefer them.
Spanish in particular has active community work on inclusive forms — follow the conventions the existing translated content already uses.

Cultural fit.
A reference to a holiday, a meme, an idiom, or a sports analogy that lands in English may not in another language.
Substitute, footnote, or rewrite as needed.
The goal is the reader feels at home, not that the translation is identical.

Code samples.
Function names, arguments, and core programming keywords (`function`, `if`, `else`, `library`, etc.) stay in English — they are part of the programming language, not the natural language.
Comments inside code blocks should be translated.
Variable names can be translated where it improves understanding for speakers of the target language without making the code harder to read.

## Step 1: Find a page to review

Local preview is the fastest way to find auto-translated pages.

```bash
git clone https://github.com/rladies/rladies.github.io.git
cd rladies.github.io
hugo server
```

Open <http://localhost:1313/>, switch to your language using the footer language picker, and browse.
Pages with the orange "auto-translated" banner are the ones waiting for review.

Alternatively, the live preview branch hosts a snapshot.
The preview URL is shared in PR comments and in `#team-website` whenever a translation review pass is in progress.
Pick a page nobody else is working on and start.

If you want to coordinate, the [translation tracking project](https://github.com/orgs/rladies/projects/11) is where the team tracks who has claimed which page.
Comment on a page's tracking issue to say you are working on it.

## Step 2: Edit the file

The page lives at `content/<section>/<slug>/index.<lang>.md`.
For example, the Spanish copy of the FAQ is `content/about-us/faq/index.es.md`.

Open it.
The front matter at the top has `translated: auto` — that is what triggers the banner.
The body below is a copy of the English source.

Replace the body with your translation.
Translate the relevant front-matter fields too — `title`, `description`, `summary`, anything reader-facing.
Do not translate `date`, `directory_id`, image filenames, or other technical fields.

Once the page is fully translated and you are confident in the work, remove the `translated: auto` line.
That promotes the page to a reviewed translation; the banner will not show.

If you only got partway through and want to commit progress, leave `translated: auto` in place — the banner is honest about the state.

## Step 3: Working with code blocks

Inside fenced code blocks, the rule is conservative.
Function names, arguments, library calls, and language keywords stay in English.
Comments translate.
Variable names can translate if it genuinely helps a reader of your language understand the code, but only if they were generic to begin with — never translate `df` to a Spanish synonym just because.

A walked-through example:

````markdown
The English source:

```r
# Filter the dataset to only include rows where age > 18
adults <- df |> filter(age > 18)
```

A good Spanish translation:

```r
# Filtrar el conjunto de datos para incluir solo filas donde edad > 18
adults <- df |> filter(age > 18)
```
````

The comment becomes Spanish.
`filter`, `>`, the pipe, the variable names — unchanged.
The code still runs.

## Step 4: Commit and PR

```bash
git checkout -b translate-faq-spanish
git add content/about-us/faq/index.es.md
git commit -m "Translate FAQ to Spanish"
git push --set-upstream origin translate-faq-spanish
```

Open a PR.
Tag [@rladies/translation](https://github.com/orgs/rladies/teams/translation).

The i18n coverage check will run and confirm the file changes are coherent.
The build check will confirm Hugo can render the page.
A reviewer who speaks the language will go through the prose; if the team can find a second native speaker for a final pass, they will.
Suggestions come back through GitHub code review.

## Reviewing several pages at once

Stack the edits on a single branch.
Each commit can be one file — that keeps the diff easy to review — but they all land in one PR.
Once you are done with the batch, push and open the PR.

Do not split a multi-page batch into multiple PRs unless they are unrelated topics.
The reviewer's overhead is per-PR, not per-file.

## When something is unclear

[Open an issue](https://github.com/rladies/rladies.github.io/issues) describing what is unclear, or ask in `#team-translation`.
Translation reviewers are themselves a small community within RLadies+, and the answer is often two messages away.
