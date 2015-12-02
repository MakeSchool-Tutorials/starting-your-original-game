---
title: "Source Control"
slug: source-control
---     

Source control, also known as version control, is a vital part of any software project. With it, you can keep track of your progress, revert to old versions if you break something in a new release, and work on the same project at the same time as other developers.

#Installing the software
We are going to be using git, a popular source control system. You'll need to download it here.

Once you have it installed, confirm the installation was successful by opening Terminal and typing in:

`git version`

Terminal should print out the version you have installed.

#Git 101
A Git repository is a record of changes and versions to your project. Let's set up a Git repository (or "repo") for your project now.

Open terminal, type in 'cd ', and drag in from Finder your project's folder (make sure there is a space between 'cd' and the folder path). Hit enter. The command 'cd' stands for 'change directory', and allows you to navigate terminal to any folder on your computer, for example, 'cd ~/Desktop' will tell terminal to go to your desktop.

To double check that you're in the right place, type 'ls'. The command 'ls' stands for 'list' and will list the files in the folder that terminal is currently in. You should see your BuildSettings, Projectfiles, and .xcodeproj folders/files listed.

Let's get this folder set up with git! Here are the commands you want to enter:

```
git init
curl https://s3.amazonaws.com/mgwu-misc/git-tutorial/mgwu-git-setup | sh
git add --all
git commit -m "Initial Commit"
```

So what did we just do there? 'git init' created a git repository in that folder. 'curl ...' did some additional configuration / set up. 'git add --all' added everything in that folder to the repository. You could have typed out 'git add \' for every file and folder in there to achieve the same result. Then with 'git commit -m “Initial Commit” you created a snapshot of your project and attached a message to it.

It is good practice to take snapshots of your project at the end of every work session – to do so, you'll need to make sure you're in the right directory in Terminal (you can type 'ls' to be double sure), then type 'git add --all' to add the files in your project to the repository. Once you've git added, type 'git commit -m “some message describing the changes I made'. So, to summarize (replace the angle brackets with the right info):

```
cd <path to project folder>
git add --all
git commit -m "message describing changes"
```

#Now for the real magic
We're going to show you how to store a copy of your project in the cloud! This will allow you to collaborate with others on the same project and recover your project in case your computer fails.

First, go to https://bitbucket.org/account/signup/ and sign up for a free account.

Next, navigate to the repositories tab in the top right and create a new repository. Make sure you keep the repository private and don't forget to select git as the repository type and Objective C as the language for the repository. You can leave the website field blank for now.

Next, select I have code I want to import from the get started panel on your new repository's home screen. Follow those instructions to push a copy of your project to Bitbucket.

Voila! You now how a backup of your project in the cloud!

#Pushing subsequent versions to the cloud
The set of commands you would enter every time you want to take a snapshot of your project and send it to bitbucket are:

```
git add --all
git commit -m "message describing changes"
git push
```

A couple tips: - Push AT LEAST once a day, this ensures you don't lose anything! - Try not to push code that doesn't build / run, if for some reason you need to, make sure to include "BROKEN" or "UNSTABLE" in your commit message.

#Reverting to an Earlier Version
So you messed up. Your code is broken, you don't know why, and you need to revert to a past commit. Watch out - this method will change all local changes you've made since that commit. First, type:

`git log --oneline`

This will print out a list of commits followed by an ID for each one. Let's say the commit you wanted to revert to had 4717a5c as its ID. To revert to it you would type:

`git revert 4717a5c`

This will revert to that commit, undoing all changes you've made since, and it will automatically take a snapshot of the reverted version so Git knows to make the reverted version you just created is the most recent and correct one.

There you go! You're all set up to use Git on your solo project.

#Collaborating with others
WARNING: if you run into major issues with this part (when things aren't working as you expected), make sure to make a copy of your project folder so you don't lose your code as you're trying to work through the issues

First, have one person on your team follow the above instructions and push a version up to bitbucket. MAKE SURE they ran the command:

`curl https://s3.amazonaws.com/mgwu-misc/git-tutorial/mgwu-git-setup | sh`

If they didn't, have them run that command, then push to bitbucket (using the above instructions).

Next, have them invite the other collaborators to the bitbucket repo through the online portal. All collaborators need to copy the URL of this repo:

[Bitbucket screenshot](./1-bitbucket-url.png "Bitbucket screenshot")

Then, in terminal, collaborators need to run the following commands (replace the angle brackets with the right info):

```
cd <path to Kobold2D-2.x.x folder>
git clone <bitbucket url>
cd <name of the folder (this should match the name of the repository)>
curl https://s3.amazonaws.com/mgwu-misc/git-tutorial/mgwu-collab-git-setup | sh
```

'git clone' creates a local clone of the repository. 'curl ...' does some additional config / set up to make collaborating easier.

Now and in the future, when any member of the team wants to push code up to bitbucket, they should run the commands:

```
git add --all
git commit -m "message describing changes"
git pull --no-edit
```

'git pull --no-edit' will automatically pull and merge changes. If 'git pull --no-edit' throws an error (see below), simply use 'git pull' instead

[No edit](./2-no-edit.png "No edit")

Normally this will work magically. If there no merge conflicts (read on to find out about merge conflicts), simply push the code back up to bitbucket with the command:

`git push`

CRITICAL INFORMATION: 2 weird things can happen when you pull:

1. If one person removed a file from the project, and another added a file to the project, you might get into a broken state where Xcode thinks a file should exist, but doesn't know where it is:

[Missing files](./3-missing-files.png "Missing files")

In this case just right click on those files and delete them.

2. Sometimes when you pull, there are merge conflicts (when git can't auto merge the changes, typically when two people changed the same line), terminal will show something like:

[Git Pull](./4-git-pull.png "Git Pull")

WARNING: If there is a merge conflict in the .xcodeproj/project.pbxproj, email support@makegameswith.us, we will screenshare with you and fix the problem. If you decide to be bold and try to fix it yourself ZIP UP THE FOLDER AND MAKE A COPY FIRST, otherwise you could ruin any hope of recovery

If there is a merge conflict in any other file (for example a .h or .m), open the file in xcode, and go through / decide which version of the code to keep:

[XCode Merge Conflict](./5-show-versions.png "XCode Merge Conflict")

The code between \<\<\<\<\<\<\< and ======= is your version of the file, the code between ======= and >>>>>>> is the version from your collaborators. Go through the file and decide which version you want to keep, sometimes you'll want to keep bits and pieces of both. Make sure to remove the lines that include \<\<\<\<\<\<\<, =======, and >>>>>>> and save the file.

After you're done fixing merge conflicts in all files, go back to terminal and run the commands:

```
git commit -m "merged"
git push
```

That's all, and remember, push early, push often!
