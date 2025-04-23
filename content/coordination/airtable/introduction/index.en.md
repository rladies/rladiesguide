---
title: Introduction to Airtable
menuTitle: "Introduction"
weight: 1
---

Airtable is a powerful, flexible database tool that combines the simplicity of a spreadsheet with the advanced functionality of a relational database.
It allows users to organize, store, and manage data efficiently while enabling collaboration across teams. Each **base** in Airtable acts as a workspace containing **tables**, which hold structured data similar to spreadsheets.
However, unlike traditional spreadsheets, Airtable supports **linked records**, **custom field types** (such as attachments, checkboxes, and dates), and **automations** to streamline workflows.
Users can also create **forms** to collect data, apply **filters and views** to customize how information is displayed, and set up **automations** to send emails, update records, or trigger actions based on predefined conditions.
Whether managing projects, tracking inventory, or planning events, Airtable provides a user-friendly yet powerful way to structure and interact with data.

The R-Ladies Global team uses Airtable for a lot of internal project tracking, from the Rotating Curation on Bluesky, to Directory management, the Abstract Review programs, as well as other internal processes.
This document is intended for our team members to learn some general tips about Airtable, and more project specific documentation is elsewhere.

A really nice introduction to Airtable [can be found on youtube](https://www.youtube.com/watch?v=Hq3rQpodt58).

## Bases

A **Base** in Airtable is like a relational database.
It holds all your **tables, views, forms, and automations** for a project.
A single base can contain multiple tables, forms and automations.
In R-Ladies global team, we usually will have one base per team, to coordinate team tasks.

## Understanding the Core Components of Airtable

Airtable is built around four main components: **Data, Interface, Forms, and Automations**. Each serves a different purpose in organizing and managing information efficiently.

### Data (Tables, Fields, and Views)

- **What it is:** The foundation of Airtable—where all your raw information is stored.
- **How it works:**
  - Data is organized into **tables**, similar to spreadsheets.
  - Each **table** contains **fields** (columns) that hold different types of data (text, dates, linked records, etc.).
  - Different **views** (grid, calendar, kanban, etc.) allow you to filter, group, and sort the data without changing the underlying records.
  - Views are made to create accessible subsets of the data to come easily back to.
  - Things like **filtering**, **grouping** and **sorting** in Airtable are not seen as dynamic functions to run _on demand_. They are rather seen as tools for creating **views** of the data to come back to over and over.
- **Example:** A **"Curators"** table stores names, contact info, and scheduled dates. You can have a **Grid View** to see all records and a **Calendar View** to visualize the schedule. You can also create a **Grid View** that is only of future curators, with just a couple of necessary fields.

### Interfaces

- **What it is:** A customizable dashboard that provides a visual and interactive way to view and interact with data.
- **How it works:**
  - Interfaces let you create **charts, summaries, and interactive views** to help users navigate data more intuitively.
  - Unlike tables, **interfaces don’t store data** — they present existing data in a user-friendly way.
- **Example:** A **Curator Dashboard** that shows upcoming schedules, curators needing follow-ups, and a quick summary of curator feedback.

### Forms

- **What it is:** A way to **collect new data** from users without giving them direct access to the database.
- **How it works:**
  - Forms are connected to a specific **table** and create new records upon submission.
  - You can customize which fields are shown and add descriptions, but the form fields must match the table structure.
- **Example:** A **Curator Signup Form** where potential curators enter their name, availability, and bio. Once submitted, a new record is added to the **Curators** table.

### Automations

- **What it is:** A tool for automating repetitive tasks in Airtable.
- **How it works:**
  - Automations are triggered by specific **events** (e.g., "When a form is submitted").
  - They can perform **actions** such as sending emails, updating records, creating new records, or linking records together.
- **Example:** An automation that **assigns a curator** when they sign up and **sends them a confirmation email**.

### How They Work Together

- **Forms** collect curator signups and store them in the **Data** table.
- The **Data** table (with views) organizes and tracks curators.
- **Interfaces** provide an overview of scheduled curators and pending tasks.
- **Automations** reduce manual work by sending notifications and updating statuses.

These components together make Airtable a powerful system for organizing and streamlining workflows.

## Data Management

### Tables & Views

Each base has **Tables**, which store data in a structured way.

You can create different **Views** of a table to organize and display data:

- **Filtering** – Show only specific rows based on conditions.
- **Grouping** – Organize data by categories, like status or date.
- **View Types**:
  - **Grid View** (like a spreadsheet)
  - **Kanban** (like Trello)
  - **Calendar** (for date-based data)
  - **Gallery** (for image-based records)

Each table may have many different view, as many as you like.
Views are a great way to organise the underlying data into more manageable sizes for easier management.
There are for example ways to use views when choosing linked records that create more simple data input.

For instance, you might want:

- **Grid view** with all records and all fields
- **Grid view** with only completed records, with unnecessary fields hidden
- **Grid view** with records that are not completed, with unnecessary fields hidden
- **Calendar view** of all **upcoming** records.

### Fields (Columns)

Setting the correct field type in Airtable will enable you to use data in meaningful ways in automations etc.
In addition, special field types will run simple validations in a form, so that only meaningful data can be entered (like the email format makes sure what is entered is an actual email).
Choosing the right field type therefore makes sure the data flow is a smooth as possible.

**Some useful tips:**

- **Use consistent field names** that are computer-friendly (avoid spaces/special characters).
- **Date fields should follow ISO format** (YYYY-MM-DD) for consistency.
  - Airtable by default uses US format (M/D-YY), edit the field and choose ISO format in the field editor.
- **Linked Records** allow connections between tables, avoiding duplicate data.
  - Linked records are the most valuable feature for teams, as it reduces the need to store the same data multiple places.
  - Linked records also allows you to "lookup" or "rollup" values from the linked table to poopulate the current table. This way you can display information from one table in another, while changes will always stay synced.
- **Formula** are special columns where you can run specific operations in a row-wise fashion on your data. Like calculating a date in the future based on a field, or running calculations on data.

## Forms

Airtable **Forms** let users submit data directly into a table.

### How They Work

- Forms are tied to a **specific table**.
- When a form is filled out, a **new record** is added to the table.

### Best Practices

- **Use clear question titles** (they don’t have to match field names).
- Keep field names **short and structured** for better data handling.

## Automations

Airtable **Automations** help reduce manual work by automatically performing actions.

### Triggers (What Starts an Automation?)

There are many types of triggers for automations, you can create extremely complex custom conditions, or run on a schedule.

** Examples**

- When a **record is created or updated**.
- When a **form is submitted**.
- On a **scheduled time** (e.g., every Monday).
- When date in a table field is _a week from now_ and the _email_ field is not empty.

### Actions (What Can Automations Do?)

- **Send emails** (notifications, reminders).
- **Update records** (change a status, update a date, or link to another record)
- **Create new records** (automate data entry).
- **Run script**: allows us to write custom JavaScript to do special actions.

## Final Tips

✅ **Use structured field names** (avoid special characters).  
✅ **Set up filters & views** to organize data effectively.  
✅ **Automate repetitive tasks** to save time.  
✅ **Use forms** for easy data entry without messing up table structure.
