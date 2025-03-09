---
title: General Airtable guide
menuTitle: "Airtable"
weight: 12
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

## **Understanding the Core Components of Airtable**  

Airtable is built around four main components: **Data, Interface, Forms, and Automations**. Each serves a different purpose in organizing and managing information efficiently.

### Data (Tables, Fields, and Views)
- **What it is:** The foundation of Airtable—where all your raw information is stored.  
- **How it works:**  
  - Data is organized into **tables**, similar to spreadsheets.  
  - Each **table** contains **fields** (columns) that hold different types of data (text, dates, linked records, etc.).  
  - Different **views** (grid, calendar, kanban, etc.) allow you to filter, group, and sort the data without changing the underlying records.  
- **Example:** A **"Curators"** table stores names, contact info, and scheduled dates. You can have a **Grid View** to see all records and a **Calendar View** to visualize the schedule.

### Interfaces
- **What it is:** A customizable dashboard that provides a visual and interactive way to view and interact with data.  
- **How it works:**  
  - Interfaces let you create **charts, summaries, and interactive views** to help users navigate data more intuitively.  
  - Unlike tables, **interfaces don’t store data**—they present existing data in a user-friendly way.  
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

### **How They Work Together**
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

### **How They Work**  
- Forms are tied to a **specific table**.  
- When a form is filled out, a **new record** is added to the table.  

### **Best Practices**  
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


## **Final Tips**  
✅ **Use structured field names** (avoid special characters).  
✅ **Set up filters & views** to organize data effectively.  
✅ **Automate repetitive tasks** to save time.  
✅ **Use forms** for easy data entry without messing up table structure.  


---

# **Tutorial: Creating a project in Airtable**  

## **Goal**  
To understand how a project can be set up in Airtable in a convenient way to track project requests and progress.

## Creating linked tables
We will create three tables:  
1. **projects** – A table to track projects.  
2. **tasks** – A table to list tasks for each project. 

We'll link **tasks** to **projects** so each task is associated with a specific project.  

### **Step 1: Create a New Base**  
1. Open **Airtable** and click **"Add a base"** (or choose an existing base).  
2. Name it **"project_management"**.  


### **Step 2: Create the "projects" Table**  
1. In the base, rename the first table to **"projects"**.  
2. Set up the following columns (fields):  
   - **project_name** (single line text) → *Primary field*  
   - **start_date** (date field, ISO format YYYY-MM-DD)  
   - **status** (single select: "requested", "not_started", "in_progress", "completed", set _default value_ to "requested").  


### **Step 3: Create the "tasks" Table**  
1. Click **"+ Add or Import"** and choose **"Create empty table"**.  
2. Rename it to **"tasks"**.  
3. Set up the following columns:  
   - **task_name** (single line text) → *Primary field*  
   - **due_date** (date field, ISO format YYYY-MM-DD)  
   - **assigned_to** (single line text or collaborator field)  


### **Step 4: Link the "tasks" Table to "projects"**  
1. In the **"tasks"** table, click the **"+"** next to the last column to add a new field.  
2. Choose **"Linked Record"**.  
3. Select **"projects"** as the table to link to.  
4. Name the field **"project"**.  

✅ **Now each task can be assigned to a project!**  


### **Step 5: Add Data**  
1. Go to the **projects** table and add some sample projects:  
   - "website_redesign", "marketing_campaign", "new_product_launch".  
2. Switch to the **tasks** table and add tasks, selecting a **project** from the linked field.  


### **Step 6: View the Linked Data**  
- In the **projects** table, Airtable automatically creates a **tasks field** showing related tasks.  
- Clicking a **project_name** in the **tasks** table opens the linked record.  


## **Bonus: Create a Kanban View for Tasks**  
1. In the **tasks** table, click **"+ Add View"** → Select **Kanban**.  
2. Choose **"project"** as the grouping field.  
3. Now you can visually organize tasks by project!  

### **Bonus: Create a Calendar View for Tasks**  
A **calendar view** helps visualize deadlines and due dates for tasks. Follow these steps to set it up:

1. **Go to the "tasks" table.**  
2. Click **"+ Add View"** (on the left panel).  
3. Select **"Calendar"** as the view type.  
4. Airtable will ask which date field to use—choose **"due_date"**.  
5. Click **"Create"** to generate the calendar view.  

✅ Now, all tasks with a **due_date** will appear in a **calendar format**, making it easy to track deadlines! 🚀

🎉 **That's it! You've successfully linked two tables in Airtable using snake_case field names.** 


## Add team organisation to the project

### **Step 1: Create the "projects" Table**  
1. Go to your Airtable **Base** to update the **"projects"**.  
2. Add the following fields:  
   - **"status"** (Single select: Options → "Not Started", "In Progress", "Completed")  
   - **"assigned"** (Linked Record → Links to "team_members" table)  

### **Step 2: Create the "team_members" Table**  
1. Create another table named **"team_members"**.  
2. Add the following fields:  
   - **"name"** (Single line text)  
   - **"email"** (Email format)  

### **Step 3: Link Team Members to Projects**  
1. In the **"projects"** table, find the **"assigned"** field.  
2. Click the field type and choose **Linked Record**.  
3. Select **"team_members"** as the linked table.  
4. Now, you can assign a team member to each project.  

