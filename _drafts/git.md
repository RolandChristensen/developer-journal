GIT and GitHub related notes

# Git for Local Development
Git is awesome for source control and is essential to working in a group, but it is also extremely helpful for local development.

Reasons for using a good local source control strategy
* You can see a diff of any changes made, to prevent accidental changes before pushing to server
* You can create a branch to experiment and if you do not like the results, you can simply undo all changes in a single step

Assumptions:  
You already have Git installed and a folder of source code, currently not under source control, that you can use to experiment with.  
The version of Git I currently have installed is 2.44.0.windows.1.  
I an using Windows, but Git on the command line works the same for Linux and Mac.  
If you are nervous, you can copy an existing folder for experimentation.

# Initialize a New Git Repo
To begin using Git to track local changes made to a folder (Assuming you did not clone another repo):  
1. Navigate to the folder with source code you will be using and open a command prompt in the root directory. (Windows: Shift+Right click root folder and select Git Bash from the menu.)
2. `$ git init`
     A hidden folder called .git will be created in the folder.
     If you made a mistake and the .git folder is not in the root folder, you can simply delete the .git folder and try again.
     Another problem you may encounter is if you added a Git repo under an existing Git repo. Do not nest repos.  

This post is just about local development using Git, so I will not discuss adding a remote repo yet.  

# Git Status 
To see what files have been changed or added to the repo since the last commit.  

`$ git status`

All the files that have been added or modified are displayed. Since you used existing source, you should see every file and folder in the root folder you added.  
Files and folders that are untracked


https://www.youtube.com/watch?v=a3Qhon09JEw



## Git Fork
Use `git fork` when you do not have permission to contribute to the repository.

A *clone* of a repository assumes you can create a pull request and modify the original repo, while a *fork* assumes you will be taking a fork in the road and all subsequent changes are separate from the parent repo. "Thanks for the start, I really appreciate it, but I want to take this in my own direction."
