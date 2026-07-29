---
title: Dark mode and theming
weight: 6
menuTitle: Dark mode & theming
---

The site has a dark mode.
It is not a separate stylesheet, not a runtime CSS swap, not a third-party library.
It is a single CSS class on `<html>`, twenty lines of JavaScript loaded inline, and Tailwind's `dark:` variant.
Knowing the few moving parts will make it obvious where to add support for new components and where to fix bugs.

## What the user experiences

A toggle in the navigation switches between light and dark.
The first time a visitor lands on the site, the theme matches their operating system preference (`prefers-color-scheme`).
The first time they click the toggle, their choice is stored in `localStorage` under the key `theme` and used on every subsequent visit.
If the OS preference changes while the user has not yet expressed an explicit preference, the site follows along.

There is no flash of the wrong theme on page load.
This matters because the alternative — a brief flicker of light mode before dark mode kicks in — is the most common visual bug in dark-mode implementations.

## How the no-flash trick works

Open [`partials/head/head.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/partials/head/head.html) and look near the top:

```go-html-template
<!-- Dark mode (load early to prevent flash) -->
{{ $darkmode := resources.Get "js/darkmode.js" | minify }}
<script>{{ $darkmode.Content | safeJS }}</script>
```

The `darkmode.js` file is read by Hugo Pipes, minified, and inlined directly into `<head>` — not loaded as an external script.
That means the script runs before the browser parses the body or applies any styles.
By the time the first paint happens, the `dark` class is already on `<html>` (or already absent), so every CSS rule scoped to `.dark` is applied or skipped from the very first frame.

The script is short.
It reads `localStorage.theme` if set, falls back to `prefers-color-scheme`, applies the matching class, and exposes `window.toggleTheme` for the navigation button to call.
[Source code here](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/js/darkmode.js).

## How a component opts in

Tailwind v4 with the configuration in [`assets/css/main.css`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/css/main.css) defines a custom variant:

```css
@custom-variant dark (&:where(.dark, .dark *));
```

That means anywhere in a template you can write:

```html
<div class="bg-white dark:bg-dark-75 text-dark dark:text-light">
  ...
</div>
```

…and Tailwind will emit the `bg-white` rule unconditionally, plus a `dark:bg-dark-75` rule scoped under `.dark`.
The same applies to text colours, borders, shadows, anything Tailwind has a utility for.

The brand palette in `main.css` defines a complete set of CSS custom properties — `--color-primary`, `--color-surface`, `--color-bg`, `--color-muted`, and so on.
Component CSS uses those tokens rather than hard-coded hex values, which means dark-mode adjustments happen by overriding the token values inside `.dark` rather than by rewriting every component.
The overrides live in [`components/darkmode.css`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/css/components/darkmode.css).

## When you find a component that does not adapt

The pattern to follow:

1. Check whether the component uses brand tokens (`var(--color-surface)`) or a Tailwind utility that already has a `dark:` variant in templates.  
2. If it uses a token, the fix is in `components/darkmode.css`: override that token's value under `.dark` if the existing override is wrong, or add a more specific rule.  
3. If it uses a hardcoded value, replace the hardcoded value with a token. Resist the temptation to add a one-off `.dark .my-component { ... }` rule. The few places that exist to bridge from old hardcoded styles are a debt to pay down, not a pattern to copy.  

Run `npm run build:css` from inside the theme to regenerate `assets/css/vendor/tailwind.css`, then `hugo server` and toggle dark mode to confirm.

## A note on user-reported visual bugs

The repo CLAUDE.md (and the project conventions) say to trust visual-bug reports rather than dismiss them.
That is here for a reason: a dark-mode bug is rarely an illusion.
If a contributor says a card is unreadable in dark mode, it almost certainly is — and the fix is usually a colour token that was forgotten when the component was written.
Inspect the actual computed CSS values, do not eyeball.

## Brand colours

The primary palette is defined as Tailwind theme tokens at the top of `main.css`:

```css
@theme {
  --color-primary: #881ef9;
  --color-primary-light: #a152f8;
  --color-primary-dark: #6b0fd4;
  ...
}
```

Tailwind auto-generates `bg-primary`, `text-primary`, `border-primary`, etc., from those declarations.
You should not hardcode `#881ef9` in a component — `bg-primary` exists for the same reason.

Accent colours (`--color-accent-blue`, `--color-accent-rose`) round out the palette and are used sparingly for variety.
Surface colours (`--color-surface`, `--color-surface-alt`, `--color-bg`) define the panel backgrounds and shift in dark mode.

## Fonts

Two fonts, both self-hosted, both preloaded.

- **[Poppins](https://fonts.google.com/specimen/Poppins)** for body and headings (`var(--font-sans)`).
- **[Inconsolata](https://fonts.google.com/specimen/Inconsolata)** for code (`var(--font-mono)`).

The variable Inconsolata file is used (one woff2 spans the whole weight axis); Poppins ships with five static weights (300, 400, 500, 600, 700).
Critical weights for above-the-fold text (Poppins 400 and 600) are preloaded in `<head>` to avoid a font-swap shift.

If you need to add a font weight that is not currently bundled, add the `@fontsource/poppins` files to `package.json` (`devDependencies` is fine — it is build-only) and update the `sync:fonts` step.
