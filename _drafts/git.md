Git and GitHub related notes

# Git and GitHub Source Control
Source control is essential for programming and Git combined with GitHub is a good solution.  
Use this page as a quick reference for often used scenarios.

To make it stick in your mind, create your own tutorials and quizes.  
Use this [repository](https://github.com/RolandChristensen/git-guided-examples) to create practice exercises and quizes.  
The repo will go into more detail on source control topics and give you a safe place to practice.  
If you find yourself in trouble and can't figure out how to undo a mistake, this repo is also a good place to reproduce the problem and learn to undo it safely.

This documents Git using the command line, but the skills outlined can be reproduced by other tools you use to interface with Git.  
I use Visual Studio and Visual Studio Code extensions for Git, but still go to the command line frequently and create scripts which demand knowledge of the command line.

# Quick Start Scenarios
Below are common scenarios used in day to day development.  

[Repo Creation](#Repo-Creation)  
[The Three States](#The-Three-States)  
[Renaming a branch](#Renaming-a-branch)  
[Push local branch to a remote repository](#Push-local-branch-to-a-remote-repository)  
[Create a dev branch](#Create-a-dev-branch)  
[Creating a feature branch](#Creating-a-feature-branch)  
[View the difference between files changed on this branch before staging](#View-the-difference-between-files-changed-on-this-branch-before-staging)  
[View the changes between staged files and the last commit](#View-the-changes-between-staged-files-and-the-last-commit)  

# Repo Creation
For an in depth tutorial, see the "***create-repo.md***" file in this [repository](https://github.com/RolandChristensen/git-guided-examples).  
I will go through two ways to create a repo.  
1. Create a new *blank canvas* repo on GitHub and clone it to your local machine.
1. Create a project on your local machine, decide you want to keep it, then create a repo using the existing code, and finally push it to GitHub.
    * You could be trying out different approaches then settle on the final archetechtural design you want to finally push to the remote.

## Create New Repo
1. Navigate your browser to: h__ps://github.com/{YourGitHubUserName}
1. Click the *Repositories* tab at the top of the page.
1. Click the *New* button.  
    * The Create a New Repository page appears.
1. Enter a Repository Name: "temp-git-repo".
    * Fill in the rest of the form as needed for the project.
1. Push the *Create Repository* button.

## Clone a Repo (The new one just created)
1. Create a folder on your local computer to hold the repository.
1. Copy the URL to the GitHub remote repository.
    * Navigate to the GitHub repo you want to clone, such as: h__ps://github.com/{your-github-username}/{repo-name}
    * If you have not added any files yet, you will see the "Quick setup" page and can copy the URL skipping the following few steps.
        * Otherwise, Click the ***<> Code*** dropdown
        * Click the ***Copy url to clipboard*** button (the one that looks like two boxes on top of each other)
1. Open Git Bash (Shift + Right click the folder created above and choose "open Git Bash here" from the context menu)
1. Type "git clone" and then right click Git Bash and choose ***paste*** from the context menu. 
    * Example: `git clone https://github.com/{your-username}/{repo-name}.git`

## Starting a git repo with existing code
1. Open Git Bash in the project folder (Shift + Right click the folder and choose "open Git Bash here" from the context menu).
1. `git init -b main` to initialize a new repo and change the default branch name to "main".
1. Add `.gitignore` file to keep unnecessary files and secrets out of source control.
1. `git add .`, `$ git add file-name`, `$ git add directory-name` to stage files to be tracked.
1. `git commit -m "First commit of existing code"`
1. Follow the instructions in the "Create New Repo" above.
1. Copy the URL on the "Quick Setup" page that appears. (https://github.com/{your-github-username}/{repo-name}.git)
    * Click the ***Copy url to clipboard*** button (the one that looks like two boxes on top of each other)
1. `git remote add origin https://github.com/{your-username}/{repo-name}.git`: lets Git know where to direct any "push", "pull", or "fetch".
1. `git push -u origin main`: pushes the changes to the remote repository on the "main" branch. (As a general rule, you should not push directly to the "main" branch, but instead create a feature branch to work off of. The initial push is the only exception to this rule.)
    * The "-u" flag sets origin as the upstream remote for your branch. This will save you time in the future, because you can simply use `git pull`, `git fetch`, or `git push` when on this branch and will not need to type the "origin {branch-name}" part.

# The Three States
For an in depth tutorial, see the "***three-states.md***" file in this [repository](https://github.com/RolandChristensen/git-guided-examples)..  

There are three states of tracked files. The ***Working Directory***, the ***Staging Area***, and the code ***Repository***.

1. ***Working Directory***: When you modify a file that is tracked by git, it will add it to the ***modified*** or ***working directory*** of its database. 
    * Use the `git status` command to see the files tracked in the ***working directory***
    * Adding the "***.gitignore***" file is the way to stop tracking any file you do not want in source control and will remove it from the ***working directory***
2. ***Staging Area***: Using the `git add {operand}` command will move files to the ***staging area*** or ***index*** of Git's database. 
    * Use `git status` to see files added to the ***staging area***
3. ***Repository***: Using the `git commit -m "Useful message` command indicates that you are very confident the changes work as expected and adds the files to the ***repository*** or ***.git directory*** under the current ***branch*** you have checked out. 
    * Use "***git log***" or "***git reflog***" to see files that have been committed to the currently checked out ***branch*** of the ***repository*** 

## Renaming a branch
At work, I include a ticket number in my feature branch name and have transposed numbers before.  
`git branch -m oldName newName`  
`git branch -M oldName newName`: "-M" is used to force the rename if the new name has already been used.  

Example: Many prefer to use *main* instead of *master* for the top-most branch.  
`git branch -m master main`  

## Push local branch to a remote repository
If you have previously issued the "git remote add origin" command then you simply need to push.  

`git push origin {branch-name}`: pushes your local branch (branch-name) to the remote repository (origin).  

If this is your very first push to the remote repo follow these instructions:  

1. Navigate to the GitHub repo you want to push changes to, such as: h__ps://github.com/{your-github-username}/{repo-name}
1. Copy the URL to the repo
1. `git remote add origin {The URL you copied}`: lets Git know where to direct any "push", "pull", or "fetch".
1. `git push -u origin {branch-name}`: pushes your local branch (branch-name) to the remote repository (origin). Example: `git push -u origin main` (As a rule, when working with others, you will not push directly to the "main" branch, but instead use feature branches to work from.) The "-u" flag sets origin as the upstream remote for your branch. This will save you time in the future, because you will simply need to use `git push` or `git pull` when on this branch and will not need to type the "origin {branch-name" part. This makes sense if you are going to be using this branch a lot. If you are using short lived feature branches, this will not save any time.

## Creating a feature branch
Isolate your new code from the shared branch everyone is working from, until the new branch has been thoroughly tested.  
This assumes you are going to merge into a branch named ***main***, but you can substitute ***main*** for any branch name such as ***dev*** if you are using a more complex branching strategy. With complexity, comes more chances to make mistakes, but plenty of organizations use complex strategies.  

If you are not worried about the current state of your main branch, follow the directions below. If you have well defined quality gates employed to prevent merging code that has not been thouroughly tested, you should usually not be worried to get the latest state of the main branch.
1. `git status` to verify you are on the main branch. If not, `$ git checkout main`.
1. `git pull` to pull down all the most recent changes from the remote to your local repository.
1. `git branch -b new-branch-name` creates a new branch identical to the current branch and 'checks out' that new branch. The branch name should correspond to the feature you are starting, often a ticket number.

If, for some reason, you are concerned about pulling the current branch as it is, follow the directions below to inspect the state of the branch before 

1. `git status` to verify you are on the main branch. If not, `$ git checkout main`.
1. `git fetch`: to see any changes from the remote repo before creating your new branch. The default remote to fetch from is *origin* so this command is equivalent to `git fetch origin`
1. `git log origin/main` and/or `git diff ..origin/main`: this will show you what has changed in the remote repo since you last pulled. If you are happy to merge those new changes into your local ***main*** branch then go ahead. If you do not want the current state of the remote repo, for whatever reason, you can leave it the way it is because you only ***fetched*** it.
    * If you are sure you are happy to merge, `git merge origin/{branch-name}`. Example: `git merge origin/main`
1. `git branch -b new-branch-name` creates a new branch identical to the current branch and 'checks out' that new branch

## View the difference between files changed on this branch before staging
Double check your work before you add it to the "staging" area.  
`$ git diff`  

The following assumes you use the *vimdiff* tool:  
1. Arrow down, arrow up, page down, page up to navigate changes from all files.
2. Enter "q" to exit the diff tool.

The diff in the command line is useful, but there are much better tools for viewing diffs (Visual Studio Git extension for one)

## View the changes between staged files and the last commit
`git diff --staged` or `$ git diff --cached`

## Committing staged changes to the branch 
Make commits self-contained, testable units.  
`git commit -m "Useful commit message, so you can find these changes in the future"`

## Ammending a previous commit
`git commit -a -m "Commit message that encompasses all changes, not just the amended changes"`

## Viewing all commits
* `git log` view the commits with the author and date of change
* `git log --oneline` the commits are abbreviated on one line

## View only the commits on the branch since it was created
* `git log --oneline main..branch-name` displays only commits

## Merge branch into another branch
Note: this is a bad idea when working with others. You should not be merging changes into "main" without going through a code review and additional quality gates.  

1. `git checkout main` switch to the branch you want to merge the changes into
1. `git merge branch-name` merge the branch into the current branch you are on
1. `git branch -d branch-name` delete the branch now that you are done with it







Todo: got to here on review.








## You forgot to delete branches and can't remember if you have merged them
You don't want to delete the branch unless you are sure you have merged them.
1. `git branch` to see all the branches on your local
2. `git checkout {parent-branch-name}` to checkout the parent branch you should have merged the feature branch into
    * This guide assumes you have pulled all changes from the remote into your local parent branch. Elsewhere on this page, you will find instructions to do this, if you haven't already.
4. `git branch --merged` to see all the branches that have been merged into the current checked out branch
    * Depending on your workflow and the branch in question, it could be that after merging you did additional work, so you may need to check the branch to see if you have any changes on it.
5. `git log parent-branch-name..feature-branch-name` to see any commits on the feature branch that are not on the parent branch
    * If the output is empty you have merged everthing into the parent branch and you can delete the branch in question.
6. `git branch -d {feature-branch-name}`

## You forgot to checkout a new branch and started working on a new feature while still on *main*
As long as you have not commited any changes you can simply checkout a new branch.
`git checkout -b new-branch-name`

If you have committed changes follow the directions below to *reset* the HEAD with the `--soft` flag to the point where you started making changes.  
`--soft` will keep your changes. If you use the `--hard` flag all changes will be forever lost                              I.
1. `git log` or `$ git reflog` to get the hash of the commit to go back to in the *log*, or note the HEAD@{#} to go back in the *reflog*
    * Press 'q' to quit the log
2. `git reset --soft {hash}` or `git reset --soft HEAD@{#}` to back the commit to *stage*
    * Verify the changed files are *staged* by using `git status`
3. `git checkout -b new-branch-name` to 

## You cannot continue working on a feature/branch due to a blocking issue or because a hotfix demands your attention
    * All commits will remain on this branch when you change branches, so you do not need to worry about them
    * Any uncommited changes need to be *stashed*
        1. `$ git stash` to stash files that are currently tracked. If you need to include untracked files, which is probably a good idea to avoid checking it into another branch inadvertantly `$ git stash --include-untracked`
        2. Switch to a new branch and complete the new feature or hotfix knowing all your hard work is waiting for you on the previous branch
        3. When ready to resume work on the original branch you abandoned to work on the other feature
        4. `$ git checkout original-branch-name`
        5. `$ git stash pop` to get the last stash pulled off the top of the stack

## A/B Local Testing Using Branches. (Using branches to try out different algorithms)
    * If you want to try two different approaches to the same problem, create two branches off the same branch then try out each approach and gather data to decide.
        1. `git status` to verify you are on main branch. If not, `$ git checkout main`.
        1. `git checkout -b new-feature-name` creates a new branch identical to the current branch and 'checks out' that new branch
        1. `git checkout new-feature-name` to switch to the new branch
        1. `git branch new-feature-a` to create the A branch
        1. `git branch new-feature-b` to create the B branch
        1. `git branch` to list all the branches to double check your work
        1. `git checkout new-feature-a` to check out the A branch to make changes based on plan A
        1. Add the following line to *dummy-file-1.md*: "This represents code for a new feature using plan A of two plans."
        1. `git add dummy-file-1.md`
        1. `git commit -m "Testing plan A"`
        1. `git checkout new-feature-b` to check out the B branch to make changes based on plan B
        1. Look at *dummy-file-1.md* to verify the line you added is not there in this branch.
        1. Add the following line to *dummy-file-1.md*: "This represents code for a new feature using plan B of two plans."
        1. `git add dummy-file-1.md`
        1. `git commit -m "Testing plan B"`
        1. At this point you would test the changes on each branch to decide which one performs best or the one that best fits your design. For this demonstration, we are going to assume we chose plan B.
        1. `git checkout new-feature-name` to get to the parent branch of the A/B branches created.
        1. Look at *dummy-file-1.md* to verify the line you added is not there in this branch.
        1. `git merge new-feature-b` to accept plan B.
        1. Look at *dummy-file-1.md* to verify the line you added is now in the parent branch.
        1. `git branch -d new-feature-a` will not delete the branch, because Git is trying to prevent you from making a mistake because you have changes that have not been merged.
        1. `git branch -D new-feature-a` to delete plan A. Using the `-D` flag will force delete the branch.
        1. `git branch -d new-feature-b` to delete the branch you merged into *new-feature-name* branch.
        1. At this point we would continue to work on the new feature or push the branch to the remote server and create a pull request. For this tutorial feel free to checkout main and delete the *new-feature-name* branch to return to the place you started from.


## Making changes and viewing the changes in the file to double check your work before commiting it

## Dividing your work into small manageable and unit testable changes (Commits) to log the changes of new work 



Assumptions:  
You already have Git installed and a folder of source code, currently not under source control, that you can use to experiment with.  
The version of Git I currently have installed is 2.44.0.windows.1.  
I an using Windows, but Git on the command line works the same for Linux and Mac.  

Scenario: 
You begin a project by installing all dependencies and stubbing out files and classes and then want to introduce source control to track any future changes.

# Initialize a New Git Repo
To begin using Git to track local changes to an existing project folder (Assuming you did not clone another repo):  
1. Navigate to the folder with source code you will be using and open a command prompt in the root directory. (Windows: Shift+Right click root folder and select Git Bash from the menu.)
2. `$ git init`
     A hidden folder called .git will be created in the folder.
     If you made a mistake and the .git folder is not in the root folder, you can simply delete the .git folder and try again.
     Another problem you may encounter is if you added a Git repo under an existing Git repo. Do not nest repos.  

This post is just about local development using Git, so I will not discuss adding a remote repo yet or add a README file.   

# Git Status 
To see what files have been changed or added to the repo since the last commit:  

`$ git status`

All the files that have been added or modified are displayed. Since you used existing source, you should see every file and folder in the root folder you added.  
You will most likely have files that you do not want care to track or push to a remote repository, so we will want to add a .gitignore file.  

# Add .gitignore file
I recommend adding a .gitignore file now, since we are using existing source.  
The easiest approach is to search the internet for a .gitignore file for the type of project you are using and add the .gitignore file to the root folder.  
Otherwise, do some research to figure out how to write your own and carefully add each file, folder, or file pattern you want to ignore now.  

Regardless, it is a good idea to carefully monitor all the files using `$git status` afte any change and ignore any files that are not necessary and waste space.  
Look for files that have a noticable pattern you can ignore instead of each file individually, such as files that have a UUID or date as part of the filename.  

Once done do another `$git status` and see the difference between the last status and this one.

# Git Add
This command will add files to the staging area (a.k.a. this command will stage files).  
In other words each file added or staged is ready to be committed, which means that it will be ready to be pushed to a remote server.
Try each one of these commands in order and do a `$ git status` after each one to see the difference.  
`$ git add {filename.ext}` or `$ git add {path/to/file.ext}`: to add a single file.  
`$ git add {path/to/directory/}`: to add a directory.  

You can add everything in the untracked files list in one go using `$ git add .`  
This can be a problem when you are dealing with a lot of files, and you should always mindfully check that each file was intentionally changed and should be pushed to the remote server.  

## CRLF line endings or LF line endings
If you are a Windows programmer, you will inevitably run into a warning when you add files that "CRLF will be replaced by LF".  
This is a good thing, if you ever work with devs using Linux or Mac.  
This means that your configuration setting for Git are working right, if you are on Windows. Just ignore the message and move on with your coding.  

The following commands will set the core.autocrlf setting to the appropriate value for your OS:  
Windows: `$ git config --global core.autocrlf true`. Alternatively, you can set your IDE to use LF line endings.  
Linux or Mac: `$ git config --global core.autocrlf input`  

If you work on Windows and will **never** interact with any OS other than Windows, you may want to turn off autocrlf to not have to see that warning.  
`$ git config --global core.autocrlf false`

# Git Commit
Once you have all of your changes *staged* you are ready to *commit* them.  
As the word implies *commits* are a way of saying you are confident and ready to commit to the change you made.  
To avoid complexity, it is recommended to change things in small bite-sized chunks that are as self contained as possible and commit these pieces as they are done one at a time.  
Planning your changes in advance before diving into coding will help you organize your work and plan your *commits* well.  

Commits should contain a message that helps you remember and lets others know what you did without reading the entire diff.  
In the commit add a useful message by adding the `-m` flag to the commit.  
`$ git commit -m "Initial commit"`

Note: you may find that you need to configure your email and name.  
`$ git config --global user.email "Email@email.com"`  
`$ git config --global user.name "FName LName"`  

`$ git status` will show that there are no staged files after the commit.

Note: you can skip the `$ git add ...` part if you dealing with a manageable nunber of changes by using the `-a` flag in the commit command.  
`$git commit -a -m "Story 101: Added special.svg for download icon."`: Adds and commits all changes.

# Git Log
`$ git log`: Lists the commits and looks something like this:  

```
$ git log
commit 278d94fc06df6555dfb9cf0cc9ce94824e361811 (HEAD -> main)
Author: Roland Christensen <CodingByVoices@gmail.com>
Date:   Thu Mar 14 21:00:42 2024 -0600

    Commented on the test actions page

commit bf9e7a85f94ac80efb7bbb58df192787a26ce599
Author: Roland Christensen <CodingByVoices@gmail.com>
Date:   Wed Mar 13 09:32:37 2024 -0600

    Initial commit
```
The most recent commit is on top marked by (HEAD -> main) That is the HEAD of the main branch.  
The line `commit 278d94fc06df6555dfb9cf0cc9ce94824e361811` with the hash is important for using `$ git reset` to get back to a specific commit if you made a mistake.  
The commit message is displayed below the rest and is the best indicator of what you are resetting. This is why a good commit message is important.  

Just to test things out, make a small change to any file and repeat the add and commit procedure outlined above.  
When you `$ git log` again you will see two commits, each with a unique descriptive name.  

Maybe even repeat the process a couple more times, so you can learn about undoing things.  

# Time Traveling in Git
You can move everything back to a specific commit at any time.  

Remember: Don't Panic!!! You can always undo anything.  

## Reset using hashes found in Git log
Using the above git log I can use the hash found in the "Initial commit" to go all the way back to that commit.  
`$ git reset --hard {hash}`  
Example `$ git reset --hard bf9e7a85f94ac80efb7bbb58df192787a26ce599`  
```
$ git reset --hard bf9e7a85f94ac80efb7bbb58df192787a26ce599
HEAD is now at bf9e7a8 Initial commit
```

After executing the above command you will find the file(s) you changed in the subsequent commits *reset* back to the original.  
Another git log will show only the original commit.  
```
$ git log
commit bf9e7a85f94ac80efb7bbb58df192787a26ce599 (HEAD -> main)
Author: Roland Christensen <CodingByVoices@gmail.com>
Date:   Wed Mar 13 09:32:37 2024 -0600

    Initial commit
```

## Git Reflog (Reference logs)
`$ git reflog` shows the logs of any changes to the tip or HEAD of the branch including *resets*.  
Note: `$ git reflog` is equivalent to `$ get reflog show HEAD` as *show* and *HEAD* are the default values.  
```
$ git reflog
bf9e7a8 (HEAD -> main) HEAD@{0}: reset: moving to bf9e7a85f94ac80efb7bbb58df192787a26ce599
278d94f HEAD@{1}: commit: Commented on the test actions page
bf9e7a8 (HEAD -> main) HEAD@{2}: commit (initial): Initial commit
```

## Reset back to a specific commit using Git reflog
Add another commit by quickly adding a comment or something trivial to a file to test Git reflog and reset.  

```
$ git reflog
f925f8f (HEAD -> main) HEAD@{0}: commit: Added another comment to make a change.
bf9e7a8 HEAD@{1}: reset: moving to bf9e7a85f94ac80efb7bbb58df192787a26ce599
278d94f HEAD@{2}: commit: Commented on the test actions page
bf9e7a8 HEAD@{3}: commit (initial): Initial commit
```  
You can get back to any place in the log by noting the HEAD@{#}  
`$ git reset --hard HEAD@{#}`  
Example: `$ git reset --hard HEAD@{1}` to get back to the point where you reset the last commit.  
```
$ git reset --hard HEAD@{1}
HEAD is now at bf9e7a8 Initial commit
```

Git log will only show the intial commit:  
```
$ git log
commit bf9e7a85f94ac80efb7bbb58df192787a26ce599 (HEAD -> main)
Author: Roland Christensen <CodingByVoices@gmail.com>
Date:   Wed Mar 13 09:32:37 2024 -0600

    Initial commit

```

Git reflog will show all the changes of the HEAD pointer.  
```
$ git reflog
bf9e7a8 (HEAD -> main) HEAD@{0}: reset: moving to HEAD@{1}
f925f8f HEAD@{1}: commit: Added another comment to make a change.
bf9e7a8 (HEAD -> main) HEAD@{2}: reset: moving to bf9e7a85f94ac80efb7bbb58df192787a26ce599
278d94f HEAD@{3}: commit: Commented on the test actions page
bf9e7a8 (HEAD -> main) HEAD@{4}: commit (initial): Initial commit
```

# Rewriting History (Amending the last Commit)
Often, you just want to add an additional comment or one last unit test to a previous commit.  
You can always add another commit with the changes, but that can become problematic. If you want to reset to a specific commit later as shown above, you may end up overrwriting that last little commit with the updated comments or unit test because the log is not clear when the feature was done.  

Best Practices:  
* Divide your work into logical pieces that are self-contained units to be *committed* in one logical commit.
* Clearly name your *commits* so you and others know what was done.

Now, to ammend the last commit:  
`git commit --amend` will add recent changes staged into the last commit as if the previous commit never happened.  

***Warning***: This should not be done if you have pushed it to a public server others may have pulled from.  
This approach is used to update local changes only!  

1. Find a place to add a harmless, but useful comment in the code.
2. Update the last commit with something like: `$ git commit --amend -m "Some new commit message"`

Below is the output of Git log for my example repo, still only showing one commit, but containing the comment change with the new commit message.  
```
$ git log
commit b9c9dd7c2a1a3f5afaebffdf2138ead7f244663a (HEAD -> main)
Author: Roland Christensen <CodingByVoices@gmail.com>
Date:   Wed Mar 13 09:32:37 2024 -0600

    Issue 19: Initial commit

```

Note: you can use *amend* to update the message only.  
`$ git commit --amend -m "Initial commit"`
```
$ git log
commit b580f83e01153cffbaa145d7bc37548885e500f8 (HEAD -> main)
Author: Roland Christensen <CodingByVoices@gmail.com>
Date:   Wed Mar 13 09:32:37 2024 -0600

    Initial commit
```

You can use the `--no-edit` flag to amend a commit without changing the message.  
`$ git commit --amend --no-edit`

Add another harmless, but useful comment and try it out.  

# Using branches
Use branches to separate work into logical changes (issues/stories/features).  
You can also use branches in local development to try out different approaches to the same issue, keeping both isolated from each other. Once you make a decision, you can remove one branch easily keeping the best solution untouched.  

`$ git branch`: lists all branches. (Equivalent to `$ git branch --list` since `--list` is the default flag.  

## Creating Branches
A branch takes all the code you are currently working with and split it into two identical branches.  
You then switch to the new branch to make changes, leaving the original intact for safety.  

`$ git branch branch-name`: creates a new branch named "branch-name" based on the branch you are currently on.  
`$ git checkout branch-name`: switches to the branch, so all changes will be recorded on that branch going forward.  
`$ git checkout -b branch-name-alt`: creates a new branch with "branch-name" and switches to that branch in one command.  

Example: after using the commands above to create two branches named "test-branch" and "test-branch-alt".  
Executing `$ git branch` displays all the branches including *main*.  The asterisk lets you know which branch you are currently working on. 

```
$ git branch
  main
  test-branch
* test-branch-alt
```

1. To test how this works add a comment or other trivial code to a file in the current branch, such as "\\ This is a comment on branch branch-name".
2. `$ git add .` and `$ git commit -m "Add comment to branch branch-name"` 
3. Use `$ git branch checkout branch-name` to switch to another branch and when you check the code you should see the comment disappear.
4. `$ git log` to see the commits. You should see commits from *main* up to the point you checked out the branch, but not see any commits on other branches like the one you just checked out.
5. Checkout the other *test branch* and add another comment to practice.

## Merging Branches
Eventually, you will decide that one of these branches is the solution you want to go with and the other can be discarded.  

1. `$ git checkout main`: to merge you go to the branch you want to merge changes into.
```
$ git checkout main
Switched to branch 'main'
```
2. `$ git merge test-branch`: merge the
```
$ git merge test-branch
Updating 899b6ac..1ddb9d2
Fast-forward
 cypress/e2e/test-querying-page.cy.js | 3 +++
 1 file changed, 3 insertions(+)
```
3. `$ git log` to see the changes on the main branch now include the changes we made on the branch.

## Deleting Branches
Once you have merged or decided you will not accept the changes made on a branch you will want to delete them to clean up.  

`$ git branch -d branch-name`: deletes the branch.

Go ahead and delete any extra braches now and do a Git branch between them to verify they are gone.  

# Git Diff (Double check your work)
My favorite algebra teacher instilled in me the need for double checking my work. Use Git Diff to see the changes in files not yet "staged" for commit.  

To try it out add a comment somewhere, delete a comment somewhere, and change a comment somewhere.  
Before adding the changes, do a diff to make sure there where no accidental changes made.  
`$ git diff`: displays a diff of the changes made to files.  

The Git Terminal uses color coding and is easier to read than this.  
Deletions are in red and are preceeded by a '-' sign.  
Additions are in green and are preceeded by a '+' sign.  
Any lines that are modified have two lines the line that was replaced beginning with a '-' and the line that replaces it preceeded by a '+'.  
Lines that do not begin with either a '-' or '+' are untouched.  

```
$ git diff
diff --git a/cypress.config.js b/cypress.config.js
index de886c6..a13c7f0 100644
--- a/cypress.config.js
+++ b/cypress.config.js
@@ -3,8 +3,10 @@ const { defineConfig } = require("cypress");
 module.exports = defineConfig({
   e2e: {
     setupNodeEvents(on, config) {
-      // The environment will default to 'local' if you do not set the environmentName when starting cypress
+      // The environment will default to 'local' if environmentName is not set when starting cypress
       // Start cypress using: $ npx cypress open --env environmentName={dev|stage|prod}
+      // Secrets are stored in (local.settings.json, dev.settings.json, etc..)
+      // Make sure to add settings files with secrets into .gitignore file.
       const environmentName = config.env.environmentName || 'local'
       const environmentFilename = `./${environmentName}.settings.json`
       const settings = require(environmentFilename)
@@ -13,10 +15,6 @@ module.exports = defineConfig({
       }
       return config

-      // Remember to add any secrets file to your .gitignore file
```

https://www.atlassian.com/git/tutorials/rewriting-history



# Git Fork
Use `git fork` when you do not have permission to contribute to the repository.

A *clone* of a repository assumes you can create a pull request and modify the original repo, while a *fork* assumes you will be taking a fork in the road and all subsequent changes are separate from the parent repo. "Thanks for the start, I really appreciate it, but I want to take this in my own direction."
