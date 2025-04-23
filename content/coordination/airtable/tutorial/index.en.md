---
title: Creating a project in Airtable
menuTitle: "Tutorial"
weight: 2
---

## Goal

To understand how a project can be set up in Airtable in a convenient way to track project requests and progress.

## Creating linked tables

We will create three tables:

1. **projects** – A table to track projects.
2. **tasks** – A table to list tasks for each project.

We'll link **tasks** to **projects** so each task is associated with a specific project.

### Step 1: Create a New Base

1. Open **Airtable** and click **"Add a base"** (or choose an existing base).
2. Name it **"project_management"**.

### Step 2: Create the "projects" Table

1. In the base, rename the first table to **"projects"**.
2. Set up the following columns (fields):
   - **project_name** (single line text) → _Primary field_
   - **start_date** (date field, ISO format YYYY-MM-DD)
   - **status** (single select: "requested", "not_started", "in_progress", "completed", set \_default value* to "requested").
### Step 3: Create the "tasks" Table

1. Click **"+ Add or Import"** and choose **"Create empty table"**.
2. Rename it to **"tasks"**.
3. Set up the following columns:
   - **task_name** (single line text) → _Primary field_
   - **due_date** (date field, ISO format YYYY-MM-DD)
   - **assigned_to** (single line text or collaborator field)

### Step 4: Link the "tasks" Table to "projects"

1. In the **"tasks"** table, click the **"+"** next to the last column to add a new field.
2. Choose **"Linked Record"**.
3. Select **"projects"** as the table to link to.
4. Name the field **"project"**.

✅ **Now each task can be assigned to a project!**

### Step 5: Add Data

1. Go to the **projects** table and add some sample projects:
   - "website_redesign", "marketing_campaign", "new_product_launch".
2. Switch to the **tasks** table and add tasks, selecting a **project** from the linked field.

### Step 6: View the Linked Data

- In the **projects** table, Airtable automatically creates a **tasks field** showing related tasks.
- Clicking a **project_name** in the **tasks** table opens the linked record.

## Bonus: Create a Kanban View for Tasks

1. In the **tasks** table, click **"+ Add View"** → Select **Kanban**.
2. Choose **"project"** as the grouping field.
3. Now you can visually organize tasks by project!

### Bonus: Create a Calendar View for Tasks

A **calendar view** helps visualize deadlines and due dates for tasks. Follow these steps to set it up:

1. **Go to the "tasks" table.**
2. Click **"+ Add View"** (on the left panel).
3. Select **"Calendar"** as the view type.
4. Airtable will ask which date field to use—choose **"due_date"**.
5. Click **"Create"** to generate the calendar view.

✅ Now, all tasks with a **due_date** will appear in a **calendar format**, making it easy to track deadlines! 🚀

🎉 **That's it! You've successfully linked two tables in Airtable using snake_case field names.**

## Add team organisation to the project

### Step 1: Create the "projects" Table

1. Go to your Airtable **Base** to update the **"projects"**.
2. Add the following fields:
   - **"status"** (Single select: Options → "Not Started", "In Progress", "Completed")
   - **"assigned"** (Linked Record → Links to "team_members" table)

### Step 2: Create the "team_members" Table

1. Create another table named **"team_members"**.
2. Add the following fields:
   - **"name"** (Single line text)
   - **"email"** (Email format)

### Step 3: Link Team Members to Projects

1. In the **"projects"** table, find the **"assigned"** field.
2. Click the field type and choose **Linked Record**.
3. Select **"team_members"** as the linked table.
4. Now, you can assign a team member to each project.

### Bonus Step: Create a View to Easily See Which Team Member Has Which Project

To make it easy to distinguish which team member is assigned to each project, follow these steps:

#### 1. Create a New "Team Projects" View

1. In the **"projects"** table, click the **"Views"** button in the top-left corner.
2. Click **"+ Create a Grid View"** and name it **"team_projects_view"**.

#### 2. Group Projects by Assigned Team Member

1. Click the **"Group"** button in the toolbar.
2. Select **"assigned"** as the grouping field.
3. Now, projects will be grouped under each assigned team member!

#### 3. Customize for Clarity

- Hide unnecessary fields (e.g., click the **"Hide fields"** button and deselect columns that aren’t needed).
- Sort by **"due_date"** so upcoming projects appear first.
- Optionally, color-code projects by **"status"** to visually track progress.

✅ **Done!** Now, every project can have an assigned team member and a status to track progress. 🚀

