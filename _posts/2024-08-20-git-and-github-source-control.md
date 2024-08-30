---
title: "Git and GitHub Source Control"
date: 2024-08-20
---

Source control is essential for programming and Git combined with GitHub is a good solution.  
Use this page as a quick reference for often used scenarios.

To make it stick in your mind, create your own tutorials and quizzes.  
Use this [repository](https://github.com/RolandChristensen/git-guided-examples) to create practice exercises and quizzes.  
The repo will go into more detail on source control topics and give you a safe place to practice.  
If you find yourself in trouble and can't figure out how to undo a mistake, this repo is also a good place to reproduce the problem and learn to undo it safely.

This documents Git using the command line, but the skills outlined can be reproduced by other tools you use to interface with Git.  
I use Visual Studio and Visual Studio Code extensions for Git, but still go to the command line frequently and create scripts which demand knowledge of the command line.

## Quick Start Scenarios
Below are common scenarios used in day-to-day development.  

[Repo Creation](#repo-creation)  
* [Create New Repo](#create-new-repo)
* [Clone a Repo](#clone-a-repo)
* [Starting a Git Repo with Existing Code](#starting-a-git-repo-with-existing-code)
* [Git Fork](#git-fork)

[The Three States](#the-three-states)  
* [Git Status](#git-status)
* [Git Add](#git-add)
* [Git Commit](#git-commit)
* [Git Log](#git-log)
* [Git Reset](#git-reset)
* [Git Restore](#git-restore)
* [Git RM](#git-rm)

[Avoiding Merge Conflicts](#avoiding-merge-conflicts)  

[Developing a New Feature](#developing-a-new-feature)
* [Start with a Clean Working Tree](#start-with-a-clean-working-tree)
* [Sync your Local Repository with the Remote](#sync-your-local-repository-with-the-remote)
* [Creating a feature branch](#creating-a-feature-branch)
* [Development Source Control Process](#development-source-control-process)
* [Push Branch to the Remote](#push-branch-to-the-remote)
* [Create a Pull Request](#create-a-pull-request)
* [Modifying Pull Request](#modifying-pull-request)
* [Merge Pull Request](#merge-pull-request)

[Double Check Your Work](#double-check-your-work)
* [Git Diff Before Staging](#git-diff-before-staging)
* [Git Diff Between Staged Files and Last Commit](#git-diff-between-staged-files-and-last-commit)
* [Git Log to View Commits on Feature Branch since it was Checked Out](#git-log-to-view-commits-on-feature-branch-since-it-was-checked-out)

[Renaming a Branch](#renaming-a-branch)  

[Merge Branches](#merge-branches)
* [How to Tell if a Branch has been Merged](#how-to-tell-if-a-branch-has-been-merged)

[Hotfix Forces You to Stop Working on a Feature](#hotfix-forces-you-to-stop-working-on-a-feature)
* [Git Stash](#git-stash)

[Algorithm Comparison Testing Using Branches](#algorithm-comparison-testing-using-branches)



## Repo Creation
For an in depth tutorial, see the "***create-repo.md***" file in this [repository](https://github.com/RolandChristensen/git-guided-examples).  
I will go through two ways to create a repo.  
1. Create a new *blank canvas* repo on GitHub and clone it to your local machine.
1. Create a project on your local machine, then create a repo using the existing code, and finally push it to GitHub.

### Create New Repo
1. Navigate your browser to: h__ps://github.com/{YourGitHubUserName}
1. Click the *Repositories* tab at the top of the page.
1. Click the *New* button.  
    * The Create a New Repository page appears.
1. Enter a Repository Name: "temp-git-repo".
    * Fill in the rest of the form as needed for the project.
1. Push the *Create Repository* button.

### Clone a Repo
To clone the new repo just created above:
1. Create a folder on your local computer to hold the repository.
1. Copy the URL to the GitHub remote repository.
    * Navigate to the GitHub repo you want to clone, such as: h__ps://github.com/{your-github-username}/{repo-name}
    * If you have not added any files yet, you will see the "Quick setup" page and can copy the URL skipping the following few steps.
        * Otherwise, Click the ***<> Code*** dropdown
        * Click the ***Copy url to clipboard*** button (the one that looks like two boxes on top of each other)
1. Open Git Bash (Shift + Right click the folder created above and choose "open Git Bash here" from the context menu)
1. Type "git clone" and then right click Git Bash and choose ***paste*** from the context menu. 
    * Example: `git clone https://github.com/{your-username}/{repo-name}.git`

### Fork Instead of Clone
*Fork* a repo when you cannot or will not be *contributing* to the repository, but want to use the repo as a starting point for the project you have in mind.

A *clone* of a repository assumes you can create a pull request and modify the original repo, while a *fork* assumes you will be taking a fork in the road and all subsequent changes are separate from the parent repo. "Thanks for the start, I really appreciate it, but I want to take this in my own direction."

1. Find a repo you want to *fork*.
2. Navigate to the repo.
3. Click the ***<> Code*** tab, if you are not already on it.
4. Click the ***Fork*** near the top right of the page, under the tabs.
    * This will open the ***Create a new fork*** page.
    * The default options are most likely what you want, but take your time to make changes as you see fit.
5. Click the ***Create fork*** button.

The repo is now in your GitHub and you can ***clone*** it to your local and begin work.

### Starting a Git Repo with Existing Code
1. Open Git Bash in the project folder (Shift + Right click the folder and choose "open Git Bash here" from the context menu).
1. `git init -b main` to initialize a new repo and change the default branch name to "main".
1. Add `.gitignore` file to keep unnecessary files and secrets out of source control.
1. `git add .`, `git add file-name`, `git add directory-name` to stage files to be tracked.
1. `git commit -m "First commit of existing code"`
1. Follow the instructions in the "Create New Repo" above.
1. Copy the URL on the "Quick Setup" page that appears. (https://github.com/{your-github-username}/{repo-name}.git)
    * Click the ***Copy url to clipboard*** button (the one that looks like two boxes on top of each other)
1. `git remote add origin https://github.com/{your-username}/{repo-name}.git`: lets Git know where to direct any "push", "pull", or "fetch".
1. `git push -u origin main`: pushes the changes to the remote repository on the "main" branch. (As a general rule, you should not push directly to the "main" branch, but instead create a feature branch to work off of. The initial push is the only exception to this rule.)
    * The "-u" flag sets origin as the upstream remote for your branch. This will save you time in the future, because you can simply use `git pull`, `git fetch`, or `git push` when on this branch and will not need to type the "origin {branch-name}" part.



## The Three States
For an in depth tutorial, see the "***three-states.md***" file in this [repository](https://github.com/RolandChristensen/git-guided-examples).  

There are three states of tracked files. The ***Working Directory***, the ***Staging Area***, and the code ***Repository***.
1. ***Working Directory***: When you add a file or modify a file that is tracked by git, it will add it to the ***modified*** or ***working directory*** of its database. 
    * Use the `git status` command to see the changed files that are tracked in the ***working directory***.
    * Adding the "***.gitignore***" file is the way to stop tracking any file you do not want in source control and will remove it from the ***working directory***.
    * To undo all changes made to a file in the ***Working Directory***, use the `git restore {path/to/file.ext}` command.
2. ***Staging Area***: Using the `git add {operand}` command will move file changes to the ***staging area*** or ***index*** of Git's database. 
    * Use `git status` to see files added to the ***staging area***.
    * To back changes out of *staging*, use the `git restore --staged {path/to/file.ext}` command.
3. ***Repository***: Using the `git commit -m "Useful message"` command indicates that you are very confident the changes work as expected and adds the files to the ***repository*** or ***.git directory*** under the current ***branch*** you have checked out. 
    * Use `git log` to see file changes that have been committed to the currently checked out ***branch*** of the ***repository***
    * To back changes out of a ***commit***, use the `git reset --soft {commit-hash}` command.
    * To completely remove a ***commit*** and completely lose all the work done, use the `git reset --hard {commit-hash}` command. 

### Git Status
`git status` is used to see modified tracked files in the ***Working Directory***.  
Modified files in the ***Working Directory*** show up under the heading "Changes not staged for commit:".

`git status` also shows you modified tracked files in the ***Staged Area***.  
Modified files in the ***Staged Area*** are under the heading "Changes to be committed:".

### Git Add
This command will move changed files in the ***Working Directory*** to the ***Staging Area***.  
`git add {filename.ext}` or `git add {path/to/file.ext}` to ***stage*** a single file.  
`git add {path/to/directory/}` to ***stage*** a directory.  
`git add .` to ***stage*** all changes. (If there are a large number of files, seriously consider adding them one at a time.)

After adding files to the ***Staging Area*** you can verify the staging by using `git status`.

### Git Commit
This command will move changed files in the ***Staging Area*** to the ***Repository*** of the current branch.  
Planning your changes in advance before diving into coding will help you organize your work and plan your *commits* well.  
Make commits fully functional, testable units of code, complete with unit tests. As the word implies *commits* are a way of saying you are confident and ready to *commit* to the change you made.  
`git commit -m "Useful commit message, so you will know what was done without needing to go through the diff to see what changed"`

Amending a commit is useful for afterthoughts. It makes for a more readable *commit log* to amend small changes, like adding comments or one last unit test, rather than creating a new commit. The better you get at planning your work in advance and double checking your work before committing, the less you will use this. Also, this should not be done if you have pushed it to a public server others may have pulled from and should be used to update local changes only.  

`git commit --amend -m "Commit message that encompasses all changes, not just the amended changes"`  
Amending can also be used to change the *commit message* only, as long as you don't have any files ***staged***.  
`git commit --amend --no-edit` to amend a commit without changing the message.  

Note: you can skip the `git add {operand}` part if you dealing with a manageable number of changes by using the "-a" flag to add ***tracked*** files in the commit command.  
`git commit -a -m "Story 101: made a single line change to file x to do y."`: Adds and commits all changes in one command.  
The "-a" flag does not work with ***untracked*** files, but only files that have been *added* before, which is probably for the best because it makes you look at each file to make sure you shouldn't add it to the .gitignore file.  

### Git Log
`git log` shows you all of the ***commits*** in the ***Repository*** / ***.git directory*** for the branch you are currently on.  
`git log {branch-name}` shows you all of the ***commits*** for the branch indicated.  
`git log --oneline {branch-name}` shows you the ***commits*** in an abbreviated way, omitting the author and date of changes.  

Assuming you created a branch named "branch-name" based off the "main" branch,  
`git log --oneline main..branch-name` only shows the commits on the branch since it was created.  

### Git Reset
You can reverse the process to back changes out of any of the three states.  
To back a ***commit*** out of the ***Repository*** to the ***Staging Area***, you want to ***reset*** the ***HEAD*** pointer to point to the last commit you want to keep.  
Using ***reset*** with the `--soft` flag will keep your changes and move them back to the ***Staging Area***. If you use the `--hard` flag all changes will be forever lost.  

1. `git log` to get the hash of the last commit you want to keep. If the commit messages are not clear enough for you to find the appropriate one, you may have to do a deeper dive and look at the diffs.
1. Drag your cursor to select the hash of the last commit you want to leave in the ***Repository***. The ***commit*** above the hash you copy will be removed from the ***Repository***.
1. Right click the selected area and select **Copy** from the context menu.
1. Press "q" to quit the log.
1. `git reset --soft {hash-you-copied}` to back the commits, after the one you copied the hash from, out to the ***Staging Area***. (Right click Git Bash and select paste from the context menu to paste the hash.)
1. Verify the change by using `git log` and `git status` to see the changes.

### Git Restore
To back out changes in the ***Staging Area*** to the ***Working Directory***, "restore" the files with the "--staged" flag.  
`git restore --staged {file.ext}` or `git restore --staged {path/to/file.ext}` to back out a file.  
`git restore --staged .` to back out everything all at once.  
You can also use globbing (wildcards) such as "*" to match files with ***restore***.  

To back out changes in the ***Working Directory***, "restore" the files.  
This is like a big *undo* command for all changes made on a single ***tracked*** file.  
`git restore {file.ext}"` or `git restore {path/to/file.ext}"` to back out a single file.  
`git restore .` to *undo* all changes in all files in the ***Working Directory***, the mightiest *undo*. (It should be obvious that you had better be absolutely sure you want to do this.)  
Note: this will not delete files, but only undo all changes on ***tracked*** files. To delete files from the ***Working Directory***, simply delete them using the file system.  

### Git RM
For an in depth tutorial, see this [repository](https://github.com/RolandChristensen/git-guided-examples) repositories README and search for "Git RM".  
RM stands for remove, and Git RM is used to remove a file from the ***Repository***. It does not remove the file or files from the file system, but removes file(s) from the ***Repository***.    
To keep files from being tracked and out of your ***Working Directory*** you will want to add the files to a .gitignore file.  

Scenario: You accidentally ***committed*** a file that you do not want in source control such as a configuration file with passwords in it.  

To remove the file from the ***Repository***:  
1. `git rm --cached {path/to/secrets-file.ext}`
1. `git status` to see the results
    * The file is *staged* in the ***Staging Area*** to be deleted.
        * You will need to *commit* this change, so that it will be *removed* from the ***repository***.
    * The file is also in your ***Working Directory***.
        * To stop tracking this file we will add it to the .gitignore file.
  
To stop the file from being *tracked*:
1. Add the full path to the file (path/to/secrets-file.ext) in the .gitignore file.
1. `git status` to see the results
    * The file "secrets-file.ext" should no longer be in the ***Working Directory***.
    * The file ".gitignore" should be in the ***Working Directory***.
    * The file "secrets-file.ext" should still be *staged* for deletion.
  
To complete the process: 
1. `git add .gitignore`
1. `git commit -m "Removed file 'secrets-file.ext' from repository"`



## Avoiding Merge Conflicts
When working with other developers, to improve code quality and reduce any headaches when merging, follow these simple rules.

* Start with a ***clean working directory*** before beginning new work.
* ***Pull/merge*** the latest main branch just before starting a new feature.  
* Create a ***feature branch*** based off the recently pulled main branch.
* Once a ***feature branch*** is created, finish it as quickly as possible to reduce the number of changes that will be merged into the remote ***main*** while you are working.
* Do not work on multiple branches that touch the same *files* simultaneously, unless absolutely needed (such as a hotfix).
* Do not have two developers work on code that will touch the same *files* simultaneously.  
    * During planning sessions, coordinate work that will be touching the same *file(s)* to be worked on consecutively rather than simultaneously.  
    * *All developers*, who are about to pick up new work, should be aware of what is being worked on and what *files* are likely to be modified.  
    * Note: C## has *partial classes* that will split a class into two or more separate files allowing multiple people to work on a class simultaneously without risking merge conflicts.  


## Developing a New Feature
To reduce the chances of merge conflicts, accidentally overwriting code, or introducing unexpected changes follow these source control best practices.

### Start with a Clean Working Tree
Do a simple verification that you do not have any uncommitted changes on your ***main*** branch (staged or unstaged changes).

`git status` will tell you what branch you are on, if you have any staged or unstaged changes, and whether you need to ***pull*** any changes on the remote.  
Verify you are on branch ***main***. If not, `git checkout main`.  
Verify Git reported "nothing to commit, working tree clean".  

If you have uncommitted files, you will need to figure out what should be done with them:  
* Undo changes, because they are not needed.
* Stash the changes, if they are needed and incomplete, so you can recover them later. (Search for "Stash" to find instructions below)
* Add and commit them to the appropriate branch, if the changes are needed and complete.

### Sync your Local Repository with the Remote
This assumes you are going to merge into a branch named ***main***, but you can substitute ***main*** for any branch name such as ***dev*** if you are using a more complex branching strategy. With complexity, comes more chances to make mistakes, but plenty of organizations use complex strategies.  

`git status` will tell you if you are up to date with 'origin/main' or not. If not, you will need to ***pull*** or ***fetch***.  

Keeping your local repository up-to-date is critical to avoid overwriting someone elses work.  
If you have well defined quality gates employed to prevent merging code that has not been thoroughly tested, you should usually not be worried to get the latest state of the main branch.  

If you are not worried about the current state of your remote main branch, follow the directions below. 
1. `git status` to verify you are on the main branch. If not, `git checkout main`.
1. `git pull` to pull down all the most recent changes from the remote to your local repository.

If, for some reason, you are concerned about pulling the current branch as it is, follow the directions below to inspect the state of the branch before merging.  

1. `git status` to verify you are on the main branch. If not, `git checkout main`.
1. `git fetch` to see any changes from the remote repo before merging them into your local branch. The default remote to fetch from is *origin* so this command is equivalent to `git fetch origin`
1. `git log origin/main` and/or `git diff ..origin/main` will show you what has changed in the remote repo since you last pulled. If you are happy to merge those new changes into your local ***main*** branch then go ahead. If you do not want the current state of the remote repo, for whatever reason, you can leave it the way it is because you only ***fetched*** it.
1. If you are sure you are happy to merge, `git merge origin/{branch-name}`. Example: `git merge origin/main`

### Creating a Feature Branch
Isolate your new code from the shared branch everyone is working from, until the new branch has been thoroughly tested.  
I will use the name "feature-name" for all these instructions.  

I will show two ways of doing this:
1. Create the feature branch locally.
2. Create the feature branch on the remote.

The advantage of creating the branch on the remote is that others can see that you have a branch created for the work to be done. 

#### To Create a Branch Locally
1. `git status` to verify you are on the main branch. If not, `git checkout main`.
1. `git branch -b new-branch-name` creates a new branch identical to the current branch and 'checks out' that new branch. The branch name should correspond to the feature you are starting. Often a ticket number is included.

#### To Create a Branch on GitHub
1. Navigate to the GitHub repo you are developing the new feature for, such as: h__ps://github.com/{your-github-username}/{repo-name}
1. Click the ***Branches*** button underneath the name of the repo
    * The ***Branches*** page will appear.
1. Click the ***New branch*** button.
    * The ***Create a branch*** dialog will appear.
1. Type in the name of the branch for the feature. The branch name should correspond to the feature you are starting. Often a ticket number is included.
1. If the ***Source*** dropdown does not show the ***main*** branch is selected, select it from the branches drop down.
1. Click the ***Create new branch*** button

Now that the branch is created on the remote, we will finish on your local machine:
1. `git branch -r` to see all the remote branches.
1. `git pull` to get the new branch on your local machine. For safety, you may want to use `git fetch` instead.
    * The output should contain a line similar to this: `* [new branch]  feature-name  -> origin/feature-name` the name of which should be the name you created on the remote.
1. `git checkout {feature-branch-name}` Example: `git checkout feature-name`

### Development Source Control Process
Choosing when to commit should be deliberate. 
* If you plan your work in advance, you should be able to divide the work up into commits in advance of doing the work.  
* A commit should be a fully functional, testable unit of code, complete with unit tests to test it.

The basic process is:
1. Develop a fully functional, testable unit of code, complete with unit tests.
1. `git diff` to view the changes you have made to double check your work before committing. (There are much better tools for viewing diffs, Visual Studio and Visual Studio Code have great extensions.)
1. `git add .`, `git add file-name`, or `git add directory-name` to stage files to be tracked.
1. `git commit -m "Useful commit message that describes the change"`
1. Repeat until the feature is done.

### Push Branch to the Remote
When done with the feature, you will want to ***push*** the feature branch to the remote.  

1. `git push -u origin {feature-branch-name}` to ***push*** the branch to the remote.
    * The "-u" flag sets origin as the upstream remote for your branch. This will save you time in the future, because you can simply use `git pull`, `git fetch`, or `git push` when on this branch and will not need to type the "origin {branch-name}" part.

### Create a Pull Request
Getting others to review your work improves the quality of the work and increases your knowledge.  
I am not going to go into the details of setting GitHub rulesets and actions here, but only go through the basics.

1. Navigate to the GitHub repo you are developing the new feature for, such as: h__ps://github.com/{your-github-username}/{repo-name}.
1. You should see a message stating "{branch-name} had recent pushes {some amount of time ago}" with a ***Compare & pull request*** button.
    * If not, select the branch from the ***branches*** dropdown.
1. Click the ***Compare & pull request*** button.
1. Fill out the form with valid information and using the standards for the project you are working on.
1. Add people who should review. Good quality gates include requiring one or more senior reviewers along with a specific number of total reviewers.
1. Click the ***Create pull request*** button.

If you have been careful you will see this message, "This branch has no conflicts with the base branch", on the resulting page.

The ***pull request*** (PR) has a couple of ways to view the changes (diffs) between the feature branch you pushed and the branch you want to merge it into.  
The ***Files Changed*** tab will give you an easy to read list of changes.  
* From the ***Files Changed*** you can leave comments on files.
* You can select specific sections of code and leave a comment on that section.
* From the ***Review Changes*** dropdown you can also ***Approve*** or ***Request Changes***

### Modifying Pull Request
If there are comments that need addressing or requested changes, you will need to address those.  

1. If you have started another feature, you will need to make sure you can ***checkout*** the branch you are trying to merge without unintended consequences.
    * If possible, you should not start a feature that has files that will conflict with the ***pull request*** until it has been merged to be as safe as possible.
    * If you notice you have made changes to a file on the new branch that could cause a merge conflict with the same file on the current ***pull request***, you are in the danger zone and should consider what to do *very carefully*.
    * If you have any ***staged*** or ***untracked*** files you should ***Stash*** those before checking out the ***pull request*** branch to avoid any accidental inclusions of changes you did not intend.
1. If you have a clean working directory, `git checkout {feature-branch-name}` 
1. Code the changes complete with unit tests. (There is often a pressure to go faster than normal when redoing work. Take a deep breath and make sure you do it right.)
1. `git add .`, `git add file-name`, or `git add directory-name` to stage files to be tracked.
1. `git commit -m "Useful commit message that describes the change"`
1. Repeat until all changes are done.

When all changes are done:
1. `git push` to ***push*** the branch to the remote (Note: this assumes you used the "-u" flag when you first pushed otherwise use `git push origin {feature-branch-name}`.
1. When you return to the ***pull request*** GitHub will recheck the branches ability to be merged, update the commit history, and update the diffs. (You may need to push the "refresh" button, if you see it.)
1. You will want to add a comment and tag the person who made comments or requested the change to recheck the ***pull request***.

### Merge Pull Request
If the ***pull request*** is ***approved*** by everyone required and all other quality gates such as unit tests and static analysis tools have passed:

1. Go to the ***Conversations*** tab
1. Click the ***Merge pull request*** dropdown and select one of the following or simply click the button to choose the default.
    * ***Create a merge commit*** will add all of the commits to the main branch and add a ***merge commit*** to mark the merge.
    * ***Squash and merge*** will squash all of your commits into a single one, keeping your commit history on the base branch cleaner and more manageable.
    * ***Rebase and merge*** will give you a cleaner history on your base branch. Your base branch will display a linear list of commits and will not include any extra ***merge commits*** each time you merge as the first option would. This can be problematic and you should plan your team workflows in advance to use rebase effectively.
1. Click the ***Confirm merge*** button
1. In general, you should then click the ***Delete branch*** button, unless your team has a different process for the deletion of feature branches.

On your local machine you will want to fetch and merge the pull request into your local ***main*** branch and delete the feature branch:
1. `git checkout main`
1. `git pull`: fetches and merges ***main*** branch from the remote
1. `git branch -d {feature-branch-name}` to delete the feature branch.



## Double Check Your Work
It is a good practice to double check everything you do before ***staging*** or ***committing***.  
It is also a good idea to review your ***commits*** before the final push to the remote.

### Git Diff Before Staging
Double check your work before you add it to the "staging" area.  
`git diff`  

The following assumes you use the *vimdiff* tool:  
1. Arrow down, arrow up, page down, page up to navigate changes from all files.
2. Enter "q" to exit the diff tool.

The diff in the command line is useful, but there are much better tools for viewing diffs (Visual Studio Git extension for one)

### Git Diff Between Staged Files and Last Commit
Once staged, you may want to see the difference between staged files and the last commit before committing.  
`git diff --staged` or `git diff --cached`

### Git Log to View Commits on Feature Branch since it was Checked Out
If you planned well and used good messages, you should be able to clearly see the design of the code in the ***commit log***.
`git log --oneline main..branch-name` displays only the commits you created since the feature branch was checked out. The `--oneline` flag abbreviates the log to one line, omitting the author and date information.



## Renaming a Branch
At work, I include a ticket number in my feature branch name and have transposed numbers before. You can rename a local branch with the following command:  
`git branch -m oldName newName`  

Example: Many prefer to use *main* instead of *master* for the top-most branch.  
`git branch -m master main`  



## Merge Branches
Note: this is a bad idea when working with others. You should not be merging changes into "main" without going through a code review and additional quality gates. However, when working on a local repo by yourself, this is perfectly fine.  

1. `git checkout main` switch to the branch you want to merge the changes into.
1. `git merge branch-name` merge the branch into the current branch you are on.
1. `git branch -d branch-name` delete the branch now that you are done with it.

### How to Tell if a Branch has been Merged
Scenario: you are checking to see what branches are on your local repository, `git branch`, to see all the branches on your local and there is a branch in the list that shouldn't be there.  
You may remember working on the feature, but cannot remember if you merged it or not.  
You don't want to delete the branch unless you are sure you have merged it.  

A quick sanity check will help to ease your mind.
1. `git checkout {parent-branch-name}` to checkout the parent branch you should have merged the feature branch into.
1. `git branch --merged` to see all the branches that have been merged into the current checked out branch. This will tell you if you merged the branch, but not if there are additional commits or unfinished work on the feature branch that are not merged.

Depending on your workflow the above may be enough for you to decide to delete it. That is up to you.  
For a complete and thorough check, please continue reading.

First, you will want to check if you have any unfinished work in the questioned branch.  
1. `git checkout {branch-you-are-not-sure-about}`
1. `git status` to see if you have unfinished work.

If you have unfinished work, carefully examine what it is and decide if you need to finish it. (Search for "git diff" on this page for help.)  

If you do not have unfinished work, you will still want to check if you have any "committed" work that has not been merged.  
To check this locally you will need to be sure you have ***pulled*** the parent branch from the remote. It is possible that you merged the branch remotely, but have not ***pulled*** those changes down.
1. `git checkout {parent-branch-name}`
1. `git pull` or `git pull origin {parent-branch-name}` if you have not set the "upstream remote".
1. `git log {parent-branch-name}..{branch-you-are-not-sure-about}` to see any commits on the feature branch that are not on the parent branch. Depending on your workflow, it could be that after merging you did additional work on the branch and have changes on it.

If the output of the "log" command above is empty you have merged everything into the parent branch and you can delete the branch in question.  
`git branch -d {branch-you-are-NOW-sure-about}`

Otherwise, you will want to examine the commits on the feature branch to see what changes have been made and whether they need to be merged.  
If you are good at writing useful messages, you will most likely be able to tell what is in the commit and make a decision.
`git log --oneline` to see the commit messages. 

If you still have any questions about a commit, follow these directions to examine the commit diff.  
1. `git log` to see the commits.
1. Drag your cursor to select the hash of the commit in question.
1. Right click the selected area and select **Copy** from the context menu.
1. Press "q" to quit the log.
1. `git show {hash-you-copied}` displays a diff of all the changes. (Right click and select **Paste** from context menu.)
1. Repeat for all the commits until you can make a decision about deleting this branch or finishing the work.



## Hotfix Forces You to Stop Working on a Feature
You are working on a new feature, but are asked to switch to a hotfix that needs to go out A.S.A.P.  
Before starting you want to make sure you are starting with a clean ***Working Directory*** and ***Staging Area***.  

All commits on the branch you are working on will remain when you change branches, so you do not need to worry about them.  
Any uncommitted changes need to be ***stashed***.

### Git Stash
1. `git stash --include-untracked` to stash files that are currently tracked and untracked files. The "--include-untracked" flag will include the ***Working Directory***, which is probably a good idea to avoid checking changes into the hotfix branch inadvertently.  
1. `git checkout main` to switch to parent branch you will be merging the hotfix into.
1. Follow the instructions in the [Developing a New Feature](#Developing-a-New-Feature) section and do not cut corners. Do not let the pressure to go fast interfere with the quality of the work.

When you are done with the hotfix, you are ready to resume work on the original feature you were working on.  
1. `git switch original-branch-name`
1. `git stash pop` to get the last stash pulled off the top of the stack.

You are now where you left off.  
Note: if you made changes to a file in the hotfix that you are also working on in the feature, you may have merge conflicts you will need to resolve.  



## Algorithm Comparison Testing Using Branches
Using branches to try out different algorithms is a nice way to see the results and quickly switch between to alternatives to see which one is best.  
This is especially true of front end local A/B comparisons to compare styles.  
Switching quickly between branches is also a good way to demo differences to stakeholders.  

If you want to try two different approaches to the same problem, create two branches off the same branch then try out each approach and gather data to decide.
These instructions assume you have already created a feature branch based on your *main* branch.  

1. `git branch new-feature-a` to create the A branch.
1. `git branch new-feature-b` to create the B branch.
1. `git branch` to list all the branches to double check your work.
1. `git checkout new-feature-a` to check out the A branch to make changes based on plan A.
1. Completely finish plan A and commit all changes.
1. `git checkout new-feature-b` to check out the B branch to make changes based on plan B.
1. Completely finish plan B and commit all changes.

Now you can switch between each branch easily using the `git switch {branch-name}` command to compare changes.

For this demonstration, we are going to assume we chose plan B.
1. `git checkout new-feature-name` to get to the parent branch of the A/B branches created.
1. `git merge new-feature-b` to accept plan B.
1. Verify the files look as you expect and check the `git log` to verify the commit log looks as expected.
1. `git branch -D new-feature-a` to delete plan A. Using the uppercase "-D" flag will force delete the branch. Note: using the lowercase "-d" flag will not delete the branch, because Git is trying to prevent you from making a mistake due to changes that have not been merged.
1. `git branch -d new-feature-b` to delete the branch you merged into *new-feature-name* branch.
