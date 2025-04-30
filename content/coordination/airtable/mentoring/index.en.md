---
title: Mentoring Airtable Base
menuTitle: "Mentoring"
weight: 5
---

This document details the structure and functionality of the "mentoring" Airtable base, used to manage the R-Ladies mentoring program.
It involves two tables, populated by two forms, and includes Slack notifications for new submissions.

{{<mermaid  align="left">}}
graph TD
A[Interest Form submission] --> B[Interest Table]
B --> E[Slack Notification Interest]
D[Feedback Form submission] --> C[Feedback Table]
C --> F[Slack Notification Feedback]

{{< /mermaid >}}

## Data (Tables and Views)

The "mentoring" base contains two tables: "interest" and "feedback".

### interest Table

This table stores information from individuals interested in participating in the mentoring program, either as mentors or mentees.

**Key Fields:**

- **slack_username:** The Slack username of the interested person.
- **type:** Indicates whether the person wants to be a mentor or a mentee.
- **geographic preference:** Captures any geographic preferences for matching.

**Key Views:**

- **Grid view:** The standard table view.
- **Kanban:** A board view, potentially organized by participant type or matching status.

### feedback Table

This table stores feedback related to the mentoring program.

## Interface

This base does not have any custom interfaces.

## Automation

This base has two automations to notify the mentoring team via Slack upon form submissions:

1.  **Slack Notification Interest:**

    - **Trigger:** When the "R-Ladies Chapter Mentoring Program" form is submitted.
    - **Action:** Sends a message to the "#team-mentoring" Slack channel.

2.  **Slack notification Feedback:**
    - **Trigger:** When the "R-Ladies Mentoring Feedback Form" is submitted.
    - **Action:** Sends a message to "#team-mentoring" Slack channel.

```

```