### **Bonus Step: Create a View to Easily See Which Team Member Has Which Project**  

To make it easy to distinguish which team member is assigned to each project, follow these steps:  

#### **1. Create a New "Team Projects" View**  
1. In the **"projects"** table, click the **"Views"** button in the top-left corner.  
2. Click **"+ Create a Grid View"** and name it **"team_projects_view"**.  

#### **2. Group Projects by Assigned Team Member**  
1. Click the **"Group"** button in the toolbar.  
2. Select **"assigned"** as the grouping field.  
3. Now, projects will be grouped under each assigned team member!  

#### **3. Customize for Clarity**  
- Hide unnecessary fields (e.g., click the **"Hide fields"** button and deselect columns that aren’t needed).  
- Sort by **"due_date"** so upcoming projects appear first.  
- Optionally, color-code projects by **"status"** to visually track progress.  

✅ **Done!** Now, every project can have an assigned team member and a status to track progress. 🚀

## Setting Up a Form for Project Entry in Airtable**  

This guide will walk you through setting up a form in Airtable that allows users to submit new project entries, including assigning a team member.  


### **Step 1: Create a New Form**  
On the top or Airtable, next to the project name, are four tabs:

- Data (where the tables are)
- Automations
- Interfaces
- Forms

### **Step 1: Click on Forms**  

### **Step 2: Add a new form**  
Click on the little `+` sign in the left sidebar to initiate a new form.

### **Step 3: Connect form to table**  
Connect the form to the **projects** table, and give the form a name.
"Request for new team project".

### **Step 4: Add and Configure Fields**  
Airtable will automatically include all fields from the **"projects"** table in the form. You can customize them as follows:  

### **Project Name**  
- Ensure the **"project_name"** field is visible.  
- This field will capture the name of the project.
- Formulate the field as a question, rather than the field name as we see it in the data.  

### **Start Date & Due Date**  
- The fields **"start_date"** and **"due_date"** fields are for internal tracking, and should not be in the form. 
- Delete them from the form.

### **Status**  
- **"status"** is also an internal tracker.  
- If it's a **single select** field, users can choose from options like:  
- Delete if from the form.

### **4. Assigned Team Member**  
-  **"assigned"** is also for internal tracking.  
- Delete if from the form.

### **5. Project Description (Optional)**  
- Add a **"long text"** field for additional details about the project.  
- This helps users provide a brief project summary.  

### **Step 4: Customize Form Settings**  
1. Click on each field to edit its label (if necessary) without changing the field name.  
2. Toggle **"Required"** for fields that must be filled in before submission (e.g., project name, due date).  
3. Reorder the fields by dragging them to match your preferred layout.  

### **Step 5: Share the Form**  
1. Click **"Share form"** at the top.  
2. Copy the **form link** and share it with your team.  


### ** Bonus step **
Try requesting a new project through your own form, and see what happens.

✅ **Done!** Your **New Project Submission Form** is now ready! Users can enter new projects and assign them to team members with ease.

## Automating Random Team Member Assignment & Email Notification in Airtable**  

This guide will help you set up an Airtable automation that does the following:  
✅ Assigns a **random team member** to a new project when a form is submitted.  
✅ Sends an **email notification** to the assigned team member.  

---

### **Step 1: Open the Automations Panel**  
1. Go to your **Airtable base** where the **"projects"** and **"team_users"** tables exist.  
2. Click on **"Automations"** in the top navigation bar.  
3. Click **"+ Create automation"** and rename it to **"Auto-Assign Team Member"**.  

### **Step 2: Set the Trigger (When a New Form is Submitted)**  
1. Click **"Choose a trigger"** and select **"When a form is submitted"**.  
2. Select the **form view** you created for project submissions.  
3. Click **"Test trigger"** to confirm Airtable detects a new submission.  

### **Step 3: Find a Random Team Member**  
1. Click **"+ Add action"** and choose **"Find records"**.  
2. Select the **"team_users"** table.  
3. **Set filter conditions**:  
   - Ensure all active team members are considered (e.g., if you have an "active" field, filter for active members).  
4. Under **"Sort by"**, choose **"Random"** (Airtable will return a random record).  
5. Limit results to **1 record** to select a single random team member.  
6. Click **"Test action"** to confirm a random team member is retrieved.  

### **Step 4: Update the Project with the Assigned Team Member**  
1. Click **"+ Add action"** and select **"Update record"**.  
2. Choose the **"projects"** table.  
3. Under **"Record ID"**, insert the record from the form submission (Step 2).  
4. In the **"assigned"** field, insert the **record ID from the random team member** (Step 3).  
5. Click **"Test action"** to confirm that a team member is assigned.  

### **Step 5: Send an Email Notification to the Assigned Team Member**  
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
   You do this by clicking the `+` button in the message field, and navigating to the information you want to enter.
5. Click **"Test action"** to confirm that an email is sent.  


### **Step 6: Enable the Automation**  
1. Click **"Turn on automation"**.  
2. Submit a test form to ensure everything works!  

Try submitting another request and see if your automation works.

✅ **Done!** Now, every time a new project is submitted, a **random team member** will be assigned and notified automatically. 🎉

