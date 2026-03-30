---
title: "Brand overview"
linkTitle: "Brand overview"
weight: 1
---

## Brand identity

RLadies+ launched its new visual identity in March 2026, designed in collaboration with [Science Graphic Design](https://www.sciencegraphicdesign.com/) and shaped by community feedback.
The full visual identity guide is available as [Branding-guidelines.pdf](https://github.com/rladies/branding-materials/blob/rebrand/Branding-guidelines.pdf) in the [branding-materials repository](https://github.com/rladies/branding-materials).

### Spelling

- **RLadies+** is the preferred form of the name.
- Use `rladies` (no plus) only where special characters are not allowed, such as usernames and account names.

### Color palette

| Role | Color | Hex code | Preview |
|------|-------|----------|---------|
| Main colour | Blue Violet | `#881ef9` | <span style="display:inline-block;width:20px;height:20px;background:#881ef9;border-radius:3px;vertical-align:middle;"></span> |
| Accent | Dodger Blue | `#146af9` | <span style="display:inline-block;width:20px;height:20px;background:#146af9;border-radius:3px;vertical-align:middle;"></span> |
| Accent | Brilliant Rose | `#ff5b92` | <span style="display:inline-block;width:20px;height:20px;background:#ff5b92;border-radius:3px;vertical-align:middle;"></span> |
| Basic | Bastille Black | `#2f2f30` | <span style="display:inline-block;width:20px;height:20px;background:#2f2f30;border-radius:3px;vertical-align:middle;"></span> |
| Basic | Lavender White | `#ededf4` | <span style="display:inline-block;width:20px;height:20px;background:#ededf4;border:1px solid #ccc;border-radius:3px;vertical-align:middle;"></span> |

The main colour (Blue Violet) should be used primarily, combined with the basic colours and/or accent colours.
Accent colours should be used sparingly as supplements to the main colour.
Use Bastille Black and Lavender White instead of pure black (`#000000`) or pure white (`#ffffff`).

Each colour also has 75%, 50%, and 25% tints for extended palette use — see the [brand.yml](https://github.com/rladies/branding-materials/blob/rebrand/brand.yml) for the full set.

### Accessibility

- **Contrast**: Text must always have high contrast with its background. The smaller the text, the higher the contrast needed. Use tools like [colourcontrast.cc](https://colourcontrast.cc) to verify.
- **Text formatting**: Avoid underlined, italic, and all-caps text. Use **bold** for emphasis. Left-align text.
- **Text size**: Never use text smaller than 6 points.
- **Line spacing**: Use at least 1.5 pt line spacing for all text.

### Typography

The official font is **Poppins**, available via [Google Fonts](https://fonts.google.com/specimen/Poppins).

| Element | Weight |
|---------|--------|
| Main titles | Poppins Medium |
| Headings | Poppins SemiBold |
| Body text | Poppins Regular |
| Emphasis | Poppins Bold (not italics or underline) |

The monospace font is **Inconsolata**.

Maintain visual hierarchy: main titles should be the largest text, headings sized according to their level, and body text consistent throughout.

### Logo

The RLadies+ logo is available in multiple variants:

| Variant | Use case |
|---------|----------|
| Vertical (stacked) | Default — use for most cases |
| Horizontal | When vertical space is limited |
| Full colour | Light or white backgrounds |
| Bastille Black | Light backgrounds (single-colour alternative) |
| Lavender White | Dark or coloured backgrounds |
| Pure Black / Pure White | High-contrast contexts |

{{% notice warning %}}
**Do not** edit the logo shapes, move the logo icon relative to its text, recreate the logo using a different font, change the logo's proportions (rotate, skew, stretch), or position the logo on a busy or discontinuous background. Leave enough space around the logo and text.
{{% /notice %}}

The R and + symbols can be customized with images or brand colours — see the [Canva]({{% relref "branding/canva-logo" %}}) and [Affinity]({{% relref "branding/affinity-logo" %}}) guides for instructions.
When customizing, ensure all parts contrast enough with their background.

### Machine-readable brand definition

A [brand.yml](https://github.com/rladies/branding-materials/blob/rebrand/brand.yml) file is available for use with [Quarto](https://quarto.org/docs/authoring/brand.html) and [Shiny](https://shiny.posit.co/) projects.
It contains the full colour palette (including tints), typography settings, and logo definitions.

## Brand assets

All official assets are in the [branding-materials repository](https://github.com/rladies/branding-materials):

| Folder | Contents |
|--------|----------|
| `logos/horizontal/` | Horizontal logo in 5 colour variants |
| `logos/vertical/` | Vertical (stacked) logo in 5 colour variants |
| `logos/favicon/` | Favicon icons |
| `logos/hex/` | Hex sticker logos (4 variants) |
| `logos/social-media/` | Social media profile pictures |
| `logos/circular/` | Circular logos (not for social media) |
| `logos/quarto/` | Clean-named SVGs for Quarto and web projects |
| `templates/slidedecks/` | PowerPoint and Google Slides templates |
| `templates/documents/` | Document templates (with and without letterheads) |
| `templates/certificates/` | Certificate templates (DOCX and PDF) |

Additional editable templates are available on [Google Drive](https://drive.google.com/drive/folders/1UV940p-KN9FWoHt4yrZnrc2oAczGw9QB?usp=sharing) and in the [RLadies+ Canva workspace](https://www.canva.com/folder/FAHCPq0DH3w).

## Tools and extensions

| Tool | Description |
|------|-------------|
| [glamour](https://github.com/rladies/glamour) | Quarto extension — applies RLadies+ branding to documents and presentations |
| [spellbind](https://github.com/rladies/spellbind) | R package with brand colours ([documentation](https://rladies.org/spellbind/)) |
| [cloak](https://github.com/rladies/cloak) | pkgdown theme for R package websites |
| [brand.yml](https://github.com/rladies/branding-materials/blob/rebrand/brand.yml) | Machine-readable brand definition for Quarto and Shiny |

## Legacy materials

Pre-2026 R-Ladies branding (purple `#88398a`, gray `#a7a9ac`, Open Sans font) is archived in the [legacy/](https://github.com/rladies/branding-materials/tree/rebrand/legacy) folder of the branding-materials repository.
