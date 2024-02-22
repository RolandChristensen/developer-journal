---
title: "GitHub Projects"
date: 2024-01-22
---

Notes taken setting up a project to use to track my learning objectives. 

Sample Projects:
* Career Development
* Make it Stick
* Portfolio

---------  All of the above should be left out of the actual post  -----------------

I prefer a project board to visualize work over a traditional "to do list" and calendar/reminders. There is a reason modern software development has evolved to use a project board.  
Using boards for everything from goal management to family chores is effective to organize yourself and your team. A hand drawn task board with sticky notes around the home office and on the refrigerator could be good.

Being in technology, I created a task board for my career development and add issues to it for:
* Learning the business rules for the product being developed for my current project
* Reading the documentation of my current project
* Reading the code of my current project
* Learning new innovative technology
* Keeping up to date with newer versions of the old technologies I use


# GitHub Docs > GitHub Issues > Projects (Free version)
https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects

# Create New Personal User Project
1. Click Profile Picture
2. Click "Your Profile" in menu
3. Click "Projects" in the list of tabs near the top of the page
     * This will list any projects you currently have created
4. Click "New Project" button
5. Select a project template or "start from scratch" and select either **Table**, **Board**, or **Roadmap**
     * Choosing a template will not commit you yet. Click the tile and you will see an example of what it looks like
     * I am going to use *Kanban* to begin
6. Choose project name
7. Click "Create Project" button

## Kanban Divergence 
Kanban is Japanese for "Visual Signal".  
Kanban lets you see your work clearly so you and the team can all stay on the same page.  
Kanban does not use iterations. Kanban expects iterative improvements to happen at their own pace. Your team will find its own cadence.

Bare necessities:
* Kanban Board
* Kanban Cards
* Work in Progress Limit

Basic Board:

|Backlog|Ready|Doing|Blocked|Review|Done|
|-------|-----|-----|-------|------|----|
|Story 1|     |     |       |    |  |

**workflow**: the left to right flow of a story to the **Done** state.  
Each time a card makes it to **Done** you then pick up a new story and pull it into the **Today** (Working/Doing/In Progress/...) column.  
As stories build up in one column or another, you will identify bottlenecks.  
You should also begin to create cards with reasonable sizes. Split up complex tasks into smaller ones that can be moved forward in a reasonable amount of time.  
**Lead Time**: The time taken from the time you start working on it to the time it's done (in production).  
**Cycle Time**: The time from when it is placed into the **Doing** (Today/Working/In Progress/...) column to the time it's done.  

# Describe the Purpose of your Repo
1. Click the **...** (Ellipsis  menu of your project) in the top right corner
2. Click the **Settings** (Gear Icon)
3. Add a description in the **Short Description** field
4. Add a README

# Adding Metadata (Custom Fields) to Your Items
* You can add custom fields (Metadata) to items such as:
    * Date fields
    * Number fields
    * Select fields
    * Text fields
    * Iteration field
* Once defined you can create automations to do things on field changes, via the API, or using GitHub Actions
* Tasklists allow you to create issue hierarchies, sub-tasks, and relations between issues
    * The feature is not available to me that I can see, but I can create markdown check lists to simulate this
* Create *views* to query project and save for future use

# Adding Draft Issues to your project
You can quickly add a **draft issue** to outline work and then go back later to create **issues** from your drafts.
1. In the **Backlog** click the **+ Add item** button
2. Type a name for the draft
3. Press Enter

# Add Issues to your Project
You can add existing **issues** in a repo or create new **issues** that will appear in your repo from the project board.
1. In the **Backlog** click the **+ Add item** button
2. Type "#" and wait for the list of items to appear
3. Select the repository you are using. Wait for the list of work items to appear
4. You can select a work item from the list or create a new **issue**
   * Create a new issue by clicking the **+ Create new issue** button.
   * You can select multiple items by selecting the **Add items from {repo name} ->** button

# Creating a field to track priority
The Kanban template has a priority field out-of-the-box, so you just need to select priorities on issues to see the priority swimlanes appear.

## Kanban Swimlane Divergence
Kanban uses priority swimlanes to organize the most important work, from top to bottom.  
Three priorities (low, medium, high or p2, p1, p0) are the usual way to organize your work.  
Medium is the default.  
High is reserved for hot-fixes and projects that have a tight deadline.  
Low is for work that is on the roadmap, but not the immediate destination.

# Grouping Issues by Priority
The Kanban template, by default, groups priorities into swimlanes.  
On the **Priority board** tab click the down arrow to view the properties of the view.  
In the list is the **Group by** property with **Priority** as the grouping.

