---
title: Directory Airtable Base
menuTitle: "Directory"
weight: 4
---

This document details the structure and functionality of the "Directory" Airtable base, which serves as the backend for the R-Ladies speaker directory. It utilizes a single table populated by a single form to manage the addition, updating, and deletion of speaker entries.

{{<mermaid  align="left">}}
graph TD

C[Submission Form] --> |form for submissions| A[submissions Table]
A --> |pulls data| D[GitHub Action]

{{< /mermaid >}}

## Data (Tables and Views)

The "Directory" base consists of a single table named "submissions" which stores all the data related to speaker directory entries and requests.

This table is pulled by a GitHub action in the directory repository, which then runs a series of operations munging the data into the necessary format to be displayed on the website.

### submissions Table

This table holds all the submitted information for the speaker directory.

**Key Fields:**

- **Identifier:** A unique identifier for each submission.
- **Created:** The date and time when the submission was created.
- **Status:** A single select field indicating the current status of the submission (e.g., New, Processing, Approved, Rejected).
- **request_type:** A single select field specifying the type of request submitted (e.g., New directory entry, Update directory entry, Delete directory entry).
- **first_name:** The first name of the speaker.

**Key Views:**

- **Grid view:** The default spreadsheet-style view of all submissions.
- **Kanban:** A board view, organized by the "Status" field to track the progress of submissions.
- **Gallery:** A card-based view, which displays speaker information visually if images or relevant attachments are included in the table.

## Form

There is a single form associated with the base, which is what community members use to request directory changes.

## Interface

There are no interfaces in this base.

## Automation

There are no active automations in this base.
