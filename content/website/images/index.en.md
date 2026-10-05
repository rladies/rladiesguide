---
title: "Images on the website"
menuTitle: "Images"
weight: 6
---

Most pages on [rladies.org](https://rladies.org) can carry an image — used as the page's Open Graph (social-share) preview, as a hero illustration in the layout, or both.
We use one consistent shape for image frontmatter across the whole site, so contributors only need to remember a single pattern.

## The `image` frontmatter

Every image in frontmatter is a YAML map with three keys:

```yaml
image:
  path: my-photo.jpg
  alt: "Short factual description of what the image shows."
  credit: "[Photographer name](https://link-to-source/)"
```

- **`path`** — required. The filename of the image, **relative to the page bundle** (the folder that contains the page's `index.md` or `_index.md`). The image must live alongside the page so it's a Hugo *page resource*.
- **`alt`** — optional but strongly encouraged when the image conveys information. Describe what is *in* the image, not what page it sits on. If the image is purely decorative or you cannot accurately describe it, leave `alt` out — bad alt text is worse than none.
- **`credit`** — optional. Plain string or markdown (links are fine). Used when an image's licence requires attribution, or when you simply want to credit a photographer.

Where the templates render an image, they read these three fields directly. The same shape is used for the OG / Twitter card meta tags in `<head>`.

## Where to put the image file

Put the image **next to the markdown file that uses it**.

For a leaf bundle (e.g. a blog post):

```
content/blog/2026/my-post/
├── index.en.md
└── my-photo.jpg          # referenced as `path: my-photo.jpg`
```

For a section index (`_index.md`), drop the file in the same folder:

```
content/about-us/our-story/
├── _index.en.md
└── og-image.png          # referenced as `path: og-image.png`
```

When a page has multiple images and you want to keep the folder tidy, group them in a subfolder and reference with the relative path:

```
content/_img/                   # home page bundle uses _img/ for tidiness
├── og-image.png
├── women-meeting-1.jpg
└── …
```

```yaml
image:
  path: _img/og-image.png
  alt: "…"
```

The leading underscore on `_img/` tells Hugo not to treat the folder as a section. Files inside are still picked up as page resources.

{{% notice tip %}}
**Translations share the same image.** Page resources are bundle-wide, not per-language, so a single `image:` map works for every `_index.{lang}.md` or `index.{lang}.md` in the same folder. Translate the `alt` (and `credit` if needed) into the page's language.
{{% /notice %}}

## Writing good alt text

Alt text is read aloud by screen readers and shown when an image fails to load. Some practical rules we follow on rladies.org:

- **Describe the image, not the page.** "World map highlighting RLadies+ chapter locations" — not "Our chapters page".
- **Keep it short.** One sentence is plenty. If you find yourself writing a paragraph, the description probably belongs in the page body, not the alt.
- **Skip "image of" / "photo of".** Screen readers already announce that it's an image.
- **Leave it empty when in doubt.** A missing alt is treated as decorative; a generic "image" alt is worse than nothing because it claims to describe but doesn't.

## Open Graph images

Pages display their `image` in their social-share preview (Open Graph + Twitter Card). The site's `<head>` template emits:

- `og:image` and `twitter:image` from `image.path`
- `og:image:alt` and `twitter:image:alt` from `image.alt` (only when set)
- `og:image:width` / `og:image:height` (1200×630, the standard OG aspect)

If a page has no `image` set, the template falls back to:

1. The first image resource it finds in the page bundle (`Resources.ByType "image"`)
2. The site's default OG image — currently the branded RLadies+ logo card

For the social preview to look right, the source image should be **at least 1200×630 pixels**. Hugo will crop and resize it; smaller sources end up grainy.