# Adding a Board Layout
Board layout organizes items by their status, so you can view their progress.  
The Kanban template has a couple tabs that use the **Board** layout by default, "Backlog", and "Priority Board".  
Click the down arrow on the **Backlog** tab to see the **Layout** group.  
There are three layouts **Table**, **Board**, and **Roadmap**.  
    * The **Table** layout is used on the **Team Items** board.
    * Of course, you can see the **Roadmap** layout on the **Roadmap** board.

# Configure Built-in Automation
1. Click the "..." ellipsis menu on your project
2. Select **Workflows** from the menu (A list of default workflows appears)
3. From the list select **Auto-add to project**
4. Click the **Edit** button in the top right
5. Under filters choose the repository you want the workflow to apply to
6. The filters that where automatically added by the Kanban template are good enough
     * The **Automating Projects > Adding Items Automatically** section goes into more detail
     * The default will automatically add pull requests and open bugs onto the board
  
# Best Practices for Projects
* Break down large issues into smaller issues
* Communicate
* Make use of description, README, and status updates
* Use views
* Have a single source of truth
* Use automation
* Use different field types

## Break Down Large Issues into Smaller Issues
Breaking down stories into smaller pieces, rather than combining all work needed into a large one:  

Pros
* Makes the individual issues more manageable
* Allows multiple people to work in parallel
* Makes each pull request easier to review
* Makes the scope of testing more manageable

Cons
* Increases the complexity of managing the issues
* Increases the complexity of coordinating the merging of pull requests (if working on the same repo)
  
To track the smaller issues:  
* Use Task Lists
* Use Milestones
* Use Labels

### Use Task Lists to organize multiple smaller stories
I think this has not yet been enabled for me. It says it is in Beta and I do not have a "+ Add Tasklist" button.

