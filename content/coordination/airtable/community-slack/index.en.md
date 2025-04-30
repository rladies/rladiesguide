---
title: Community Slack Invites Airtable Base
menuTitle: "Community Slack Invites"
weight: 9
---

This document details the structure and functionality of the "Community slack invites" Airtable base, used to manage requests for invitations to the R-Ladies community Slack workspace. It utilizes a single table populated by a single form to screen potential members.

{{<mermaid  align="left">}}
graph TD
B[Community Slack Table]

    C[Slack Invite Request Form] --> |submit| B

    C --> E[Send Confirmations]

    E --> |Notify| F[Slack - #team-community_slack]
    E --> |Send| G[Email]

{{< /mermaid >}}

## Data (Tables and Views)

The "Community slack invites" base contains a single table named "Community slack" which stores information about individuals requesting access to the community Slack workspace.

### Community slack

This table holds all the submitted information from the Slack invite request form.

**Key Fields:**

- name
- email
- Status
- What is R-Ladies?
- For whom is this Slack?
- What should and can you do on this Slack?
- What is this Slack not?
- Code of Conduct #1
- Code of Conduct #2
- Reporting
- Gender neutral language
- Received
- Last update

**Key Views:**

- No specific key views were identified for this table.

## Interface

This base does not have any custom interfaces.

## Automation

This base has a single automation:

**Send confirmations:**

- **Trigger:** When a form is submitted (likely the Slack invite request form).
- **Action:** Sends a message to the #team-community_slack channel in Slack, and sends an email (recipient not fully visible).
