---
title: YouTube Content Request Airtable Base
menuTitle: "YouTube Requests"
weight: 7
---

This document details the structure and functionality of the "youtube" Airtable base, used to manage requests for adding content to the R-Ladies YouTube channel. It utilizes a single table populated by a single form.

{{<mermaid  align="left">}}
graph TD

    C[Form Submission] --> |submit| B[Submission Table]
    B --> |Add language| B
    B --> F[Notify Slack]

{{< /mermaid >}}

## Data (Tables and Views)

The "youtube" base contains a single table named "submission_table" which stores all the submitted requests for YouTube content.

### submission_table

This table holds all the information submitted through the YouTube content request form.

**Key Fields (Observed):**

- chapter
- meetup_url
- request_type
- channel_desc
- channel_url
- event_desc
- event_title
- event_description
- event_url
- cp_contains
- cp_content
- cp_license
- cp_url
- google_email
- video_url
- language
- other_language
- playlist_title
- playlist_description
- submitter_contact_pref
- submitter_contact_info

**Key Views:**

- No specific key views were identified for this table.

## Interface

This base does not have any custom interfaces.

## Automation

This base has two automations to manage YouTube content requests:

1.  **Add content language:**

    - **Trigger:** When a record in the "submission_table" has its "language" field set to "Other language".
    - **Action:** Updates the record with the new language, making it available to all new submitters.

2.  **Notify Slack:**
    - **Trigger:** When a new form is submitted to the "submission_table".
    - **Action:** Sends a notification message to a Slack channel.

This documentation provides an overview of the "youtube" Airtable base based on the currently available information.