Use task lists to coordinate multiple smaller issues.  
You can however create a check list in an issue description using markdown.  
- [X] Code (This Issue)
- [ ] Documentation (#100) ("#" shorthand syntax creates a link to an issue)
- [ ] Backend work done (#101)
- [ ] Run script to update DB (#103)
- [ ] ...

Markdown example:
```markdown
- [X] Code
- [ ] Documentation
```

You can link to other issues, pull requests, or bugs in your task list using full URLs, or the #{issue-number} shorthand syntax.  
After you type the "#" a list of issues will appear to select from.  

### Use Milestones to organize multiple smaller stories
You can create a **Milestone** (an expected due date) that you can apply to multiple issues and pull requests.  
1. Navigate to repository
2. Click either **Issues** or **Pull Requests**
3. Click **Milestones** button (Confusingly, there is a drop down with the same name, but push the button)
     * The **Milestones** page appears
5. Click **New Milestone** button
6. Give it a Title, Due Date, and Description
7. Click **Save Changes** button

Associate issues and pull requests to the milestones:  
1. Navigate to the **Issues** or **Pull Requests** of a repo
2. Use the checkmarks to select multiple items
3. Click the **Milestone** dropdown (Not the **Milestones** button next to the filters)
4. In the **Filter milestones** you can filter to easily find your milestone
5. Select the milestone you want to apply
     * The milestone will appear underneath the issue or pull request title

From the **Milestones** tab you can view the progress of your **Milestone**  
1. Follow the instructions above to get to your **Milestone**
2. Click on the title of the milestone (link)
The following can be examined from the milestone page:
* Description
* A list of Issues and pull requests associated with the milestone and their status
    * You can prioritize items in the list by grabbing the left-most handle and dragging the issues into an order from top (most important) to bottom
* Due date
* Completion percentage

From the **Roadmap** tab on the Kanban template you can add a marker to see the due date of the **Milestone**
1. Navigate to the **Roadmap** of your project
2. Click the **Markers** dropdown just below the filter row (You can also use the arrow drop down beside the title on the tab)
3. Select **Milestones** from the dropdown
     * The milestones appear with a label in your timeline

### Use Labels to organize multiple smaller stories
Create a new label with a unique identifier to group stories together:  
1. Navigate to the **Issues** or **Pull Requests** of a repo
2. Click the **Labels** button beside the filters (Not the labels dropdown filter)
3. Click **New Label** button
4. Add a name, description, and a color
5. Click **Create label** button
     * The label will appear beside the title of the issue or PR

Add the label to issues:  
1. Navigate to an issue, pull request, or discussion
2. Click the **Labels** button in the right-side bar
3. Select the label(s) to apply to the item

Filter by the label:  
1. Navigate to the **Issues** or **Pull Requests** of a repo
2. Click the **Label** dropdown (Not the **Labels** button by the filters)
3. Select the label to display from the dropdown

## Communicate
Use @mentions to keep them in the loop.  
Assign issues to people so everyone knows their responsibilities.

## Make use of the description, README, and status updates
Clearly describe the project.  
* Describe the project
* Describe the project views and how you use them
* Provide links to additional documentation
* Provide contact information 
    * RACI (Responsible, Accountable, Consulted, Informed)
    * Responsible individuals are responsible for the tasks and deliverables.
        * Developers, Project Managers, Business Analysts, ...
    * Accountable to project deadlines and eventually for project completion (Frequently also Informed.)
        * Product Owners, Key Stakeholders, Business Owners, ...
    * Consulted individuals' opinions are crucial and their guidance is a prerequisite to other project tasks.
        * Legal experts, Cybersecurity experts, Compliance consultants, ...
    * Informed individuals are not consulted during decision-making, but are informed of all project updates. Often they are also accountable for the project.
        * Business owneres, external stakeholders, ...
    * RACI Matrix
* Share project updates

## Use Views
Use project views to see all the angles of the project.  
* Filter by all unstarted items
* Group by priority to see all high priority items
* Sort by ship date to see the earliest target ship date

## Have a Single Source of Truth
Maintain a single field for important data such as target ship dates. Do not put it in descriptions or comments.  
Projects automatically stay up-to-date with GitHub data, such as assignees, milestones, and labels.  

## Use Automation
* Use Built-in Automations
    * Archiving items automatically
    * Adding items automatically
* GitHub Actions: https://docs.github.com/en/actions
* GraphQL API enables you to automate routine project management tasks.

### Using Built-in Automations
Update the **Status** of items based on certain events. 
* Set status to **Todo** when an item is added to your project
* Set status to **Done** when an issue is closed
* When pull requests are merged, their status is set to **Done**
* Automatically archive items when certain criteria is met

There are two built-in automations enabled at project creation
1. When issues are closed their status is set to **Done**
2. When pull requests are merged their status is set to **Done**

To enable or disable built-in workflows
1. Click the elipsis "**...**" menu on your project on the top right
2. Click **Workflows**
3. Under **Default workflows** click the workflow that you want to edit
4. Click the **Edit** button
5. Select whether the workflow should apply to issues, pull requests, or both
6. Under "set value, choose the value that you want to set the status to (Backlog, Ready, In progress, In review, or Done)
7. Save 

### Archiving Items Automatically
There is a 1,200 item limit for projects. Archive to keep that limit from being reached.  
There is a 10,000 total item limit, so you will also want to permanently delete items regularly.  

1. Click the elipsis "**...**" menu on your project on the top right
2. Click **Workflows**
3. Under **Default workflows** click **Auto-archive items**
4. Click **Edit** button on the top right
5. Choose the filter criteria used to automatically archive an item
     * The default filter is `is:issue is:closed updated:<@today-2w`, which will archive issues that are closed for 2 weeks.
     * This says all issues (`is:issue`), will be archived if they are closed (`is:closed`) for 2 weeks (`updated:<@today-2w`)
     * GitHub says this workflow will run every 12 hours
  
### Adding Items Automatically
The free tier limits you to 1. Pro would get you 5.  
You can add a work item when you add something to a repo.  

1. Click the **...** menu in your project
2. Click the **Workflows** menu item
3. Click the **Auto-add to project** tab on the left side bar
4. Click the **Edit** button
5. Select the repo to use
6. Create a filter for the auto-add
     * The default filter is: `is:issue,pr is:open label:bug`
     * This will add a work item to the board when you add a issue or pull request, which is open, and labeled bug.
7. Click **Save**

You can duplicate an auto-add to monitor up to four different repositories.  
1. Click the **...** menu on the **Auto-add to project** tab on the left side bar
2. Click the **Duplicate workflow** button
3. Change the repo
4. Save with the name you enter

### Automating Projects using Actions
Example action: automatically add a pull request to the project board with a **Status** of **Todo** with a **Date Posted** field set to today, when you mark a PR as **Ready for review**  
The workflow is specific to a repository, so each repo in a project will need to be set up.

#### Quickstart for GitHub Actions
1. Go to the root of some repository
2. Create a file with the full path `.guthub/workflows/{github-actions-filename}.yml`
     * The file name will be the name of the Git Action: **GitHub Actions Filename** would be the name on the workflow.
3. Add the contents found in the GitHub quickstart guide:
```yaml
   name: GitHub Actions Demo
   run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
   on: [push]
   jobs:
     Explore-GitHub-Actions:
       runs-on: ubuntu-latest
       steps:
         - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
         - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
         - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
         - name: Check out repository code
           uses: actions/checkout@v4
         - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
         - run: echo "🖥️ The workflow is now ready to test your code on the runner."
         - name: List files in the repository
           run: |
             ls ${{ github.workspace }}
         - run: echo "🍏 This job's status is ${{ job.status }}."
```
4. Commit the file on a new branch and start a pull request

#### Viewing Workflow Results
1. Navigate to the main page of your repository
2. Click **Actions** on the top row of tabs
3. Click the name of the workflow you created in step 2
4. Click the run you want to view `{username} is testing out GitHub Actions` if you did not change it.
5. Click the **Explore GitHub Actions** button
6. Drill down into any step

#### More Starter Workflows
Creating your own Continuous Integration (CI) workflows is easy using the *starter workflows* provided:  
https://github.com/actions/starter-workflows

GitHub analyzes and suggests starter workflows based on the code base.  
* **Continusous Deployment (CD)**: automate seting up environment and deploy built executables to the environment.
* **Security**: Static analysis scanning
* **Continuous Integration (CI)**: small frequent changes, merged, static analysis scanned, built, tests run, and if all gates passed deploy.
* **Automation**: manage pull requests, automatically applying labels based on paths modified, greeting first-time contributors, ...
* **Custom Workflows**: create your own workflows that will be displayed beside the GitHub starter workflows.

Choosing a starter workflow:  
1. Navigate to the main page of a repository
2. Click **Actions** tab from the top row
3. Click **New Workflow** button in the left pane
     * If you don't have any workflows, you will be met with the "Choose a workflow" page
     * Otherwise, clicking the **New Workflow** button takes you to the "Choose a workflow" page
     * The "Choose a workflow" page contains suggested starter workflows based on the contents of the repo
4. Find a starter workflow and click the **Configure** button
     * There is a workflow called "manual" under the **Automation** section that is a simple *hello world* example to get started
6. The workflow *.yml will appear and any documentation associated with the workflow.
7. If the workflow needs secrets (`repo-token: "${{ secrets.GITHUB_TOKEN }}"`), you will need to include the secret in your repository
     1. Navigate to your repository main page
     2. Click **Settings** in the top row of tabs
     3. In the **Security** section on the left side panel, click **Secrets and Variables** dropdown
     4. Select **Actions** from the dropdown menu
     5. On the **Secrets** tab click the **New Repository Secret** button
     6. Type in the name and secret (user / pass) for the secret
     7. Click the **Add Secret** button
8. If you added a secret, change the secret in the *.yml file to match the name you provided in the secret (`repo-token: "${{ secrets.SECRET_NAME }}"`)
9. Read the file and change as desired
10. Commit your changes
11. If you used the "manual" workfow, you will see a **Manual workflow** button on the left side panel
      1. Click the **Manual workflow** button
      2. Click the **Run Workflow** dropdown
      3. Click the **Run workflow** button
           * A completed workflow row will appear, which you can dive down into to see each step performed
        
### Use the GraphQL API to Manage Projects
You can use it to manage a project and in a GitHub Actions workflow.  
https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-api-to-manage-projects

Example: setting a pull request to "ready for review" adds a new task with **Status** field set to **Todo** and the current date is added to a custom **Date Posted** field.  
https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/automating-projects-using-actions  

### Use Different Field Types
Use a single select field to enforce specific values.  
* This makes filters reliable and easy
* You can also group by the specific values easily

# Finding Your Projects
1. Click your profile picture on the top right
2. Click **Projects**

or

1. Click the "Hamburger" menu in the upper left corner of any GitHub page after logging in
2. Click the **Projects** menu option
     * The default view is the **Recently Viewed**, so you may not see old projects there
     * Select **Created by Me** on the left side to see all projects you have created
4. From the **Projects** page you can filter to find one in a long list

### Syntax for Filtering a List of Projects
`is:open`: projects that are open
`is:closed`: duh
`is:template`: marked as template
    * You can create your own templates, much like the built-in templates you are greeted with when you create a new project
`is:private`: private repos
`creator:USERNAME`: GitHub username to find them

Change the sort order
`sort:title-asc`: by title ascending
`sort:updated-asc`: last updated ascending
`sort:updated-desc`: descending
`sort:created-asc` or `sort:created-desc`

## Adding Your Project to a Repository
1. Navigate to the repo
2. Click **Projects**
3. Click **Link a Project**
4. Select the **Project** from the list
     * A Check mark appears next to the linked project

## Finding Projects Linked to a Repository
1. Navigate to the repo
2. Click **Projects**
     * You may need to search for it
3. Click the project name to open it

## For Organizations and Teams
Learn if needed

# Sharing Project Updates
Keep a team up to date with high-level overviews.  
Statuses: "On Track", "At Risk"  
You can also set start dates and target dates.  

Status updates appear on your project's side panel and the project's header with a full list of all updates.

## Add a Status Update
1. Navigate to the project
2. In the top right click the **Add status button** or the "slide out panel" button to open the side panel
3. Click the **Add update** button
4. Use the markdown text panel to add an update
5. Click the **Save update** button
