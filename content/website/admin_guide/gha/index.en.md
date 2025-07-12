---
title: GitHub Actions
weight: 3
menuTitle: GitHub Actions
---

## Website build action

{{< mermaid >}}
graph TB

A[Checkout repository] --> B[Get Hugo version]
B --> C[Install cURL Headers]
C --> D[Setup R]
D --> E[Setup renv]
E --> F["Populate untranslated pages\n(scripts/missing_translations.R)"]

subgraph Site Data
F --> G["Get directory data\n(rladies/directory)"]
F --> H["Meetup\n(rladies/meetup_archive)"]
F --> I["Get blogs list\n(rladies/awesome-rladies-blogs)"]
G --> J["Clean cloned repos"]
J --> K["Merge chapter and meetup\n(scripts/get_chapters.R)"]
end

H --> J
I --> J
K --> L[Setup Hugo]
L --> M[Build]

M -->|Production| N[Deploy]

M -->|Preview| O[Install netlify cli]
O --> P[Deploy Netlify]
P --> Q["Notify PR about build"]
{{< /mermaid >}}

##

{{< mermaid >}}
{{< /mermaid >}}

##

{{< mermaid >}}
{{< /mermaid >}}

##

{{< mermaid >}}
{{< /mermaid >}}

##

{{< mermaid >}}
{{< /mermaid >}}

## Automatic merging of "pending" PRs

Once a post (news or blog) is finished, it should be labelled as "pending" on GitHub.
Any PR with a blog or news post that has the label "pending", will merge automatically into the main branch on the date specified in the content yaml.

{{< mermaid >}}

graph TD

    B[On: workflow_dispatch or daily schedule] --> C[Job: find_pending_prs];

    C --> C1[Step: Checkout Repository];
    C1 --> C2[Step: Find PRs with pending label];
    C2 -- outputs prs and process --> D{Condition: process == 'true'};

    D -- yes --> E[Job: process_prs];

    E --> E1[Step: Checkout Repository];
    E1 --> E2[Step: Process and Merge Qualifying PRs];
    E2 --> E3[Step: Trigger Website build];

    E3 --> F[End];
    D -- no --> F;

{{< /mermaid >}}
