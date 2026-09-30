---
title: "Adding a chapter"
menuTitle: "Add chapter"
weight: 3
---

A chapter is a JSON file under `data/chapters/` in the website repository.
The chapter list at <https://rladies.org/chapters/>, the world map on the home page, and the per-chapter pages all read from these files.
Adding or updating a chapter means adding or editing one JSON file and opening a PR.

The chapters live in the website repo, not the directory repo.
That is intentional — chapter information is public, the directory is access-controlled, and they are organisationally distinct concerns.

## File naming

The filename is the chapter identifier in `country-state-city.json` form, lowercased, dashes for spaces, no special characters.
This is what becomes the chapter's URL: `data/chapters/usa-new-york-new-york-city.json` becomes `https://rladies.org/chapters/usa-new-york-new-york-city/`.

If the chapter does not have a state or region, drop that segment: `data/chapters/algeria-algiers.json`.

The naming convention exists so two chapters in different states with the same city name (e.g., a Portland in Oregon and a Portland in Maine) get unique files and unique URLs.

## File content

Open [the chapter JSON schema](https://github.com/rladies/rladies.github.io/blob/main/scripts/json_shema/chapter.json) for the canonical fields.
A typical entry:

```json
[
  {
    "urlname": "rladies-Algiers",
    "status": "prospective",
    "country": "Algeria",
    "city": "Algiers",
    "social_media": {
      "meetup": "rladies-Algiers",
      "email": "algiers@rladies.org"
    },
    "organizers": {
      "current": ["Kamila Benadrouche", "Lounas Fairouz", "Zibani Fazia"],
      "former": []
    }
  }
]
```

Fields:

`urlname` — the chapter's identifier on meetup.com. The site uses this to fetch event data from the meetup archive.

`status` — one of `prospective`, `active`, `inactive`. Only `active` chapters appear on the world map.

`country`, `state.region`, `city` — used to position the chapter on the map and group it on the chapters list. The `country` value should match an entry in [the continents data file](https://github.com/rladies/rladies.github.io/blob/main/data/continents.yaml) so the chapter ends up in the right continent group on the chapters list.

`social_media` — see the social media list below. Optional fields can be omitted entirely; do not leave them empty.

`organizers.current` and `organizers.former` — arrays of organiser names. The chapter page renders both lists on a tabbed view. When an organiser retires, move them from `current` to `former` rather than deleting them — see [Updating chapter organisers](/website/organizers/).

## Social media keys

The chapter pages and chapter list render icons for each social media key.
The supported keys:

```
"twitter": "username"
"github": "username"
"instagram": "username"
"youtube": "channel/UCxxxxxxx"   # or "user/yourname"
"tiktok": "username"
"periscope": "username"
"researchgate": "username"
"website": "https://full.url"
"linkedin": "company-or-handle"
"facebook": "groupname"
"orcid": "0000-0000-0000-0000"
"meetup": "rladies-yourchapter"
"mastodon": "@username@server.example"
"bluesky": "handle.bsky.social"
"slack": "https://invite-link"
```

If your chapter uses a network not in this list, ask in `#team-website` first — adding a new key requires updating the social media partial in the theme so it can render the icon and link.

## Adding the chapter image

Each chapter can have an image shown on its page, typically the chapter's logo or a group photo.

The image goes under `assets/chapters/`, with the filename matching the JSON filename minus the extension.
A chapter at `data/chapters/algeria-algiers.json` looks for an image at `assets/chapters/algeria-algiers.png` (or `.jpg`, `.webp`, etc.).
Hugo's image processing pipeline picks up the file automatically — no separate front-matter reference needed.

If the image is missing, the chapter page renders without it.
The build does not fail.

## How to actually do it

The path of least resistance is the GitHub web UI.

1. Go to [the data/chapters folder](https://github.com/rladies/rladies.github.io/tree/main/data/chapters) on GitHub.  
2. Click "Add file" → "Create new file".  
3. Name the file `country-state-city.json` (or `country-city.json` if there is no state).  
4. Paste the JSON template, fill in the fields.  
5. Scroll down, write a commit message, choose "Create a new branch and start a pull request", click "Propose new file".  

The JSON validation action will run on the PR and tell you if anything is wrong with the schema (missing required field, unknown social network, incorrect type).
The build action will run and produce a preview link.

If you prefer to work locally, see [Working with the website](/website/fork-clone-pr/) for the clone-and-PR workflow.

## After it merges

The next production build will include your chapter on the chapters list and the world map.
The first time it appears, the meetup integration will start polling for events using your `urlname`, and any upcoming events will show up on the events page automatically.
