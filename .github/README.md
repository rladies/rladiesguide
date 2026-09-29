# CODEOWNERS

This file documents the ownership rules in `.github/CODEOWNERS`.

A page about how a team works is reviewed by that team.
Adding a section to `CODEOWNERS` gets the right people onto a pull request automatically.

The last matching pattern wins, and a match replaces the owners rather than adding to them.
The rules are ordered from broad to narrow: the whole repository, whole areas, then sections run by a single team.

`@rladies/website` owns everything not claimed by a more specific rule, including layouts, configuration, and the theme.
CI and Netlify cover the checks previously reviewed by eye.
`check-build` fails on a Hugo error, `link-check` catches broken links, and `i18n-check` reports missing translations.
`alt-text-check` checks images, and Netlify posts a preview.

## Global team areas

The global team owns `global-team/`, `about/`, `community/`, `coordination/`, `ally/`, and `acknowledgements/`.
Anything in these areas without a more specific owner goes to `@rladies/global`.

## Global team operations

Each listed operation is reviewed by the team that runs it.

## Community programmes

Community programme sections are assigned to the teams responsible for them.

## Conduct and brand

The conduct, branding, and RoCur sections are assigned to their respective teams.

## Organizers and website documentation

The organizer guide has no owning team, so it stays with the catch-all.
Only parts belonging to a specific team are listed.
The blog and multilingual documentation sections are assigned to their respective teams.