## Setting Up a Form for Project Entry in Airtable\*\*

This guide will walk you through setting up a form in Airtable that allows users to submit new project entries, including assigning a team member.

### Step 1: Create a New Form

On the top or Airtable, next to the project name, are four tabs:

- Data (where the tables are)
- Automations
- Interfaces
- Forms

### Step 2: Add a new form

Click on the little `+` sign in the left sidebar to initiate a new form.

### Step 3: Connect form to table

Connect the form to the **projects** table, and give the form a name.
"Request for new team project".

### Step 4: Add and Configure Fields

Airtable will automatically include all fields from the **"projects"** table in the form. You can customize them as follows:

### Project Name

- Ensure the **"project_name"** field is visible.
- This field will capture the name of the project.
- Formulate the field as a question, rather than the field name as we see it in the data.

### Start Date & Due Date

- The fields **"start_date"** and **"due_date"** fields are for internal tracking, and should not be in the form.
- Delete them from the form.

### Status

- **"status"** is also an internal tracker.
- If it's a **single select** field, users can choose from options like:
- Delete if from the form.

### 4. Assigned Team Member

- **"assigned"** is also for internal tracking.
- Delete if from the form.

### 5. Project Description (Optional)

- Add a **"long text"** field for additional details about the project.
- This helps users provide a brief project summary.

### Step 4: Customize Form Settings

1. Click on each field to edit its label (if necessary) without changing the field name.
2. Toggle **"Required"** for fields that must be filled in before submission (e.g., project name, due date).
3. Reorder the fields by dragging them to match your preferred layout.

### Step 5: Share the Form

1. Click **"Share form"** at the top.
2. Copy the **form link** and share it with your team.

### Bonus step

Try requesting a new project through your own form, and see what happens.

✅ **Done!** Your **New Project Submission Form** is now ready! Users can enter new projects and assign them to team members with ease.

## Automating Random Team Member Assignment & Email Notification in Airtable\*\*

This guide will help you set up an Airtable automation that does the following:  
✅ Assigns a **random team member** to a new project when a form is submitted.  
✅ Sends an **email notification** to the assigned team member.

### Step 1: Open the Automations Panel

1. Go to your **Airtable base** where the **"projects"** and **"team_members"** tables exist.
3. Click **"+ Create automation"** and rename it to **"Auto-Assign Team Member"**.

### Step 2: Set the Trigger (When a New Form is Submitted)

1. Click **"Choose a trigger"** and select **"When a form is submitted"**.
2. Select the **form view** you created for project submissions.
3. Click **"Test trigger"** to confirm Airtable detects a new submission.

### Step 3: Find a Random Team Member

1. Click **"+ Add action"** and choose **"Find records"**.
2. Select the **"team_users"** table.
3. **Set filter conditions**:
   - Ensure all active team members are considered (e.g., if you have an "active" field, filter for active members).
4. Under **"Sort by"**, choose **"Random"** (Airtable will return a random record).
5. Limit results to **1 record** to select a single random team member.
6. Click **"Test action"** to confirm a random team member is retrieved.

### Step 4: Update the Project with the Assigned Team Member

1. Click **"+ Add action"** and select **"Update record"**.
2. Choose the **"projects"** table.
3. Under **"Record ID"**, insert the record from the form submission (Step 2).
4. In the **"assigned"** field, insert the **record ID from the random team member** (Step 3).
5. Click **"Test action"** to confirm that a team member is assigned.

### Step 5: Send an Email Notification to the Assigned Team Member

1. Click **"+ Add action"** and select **"Send an email"**.
2. In the **"To"** field, insert the email of the assigned team member from Step 3.
3. **Customize the subject**:
   - _"You’ve been assigned a new project!"_
4. **Customize the message**:

   ```
   Hello {team_member_name},

   You have been assigned a new project: "{project_name}".

   Start Date: {start_date}
   Due Date: {due_date}
   Status: {status}

   Please check Airtable for details.

   Thanks,
   The Team
   ```

   Where you see the `{}` you should enter dynamic input from the data.
   You do this by clicking the blue `+` button in the message field, and navigating to the information you want to enter.

5. Click **"Test action"** to confirm that an email is sent.

### Step 6: Enable the Automation

1. Click **"Turn on automation"**.
2. Submit a test form to ensure everything works!

Try submitting another request and see if your automation works.

✅ **Done!** Now, every time a new project is submitted, a **random team member** will be assigned and notified automatically. 🎉
