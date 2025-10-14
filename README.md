# Git Workshop
The goal of this document is to take a person who has little understanding of git, and bring them to a point where they are able use git proficiently. I want to remove the fear that I so often felt when using git.

## Resources:
- Git branch lessons and graphic visualization: https://learngitbranching.js.org/
- Git book: https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository
- An explanation of the "behind the scenes" of git https://jwiegley.github.io/git-from-the-bottom-up/

## Goals:
By the end of this workshop you should have a solid understanding of:
1. Version control basics
2. Initializing git repositories
3. Making and tracking changes using `add` and `commit` commands
4. Knowledge of branch manipulation
5. Collaboration with remote repositories
6. Resolving conflicts
7. Workflows for both individual and group work

## TLDR:
- If you read nothing else, this will get you by so that you can at least contribute to your group project:
- To make changes to a group repo, follow the workflow:
```
git pull
git checkout devBranch
git merge main
* Make changes *
git commit -am "make change"
git push origin devBranch
make pr on github
```
- Do not make changes without pulling from origin
- Push to your remote branch (NOT MAIN)
- Do not make Pull Requests that are that do not function/work

## Welcome - from the workshop
Welcome to the **Git Workshop**! This repository will serve as the base for our workshop activities.
Note that this workshop assumes you already have git installed and connected to your GitHub account

## Introduce Yourself to Git
```
git config --global user.name "Name Here"
git config --global user.email "yourEmail@gmail.com"
```
- *If you have never used git before on your machine*, then you need to introduce yourself!
- We need to tell git who we are. More specifically, we need to tell git which name and email we want to associate with our commits and our account
- Here were configuring a global configuration key, thus it will apply to all git repository's from now on
- you can view all your git config settings using:\
`git config --list`
- Or you can view specific config values:\
`git config <key>`\
`git config user.name`

## Init a Git Repo
- Git will only work if we tell it which repositories we want it to track. 
- Navigate to the repository you want to track
- Tell git that we want it to track this file\
`git init`
- This will create a `.git` file within your repo. Note that it is a hidden file
- Your first time initializing a repository, git may ask you if you want to rename your default branch from `master`. Follow the provided instructions to change it to `main`
- For more information about branches, and what this is doing, see the branch section

## Changes
 There are some handy commands we will be using this tutorial that speeds up the process of changing files
 ```
touch filename.txt                      (Creates an empty file)
rm filename.txt                         (Deletes the file Careful, it will not ask for permission and is unrecoverable)
cat filename.txt                        (print a file to the terminal)
echo "A line of text" >> filename.txt   (Appends (adds) the line of text to the end of the file, or creates a new file if filename.txt does not exist)
    - Note: if you use a single arrow: > instead of a double >> your entire file will be rewritten with just your new line of text remaining so be careful!
```

- Assuming that it is an empty repository, there is nothing for our git to track. Let's create a file and start tracking it\
`echo "hello" >> a.txt`

- Using the append command we have created our first, file, a.txt
- There has now been a change to the repository!

### Git status

- `git status` is our best friend. We will be using it a lot. 
- In a way, git status is our home base. It provides general information about our repo, and about the files git is managing
- It is here we start to discuss the git workflow
```
git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	a.txt

nothing added to commit but untracked files present (use "git add" to track)
```

**Untracked Files**
- Untracked, as one would expect, are not tracked by git. Git only tracks files that we explicitly tell it to. 
- Currently, our `a.txt` file is untracked, thus if we were to delete our file, the file and our changes would be lost.
- `git add a.txt` This now adds our file to the staging area.

## Git Add and the Staging Area 
```
git add . (Take a snapshot of your entire reposity and adds it to the staging area)
git add * (Add all new and modified file, does not stage hidden files, does not stage file deletions)
git commit -a (Skip the staging step. Add all files that have been changed/deleted. Does not add untracked files to stageing area)
```
- Note on file deletions. If deleting a tracked file locally,(`rm filename.txt`), we need to tell git that changes have been made to the file. Use `git add filename.txt` to stage individual deletions to the staging area

