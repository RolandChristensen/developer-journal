Git and GitHub related notes

# Git for Local Development
Git is awesome for source control and is essential to working in a group, but it is also extremely helpful for local development.  
This documents the command line, but describes basic skills that are important regardless of the tool you use to interface with Git.  
I work on Windows professionally, so use Visual Studio Code or Visual Studio which have great extensions for Git.  

Reasons for using a good local source control strategy
* You can create a branch to experiment and if you do not like the results, you can simply undo all changes in a single step
* You can incrementally make small changes and back up to any step along the way on a test branch
* You can see a diff of any changes made, to prevent accidental changes before pushing to the server


This post covers the basics of:  
`$ git init`  
`$ git status`  
`$ git add`  
`$ git commit`  
`$ git log`  
`$ git reflog`  
`$ git reset`  
`$ git branch`  
`$ git checkout`  
`$ git diff`

Quick Start Scenarios:  
* Starting a git repo with existing code
    1. `$ git init` to initialize a new repo
    2. `.gitignore` to keep unnecessary files and secrets out of source control
    3. `$ git add .`, `$ git add file-name`, `$ git add directory-name` to stage files to be tracked
    4. `$ git commit -m "First commit of existing code"`
* Renaming a branch
    * `$ git branch -m master main`
* Creating a branch in order to keep new work isolated from the main branch before it has been thoroughly tested (double checked)
    1. `$ git status` to verify you are on main branch. if not `$ git checkout main`.
    2. `$ git branch -b new-branch-name` creates a new branch identical to the current branch and 'checks out' that new branch
* View the changes (diff) between files changed before "staging" to double check your work
    * `$ git diff`
* View the changes between staged files and the last commit
    * `$ git diff --staged` or `$ git diff --cached`
* Committing staged changes to the branch. Make commits self-contained, testable units
    * `$ git commit -m "Useful commit message, so you can find these changes in the future"`
* Ammending a previous commit
    * `$ git commit -a -m "Commit message that encompasses all changes, not just the amended changes"`
* Viewing all commits
    * `$ git log` view the commits with the author and date of change
    * `$ git log --oneline` the commits are abbreviated on one line
* View only the commits on the branch since it was created
    * `$ git log --oneline main..branch-name` displays only commits
* Merge branch into *main*
    * `$ git checkout main` switch to the branch you want to merge the changes into
    * `$ git merge branch-name` merge the branch into the current branch you are on
    * `$ git branch -d branch-name`
* You forgot to checkout a new branch and started working on a new feature while still on *main*
    * As long as you have not commited any changes you can simply checkout a new branch.
        * `$ git checkout -b new-branch-name`
    * If you have committed changes follow the directions below to *reset* the HEAD with the `--soft` flag to the point where you started making changes.
        1. `$ git log` or `$ git reflog` to get the hash of the commit to go back to
        2. `$ git reset --soft {hash}`
* You cannot continue working on a feature/branch due to a blocking issue or because a hotfix demands your attention
    * All commits will remain on this branch when you change branches, so you do not need to worry about them
    * Any uncommited changes need to be *stashed*
        1. `$ git stash` to stash files that are currently tracked. If you need to include untracked files, which is probably a good idea to avoid checking it into another branch inadvertantly `$ git stash --include-untracked`
        2. Switch to a new branch and complete the new feature or hotfix knowing all your hard work is waiting for you on the previous branch
        3. When ready to resume work on the original branch you abandoned to work on the other feature
        4. `$ git checkout original-branch-name`
        5. `$ git stash pop` to get the last stash pulled off the top of the stack
* 

* Making changes and viewing the changes in the file to double check your work before commiting it
* Dividing your work into small manageable and unit testable changes (Commits) to log the changes of new work 
* Using branches to try out different algorithms 


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
`$ git commit --amend` will add recent changes staged into the last commit as if the previous commit never happened.  

***Warning***: This should not be done if you have pushed it to a public server others may have pulled from.  
This approach is used to update local changes only!  

1. Find a place to add a harmless, but useful comment in the code.
2. Update the last commit with something like: `$ git commit --amend -m "Some new commit message"

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
