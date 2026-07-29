---
title: "Updating chapter organisers"
menuTitle: "Update chapter organisers"
weight: 4
---

When an organiser steps down from their chapter, two things need to happen.
Their role on meetup.com is changed from organiser to member.
Their entry in the chapter's JSON file is moved from the `current` list to the `former` list.

Both tasks are quick.
The instructions below cover each.

## Task 1: Change the organiser's role on meetup.com

You need to be signed in with the RLadies+ Global meetup account.
Some browsers (Firefox in particular) have intermittently struggled with the Pro Dashboard interface — Chrome usually works.

1. Sign in to <https://meetup.com> with the RLadies+ Global account.  
2. Click "Pro Dashboard" in the top-right.  
3. Search for the meetup group with the retiring organiser; open its page.  
4. In the "Organizers" panel, click "RLadies+ Global and _N_ others" to open the organisers admin page.  
5. Find the organiser, click the `...` next to their name, choose "Change member role".  
6. Select "member" and click "Update role".  

That removes their organiser permissions on meetup.

## Task 2: Move them from `current` to `former` in the chapter JSON

The chapter JSON files live under [`data/chapters/`](https://github.com/rladies/rladies.github.io/tree/main/data/chapters) in the website repo.
The path of least resistance is the GitHub web UI:

1. Find the chapter's JSON file (`country-state-city.json` or `country-city.json`).  
2. Click the pencil icon at the top right of the file view to edit in place.  
3. Move the organiser's name from the `organizers.current` array to the `organizers.former` array.  
4. Scroll down, write a brief commit message ("Retire Jane Doe from RLadies+ Algiers"), choose "Create a new branch and start a pull request", click "Propose changes".  

The JSON validation action will run on the PR.
A team member from `@rladies/website` will review and merge.

If you prefer to work locally, the same change can be made through a clone — see [Working with the website](/website/fork-clone-pr/).

## Why we keep `former` organisers in the file

We could just delete the entry, but the historical record matters.
Each chapter's page lists former organisers as recognition of past contribution, and the names stay attached to the chapter that hosted them.
The `former` field exists for that reason.
Do not delete entries from `current`; move them.

## When the chapter itself goes inactive

If a chapter as a whole is shutting down rather than just losing an organiser, change the chapter's `status` from `active` to `inactive` in the same JSON file.
Inactive chapters drop off the world map and the active chapters list, but the chapter page itself remains so members can still find historical context.
Discuss with the chapters team before doing this — the criteria for marking a chapter inactive (no events for X months, no responsive organisers) are a leadership call, not a website-team call.