- Using `git add` on an untracked file will tell git to start tracking that file, and add its current state to the **staging area**
- The staging area is where files are put, ready to be used in a commit. 
- The staging area is a snapshot of the file, at the time it is added.
- If further changes of the file are made, we need to update this snapshot, by repeating `git add a.txt`
    - One way to picture the purpose of the staging area, is to think of it as the counter at a restaurant the chef's put the food on for the servers to take to the customers,
    - If the food needs changes (perhaps the chef forgot some salt) changes can still be made to the plate. (using `git add filename` again after changes have been made)
    - If the food (file) is a mistake, and never should have been tracked, it can be removed from the counter perminently with `git rm --cached filename`
    - If it's ready to be taken away, we can commit it, and have the server take the plates away.

---

## Git Commit
```
git commit  (opens text editor to add details about commit)
git commit -a (adds all tracked files to staging area and opens text editor)
git commit -m "Commit message" (commit message in command, does not open text editor)
git commit -am "Commit Message" (Adds all tracked files into staging area, inline commit message)
```

- Let's suppose we are in a place with our repository that we want to save. 
- To be able to come back to this exact point, let's make a commit\
`git commit -m "add file a.txt to the repository"`
- We've now created a commit, a snapshot of all our tracked file at the point of the commit
- Each commit has a commit ID that can be called if we ever want to view this commit later.
- If we ever want to call the ID, we don't have to use the entire commit ID value, just enough to provide uniqueness. A minimum of 4 values. 

---
- Let's create another change, and another commit so that we have a second commit
```
echo "second commit text" >> a.txt
git commit -am "update a.txt"
```
- Here we are combining the -a and -m options, thus through our commit message we are adding any changes in all our tracked files, and adding the commit message within a single line. Very handy!
    - Note that the `-a` option does not add untracked files to the staging area.
- Let's see what happens if we want to go back to our old commit point.

## Git Log
```
git log (Show past commits in full)
git log --oneline (Show one line for commit)
git log -5 (Only show the logs of the last 5 commits)
git log --oneline --graph --all --decorate (--graph show a graphical version of the commit history, --all shows all branches, --decorate labels branches)
git log -S "string" (show logs that have changed any lines that contain the string)
git log -patch (or -p for short, Shows line changes within log)
```
- `git log` allows us to view the commit history of a git repository. It provides a lot of in depth information, however it is not very useful as an out of the box tool. 
- Let us add the following options:\
`--oneline` Shorten the commit to just 1 line\
`--all` Show all branch paths\
`--graph` Visually show the path of the commit history\
`-5` Limit the output to show the last 5 commits in the history (very useful!)

- Thus let's run our new command:\
`git log --oneline --all --graph -5` 
- Note that since we only have 2 commits, our limit of 5 does nothing

- For a very comprehensive git log that provides oneline, graph, color, relative date of commit, commit hash, and commit message, create the following alias:
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
- We can now use `git lg` to call a lovely looking git log graph! 
- Note that this does not have --all in it, and it can be included if you would like, or just add it when you want to see paths of all branches
`git lg --all -10`


## Git Checkout
```
git checkout BranchName (Set branchName as current branch)
git checkout -b newBranch (Create newBranch and set as current branch)
git checkout commitID (checkout a specific commit, replacing commitID with first 4-5 values of the commit Hash value)
```

- We can now make a note of the first commit ID, and let's view the repository at a certain point in history\
`git checkout 'commitID'`
- We've now disconnected from the main branch, and are now a floating HEAD.\
**HEAD** - A side note:
- `HEAD` is the pointer to whatever commit we are currently observing.
- Normally `HEAD` is attached to a branch, e.g. main, which it automatically tracks and remains unseen. 
- When we checkout a different branch, we are moving our HEAD to look at the commitID that the branch is currently pointing to. 
- However, we can disconnect our HEAD pointer from the branch and take a look at other commits.
- `cat a.txt` shows that we are looking at a version of our file at the point of the first commit.
- In general, don't make any changes while floating. If you really need to make changes from a certain point in history, create a new branch at that point, and make changes from here, then merge into main.
- `git checkout main` reattach our HEAD pointer to follow the main branch 
- Check https://learngitbranching.js.org/ for a great visualization of what branches are doing within git, and how we can move the HEAD pointer to different commits in the commit history

