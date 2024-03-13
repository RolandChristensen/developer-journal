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



https://www.youtube.com/watch?v=a3Qhon09JEw



## Git Fork
Use `git fork` when you do not have permission to contribute to the repository.

A *clone* of a repository assumes you can create a pull request and modify the original repo, while a *fork* assumes you will be taking a fork in the road and all subsequent changes are separate from the parent repo. "Thanks for the start, I really appreciate it, but I want to take this in my own direction."
