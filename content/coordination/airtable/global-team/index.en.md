---
title: Global Team Overview Airtable Base
menuTitle: "Global Team Overview"
weight: 3
---

This document details the structure and functionality of the "Global Team overview" Airtable base, which serves as a central hub for managing information about the R-Ladies Global team members, their roles, and contact details.

{{<mermaid  align="left">}}
graph TD
A[Members Table] -->|linked record| B[Teams Table]
B -->|linked record| D[Repositories Table]
C[Team emails Table] -->|linked record| B
D -->|linked record| B

    A -->|Automation| F[Retire member]
    F -->|creates record| E[Alumni Table]
    F -->|sends notification| G[Slack Channel]

{{< /mermaid >}}

## Data (Tables and Views)

The "Global Team overview" base is organized into several tables, each containing specific fields to manage different aspects of the team.

### Members Table

This table holds information about each individual member of the R-Ladies Global team.
This table is pulled by a GitHub action in the website repository to populate the [Global Team members on the website](https://rladies.org/about-us/global-team/).

**Key Fields:**

- **Name:** The full name of the team member.
- **City / State / Region:** The member's geographical location.
- **Country:** The member's country of residence.
- **GitHub handle:** The member's GitHub username.
- **R-Ladies email:** The member's primary R-Ladies email address.
- **R-Ladies emails forward…:** R-Ladies emails that are forwarded to this person
- **Zoom email:** The member's email address used for Zoom meetings.
- **Team membership:** A linked record field connecting the member to one or more teams in the "Teams" table.
- **\# teams:** A rollup field that counts the number of teams the member is part of.
- **Temporary Teams:** A field for any temporary team assignments.
- **Time Zone (UTC relative):** The member's time zone relative to UTC.
- **photo:** An attachment field for the member's photo.
- **Last Modified:** A system field indicating the last time the record was updated.
- **Start date:** The date when the member joined the global team.
- **Status:** A single select or status field indicating the member's current status (e.g., Active, Retired).
- **History:** A field, potentially a long text or rich text field, for notes or history of team memberships related to the member.
- **organiser_slack:** The member's Slack handle for organiser-related channels.
- **community_slack:** The member's Slack handle for community-related channels.
- **Goes by:** The preferred name or nickname of the member.

**Key Views:**

- **General**
  - **Full table:** Displays all member records and all fields.
  - **Team members:** Grouped to show members organized by their teams.
  - **Gallery:** Presents member records as visual cards, displaying photos and key information.
- **Team memberships**
  - Used in various places around the entire workspace (not just base) so that admins of teams are populated in other bases.
  - **Blog:** Filtered to show members involved with the blog, with relevant fields displayed.
  - **Leadership:** Filtered to display members in leadership positions.
  - **Campaigns:** Focused on members involved in specific campaigns, showing their roles.
  - **RoCur:** Displays members of the Rotating Curation team. Synced to the RoCur Base.

### Teams Table

This table lists the various teams within the R-Ladies Global organization.

**Key Fields:**

- **Team:** The name of the team.
- **Brief responsibilities:** A short description of the team's main tasks.
- **Technical skills:** Lists the technical skills relevant to the team.
- **Weekly time estimate (o …):** An estimate of the weekly time commitment for team members.
- **Current # members:** A rollup or count field showing the current number of members in the team.
- **Desired # members:** The ideal number of members for the team.
- **Team members:** A linked record field connecting to the "Members" table, listing the members in each team.
- **GitHub Repos:** A linked record field connecting to the "Repositories" table, listing the GitHub repositories associated with the team.
- **Seeking members:** A checkbox or single select field indicating if the team is currently looking for new members.
- **Temporary Members:** A field for any temporary members of the team.
- **Last updated:** A system field indicating the last time the team's information was updated.
- **GitHub Team:** The name of the team's GitHub organization or sub-team.
- **Email:** The team's general email address.

**Key Views:**

- **All teams:** Displays all teams and their details.
- **Team Vacancies:** Filtered to show teams that are currently seeking new members.
- **Teams Full:** Filtered to show teams that have reached their desired number of members.

### Team emails Table

This table manages email addresses associated with specific teams.

**Key Fields:**

- **Email:** The email address.
- **First Name:** The first name associated with the email as seen on Google workspace, information on email purpose.
- **Status:** The current status of the email address.
- **Last Sign In:** The last login date (if applicable).
- **Recovery Email:** A recovery email address (if applicable).
- **Teams:** A linked record field connecting the email address to specific teams in the "Teams" table.

### Repositories Table

This table lists the GitHub repositories used by the R-Ladies Global team.

**Key Fields:**

- **Repository:** The name of the repository.
- **url:** The URL of the GitHub repository.
- **Description:** A brief description of the repository's purpose.
- **Teams:** A linked record field connecting the repository to the relevant teams in the "Teams" table.

### Alumni Table

This table stores information about former members of the R-Ladies Global team.
Its setup mimics the Members table, but is generally unpopulated.
When a team member retires, the GitHub Action on the R-Ladies website pulls the data for the retired member, and stores the data in the website repo for posterity, then sends a delete command to Airtable.

**Key Fields:**

- **Name**
- **Country**
- **GitHub handle**
- **R-Ladies email**
- **R-Ladies emails forward…**
- **email_zoom**
- **photo**
- **Start Date**
- **History**
- **End Date:** The date when the member left the global team.

## Interface

The "Global Team overview" base features a user interface with several pages designed to provide a more accessible and organized way to interact with the data.

**Interface Pages:**

- **Overview:** A landing page with a general description of the R-Ladies Global teams and bookmarks to other interface pages and resources.
- **Dashboard:** Provides an overview of team memberships, using visualizations and summaries.
- **Members:** Presents a user-friendly view of all global team members, with key information displayed.
- **Teams Vacancies:** Specifically displays teams that are currently looking for new members. Used for recruiting new members.
- **Add New Member:** Embeds a form to easily add new members directly to the "Members" table. We ask new members to fill this out themselves once they have 1password access, and thus access to Airtable.

## Automation

The base has one identified automation to streamline the process of managing team member status.

**Automation: Retire member**

- **Trigger:** When a record in the **Members** table has its "Status" field changed to "Retired".
- **Actions:**
  1.  A new record is created in the **Alumni** table, automatically transferring the information of the retired member.
  2.  A message is sent to the "Notify Global Team" Slack channel to inform the team about the member's retirement.

```

```