## Branches (Finally)
```
git checkout branchName  (swap to sepcified banche)
git checkout -b NewBranchName  (create new branch and move to it)
git branch  (lists local branches)
```
- At long last we get to branches. 
- Using a git branch is a way of making experimental changes to a repository while also maintaining a working copy
- Suppose you have to present an update to your class, and must show a working prototype. The main branch of the git repo is working and we dont want to effect it, but we still want to keep working on the project. What do we do?
- We make a branch!\
`git checkout -b newBranchName`
- We now have a branch that we can make changes to, make commits to, and progress the project further, all while keeping the working copy of the file ready for the demo.
- Once we finish our demo, we can easily merge our experimental branch to our main branch, and continue working from where we left off.

### Main

- Say it with me: "**Main is holy**". Main should always be a working copy of your project. If main is not functional, something has gone very wrong. 
- Code updates should be performed on development branches, and only once they are function should they be merged into main.
- Note that in order to merge a branch into main (or update main with a branch's changes) we first checkout main, then merge the branch using `git merge devBranch`

### Branch Workflow
- Thus, when we want to make changes, follow the following workflow
```
git checkout -b bugFix
echo "within bugfix" >> a.txt (make changes within bugFix)
git commit -am "fix bug"
echo "second commit within bugifx" >> a.txt (Maybe fix a second bug)
git commit -am "fix second bug"
git checkout main
git merge bugFix
git branch -d bugFix
```
Let's break down what's happening:
1. Create and checkout a new branch called bugFix
2. We make our changes within the branch
3. Add and commit our first fix
4. Fix the second bug in the code
5. Commit the second bug fix
6. Now that our fixBug branch is working perfectly, we checkout main to merge the changes
7. Merge the changes from bugFix into our main branch
8. Now that we have fixed our bug, we can delete or bugFix branch. All this does is remove the pointer to this commit, the commit history still exists. We just want to clean up our git and keep things organized.

## Working with remotes:
- Now the fun begins. How do we work as a team?
- First let's talk about the connection between our local and remote repositories
- I want you to fork this repo on GitHub, creating your own version of the repo, 
    - A forked repo is now yours, you can do whatever you want to it (within the laws of the license). 
    - Your changes will not affect the original repo unless you make a request to pull your changes into the original repo
- Next, create a branch in your new GitHub repo: `yourNameDev`
- Then clone the repo to your local machine using the ssh download key 
- Click the big green `"<>Code"` button, select SSH, and copy the text
- In a terminal, or on Windows a Git Bash terminal, navigate to where you want to copy the folder to. 
```
cd ~/Documents/
git clone git@github.com:DannyPlatt/gitWorkshop.git   (paste the text here)
cd gitWorkshop/
```
- `git clone` copies a remote repository into your file system and initializes the repository
- When running `git branch` you'll notice we only have main. Why? where is our other branch we've made?
- Running `git branch -a` will show all repositories, remote and local
- Local main branch is set to track the remote main branch (remote/origin/main is the upstream of our local main)
- To create our own local version of the remote dev branch, we must create a branch localy with the same name
```
git checkout -b yourNameDev
```
- We can now make changes to yourNameDev branch. Push them to our remote branch.
- Once you feel like your dev branch is ready, you can create a PR to merge it into the main branch of your remote repo
- Then, if your feeling really bold, you can contribute your changes to my repository. 
    - Note that you must request that I pull your changes into my repository, as you are not set as a contributor, and cannot directly make changes to my repo
- Thus the workflow is as follows:
```
git pull origin main        (check for changes in the remote main branch, and merge them into your local/main branch)
git checkout yourNameDev    (work should never be performed on the main branch)
* Make your changes to the files *
git commit -am "Make changes"
git push origin yourNameDev (push changes to your remote branch)
```
- When ready, create a Pull Request on GitHub to merge your branch into main

## Avoiding Merge Conflicts
```
git checkout devBranch
git merge main
echo "change in dev" >> file.txt
git commit -am "change in dev"
git checkout main
git merge devBranch
```
- Merge conflicts can only occur when commits are made on two or more separate branches.
    - Thus, when working alone it is unlikely you will run into merge conflicts
- However, the following still applies whether alone or working in groups:
- To avoid Conflicts, always pull or merge from main before working to get the latest updates.
0. Update your main branch with the remote main in case anyone has made changes
1. Checkout your development branch
2. Merge any changes from main into your branch to ensure it is up-to-date
3. Begin working, make your changes
4. Commit your changes
- Then depending on if your working locally or with a group, your steps split
- Locally:
    - Checkout main branch
    - Merge your changes into main
- Using GitHub:
    - Push to your remote dev branch
    - Make a Pull Request to merge changes

- Communication is key. Let people know what file you're going to change, and when you're working on it.
- Avoid working on the same file if possible. 
- Two different people should not be fixing the same problem.
- These rules apply to working with remote repositories as well. 

## Merge Conflicts
```
CREATE A MERGE CONFLICT:
git checkout -b bugFix
echo "hello" >> a.txt
git commit -am "Fix bug in bugFix"
git checkout main
echo "Goodbye" >> a.txt
git commit am "Goodbye from main"
git merge bugFix
```
- This will lead to the following:
```
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt
Automatic merge failed; fix conflicts and then commit the result.
```
**Do not panic!**
- During merging, Git is pulling in changes from both branches and trying to combine them.
- Conflicts arise when we change the same lines in a file from 2 different branches.
- Running `git status` tells us that we have conflicts to solve
- Git helps us along the way, and gives us instructions on what to do:
```git
git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
- Let's fix our conflicts.
- If we print our a.txt file we'll notice that the file looks very different\
`cat a.txt`
```git
* old text *
<<<<<<< HEAD
Goodbye from main
=======
hello
>>>>>>> bugFix
```
- Here git is telling us exactly what is wrong!
- Everything from `<<<<<<<< HEAD` to the `========` is from the main branch (where our HEAD currently points)
- Everything from `========` to the `>>>>>>>> bugFix` is from the bugFix branch 
- To fix this, open the file within a text editor of your choice.
- Navigate to the problematic lines and replace the whole section (from the `<<<<<<` to the `>>>>>>`) with your fix:
```
* old text *
hello and Goodbye from main
```
- Stage the file
```
git add a.txt
```
- Using `git add` will mark that file as resolved.
- Since we only had 1 file conflict, the conflict can now finish our merging process
- We need to make one more commit. Why? Because changes to files have occurred during our resolution process
```git
git commit -m "reslolve conflict merge with bugFix and Main"
```
- One slight note, we have pulled the changes from bugFix into main, but have not pulled changes from main into bugfix
- What does this mean? BugFix has not been update with our latest mergee conflict fix commit
- `git log --oneline --decorate --all --graph` shows that main is ahead of bugFix by one commit
```git
*   467eb2d - (HEAD -> main) fix conflict (2 seconds ago) <DannyPlatt>
|\  
| * b492774 - (bugFix) commit from bugFix (28 minutes ago) <DannyPlatt>
* | 526ec4f - Commit from main to create conflict (27 minutes ago) <DannyPlatt>
```
- This is fine if we are deleting bugFix, however if we intend to keep it around, we must update it to match the changes from main before making changes
```
git checkout bugFix
git merge main
```
- Now the git log will show both branches on the same commit
```
*   467eb2d - (HEAD -> bugFix, main) fix conflict (26 seconds ago) <DannyPlatt>
```
Congradulations, crisis averted.
## How to Really Learn Git
- If you've come this far, congrats! I imagine you have learned a lot, and likely forgotten most of it.
- So, if how do you actually learn git? Well, I can only tell you how I learned it. ChatGPT
- I had attended a few seminars, I had practiced with git visualizers, I had even read a good chunk of the book, but it wasn't until I decide to sit down with chatGPT for an hour every night that I learned it. 
- Each night I picked a new topic, and asked it to explain it to me. Run through problems for me to practice. Only then did all the theory finally stick in a practical and usable way. 
- I HIGHLY encourage you to create a GitHub repo called gitPractice, and just play with it the way we've done here. Create files, delete files, commit, create branches, MAKE CONFLICTS, fight with them, solve them. This is how you learn. 
- Also, iF you find splling erorrs or gramer suggestions, please fix them and send a PR outlining you're chnges!
