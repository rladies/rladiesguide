---
title: "Communications templates"
linkTitle: "Templates"
weight: 3
---

Text that somebody copies verbatim into an email, a Slack message or a Meetup page lives in **`static/templates/<name>.md`** — one file, rendered wherever it is needed:

```markdown
{{</* template-file "chapter-onboarding-welcome" */>}}
```

**Edit the file under `static/templates/`, never the rendered copy on a page.**
Two things depend on that:

- The same text often appears on more than one page.
  The group description is shown both in the Meetup instructions and on the chapter accounts page; they cannot drift because they read the same file.
- [jinx](https://github.com/rladies/jinx) fetches these files from `https://guide.rladies.org/templates/<name>.md` when it writes to onboarding issues and sends chapter emails.
  Editing the guide updates what the bot says, with no software release.
  A copy pasted into a page is a copy that will quietly diverge from what we actually send.

Fill-in slots use `<<UPPER_SNAKE>>`, for example `<<FIRST_NAME>>`, `<<CITY>>`, `<<MEETUP_URL>>`.
Keep to that spelling: a human filling one in by hand can see what it wants, and jinx substitutes them mechanically.

## Translating a template

A translation sits beside the canonical file, suffixed with its language code:

```text
static/templates/chapter-reactivation.md      the canonical text
static/templates/chapter-reactivation.es.md   Spanish
```

The unsuffixed file stays the canonical one, so jinx's `https://guide.rladies.org/templates/<name>.md` keeps working and a translated file can be added or removed without touching any page.

The shortcode needs no arguments for this.
It opens the block in the language of the page being read and falls back to the canonical text when that translation does not exist, saying so in a short note rather than serving English under a Spanish heading.

Where more than one language exists, the block grows a language switch.
That is deliberate, and not the same thing as the page language: the language a template is *read* in and the language it is *sent* in are separate choices.
An organiser who reads the guide in Spanish may still need to write to a chapter in English.

Translated files are plain copies with the text translated.
Leave the `<<UPPER_SNAKE>>` slots exactly as they are — jinx substitutes them by name, in any language.

If you name a template that does not exist, the site build fails with the name you asked for — it will not quietly render an empty block.

## What is not a template

Not every code block belongs here.
Front matter samples, mermaid diagrams, directory listings and shortcode examples illustrate the prose around them and should stay inline.

The test is simple: **does somebody copy this text into a message?**
If yes, it is a template.
