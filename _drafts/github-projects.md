---
title: "GitHub Projects"
date: 2024-01-22
---

Notes taken setting up a project to uee to track my learning objectives.

Sample Projects:
* Regions Edu
* Make it Stick
* Portfolio

# GitHub Docs > Projects
https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects

Project Management Light (But Being Developed)

Project management concepts are not discussed in the docs. Basic knowledge is a pre-requisite or the willingness to learn independently.

* You can add custom fields (Metadata) to items such as:
    * Date fields
    * Number fields
    * Select fields
    * Text fields
    * Iteration field
* Automations to do things on field changes, via the API, or using GitHub Actions
* Tasklists allow you to create issue hierarchies, sub-tasks, and relations between issues.
* Create *views* to query project and save for future use.

# Create New Personal User Project
1. Click Profile Picture
2. Click "Your Profile" in menu
3. Click "Projects" in the list of tabs near the top of the page.
     * This will list any projects you currently have created.
4. Click "New Project" button
5. Select a project template or "start from scratch" and select either **Table**, **Board**, or **Roadmap**.
     * Choosing a template will not commit you yet. Click the tile and you will see an example of what it looks like.
     * I am going to use *Kanban* to begin. ***Because I use the Kanban template much of the rest of the Quickstart will be useless***
6. Choose project name
7. Click "Create Project" button

## Kanban Divergence 
Kanban is Japanese for "Visual Signal"

Kanban lets you see your work clearly so the team can all stay on the same page.

Bare necessities:
* Kanban Board
* Kanban Cards
* Work in Progress Limit

Basic Board:

|Backlog|Ready|Doing|Blocked|Review|Done|
|-------|-----|-----|-------|------|----|
|Story 1|     |     |       |    |  |

**workflow**: the left to right flow of a story to the **Done** state.  
Each time a card makes it to **Done** you are able to pick up a new story and pull it into the **Today** (Working/Doing/In Progress/...) column.  
As stories build up in one column or another, you will identify bottlenecks.  
You should also begin to create cards with reasonable sizes that can be moved forward in a timely manner.  
**Lead Time**: The time taken from the time you start working on it to the time its done (in production).  

# Describe the Purpose of your Repo
1. Click the **...** (Elipsis menu of your project)
2. Click the **Settings** (Gear Icon)
3. Add a description in the **Short Description** field
4. Add a README

# Adding Draft Issues to your project
1. In the **Backlog** click the **+ Add item** button
2. Start to type a name for the draft
3. Press Enter

# Add issues to your project
1. In the **Backlog** click the **+ Add item** button
2. Type "#" and wait for the list of items to appear.
3. Select the repository you are using. Wait for the list of work items to appear.
4. You can select a work item from the list
   * Create a new issue by clicking the **+ Create new issue** button.
   * You can select multiple items by selecting the **Add items from {repo name} ->** button.

# Add an iteration field
Kanban does not use iterations. Come back to this when you practice scrum.  
Kanban expects iterative improvements to happen at their own time. Your team will find it's own cadence.

# Creating a field to track priority
The Kanban template has a priority field out-of-the-box, so you just need to select priorities on issues to see the priority swimlanes appear.

## Kanban Swimlane Divergence
Kanban uses priority swimlanes to organize the most important work, from top to bottom.  
Three priorites (low, medium, high or p2, p1, p0) are the usual way to organize your work.  
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

# Configure Built-in Automation
1. Click the "..." elipsis menu on your project
2. Select **Workflows** from the menu (A list of default workflows appears)
3. From the list select **Auto-add to project**
4. Click the **Edit** button in the top right
5. Under filters choose the repository you want the workflow to apply to
6. The filters that where automatically added by the Kanban template are good enough
     * The **Automating Projects > Adding Items Automatically** section goes into more detail
     * The default will automatically add pull requests and open bugs onto the board
  
# Best Practices for Projects
## Break Down Large Issues into Smaller Issues
Breaking down stories into smaller pieces:  

Pros
* Makes the individual stories more manageable
* Allows multiple developers to work in parallel
* Makes each pull request easier to review
* Makes the scope of testing manageable

Cons
* Increases the complexity of managing the stories
* Increases the complexity of coordinating the merging of stories
  
To track the issues:  
* Use Task Lists
* Use Milestones
* Use Labels

## Task Lists
I think this has not yet been enabled for me. It says it is in Beta and I do not have a "+ Add Tasklist" button.  

You can however create a check list in an issue description using markdown.  
- [ ] Code
- [ ] Review
- [ ] Test

`- [ ] Code` etc..

You can link to other issues, pull requests, or bugs in your tasklist using full urls, or the #{issue-number} (`#14` => #14) shorthand syntax.  
After you type the "#" a list of issues will appear to select from.  

## Milestones
