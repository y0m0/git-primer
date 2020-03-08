# Git Primer
<p align="center">
<img src="https://imgs.xkcd.com/comics/git.png" alt="img" width="400" />
</p>


## The Basics

### Create a new repository

New repos can be created from Github.com, just click on "New".
Fill in the new repo name, in my case it will be **git-primer**, and select **private**.

Once done creating the new repository, on your machine create a new directory for your project.
Go inside the newly created directory, and then issue the following commands to initialize it as a git repository, and to connect it to the Github repository we previously created.

```
git init
git remote add origin git@github.com:y0m0/git-primer.git
```
Done, you can now start to commit away.

### Clone an existing repository

If the repository already exists from before you can just download a local copy to your machine.

```
git clone git@github.com:y0m0/git-primer.git
```
if you don't have ssh setup on your github account, use the https version of the link instead
```
git clone https://github.com/y0m0/git-primer.git
```

### A good first commit

Every good repository needs a readme file, so we can start by creating one.
Just create a file called README.md. Inside this file write a description for the repository and save it.  

We can check the current status of the repository
```
git status
```
This will list wich files are staged, unstaged and untracked.
In our case it will show that README.md is unstaged.

In order to save it to the repository we need to:
1. Stage our changes
2. Create a commit with a meaningful message
3. Push them changes to the repository.

```
git add README.md
```

If you have multiple files to commit you can use the following command to tell git to stage them all at the same time
```
git add .
```

After staging the files we are now ready to create a commit and push it to the repository.
```
git commit -m "first commit"
git push -u origin master
```
*The  -u flag is used only the first time to set the the upstream, more on this [here](https://stackoverflow.com/a/37770744)*.

Go to your newly created repo you should now see the README.md file has been uploaded.

Back on the command line we can also inspect the commit history
```
git log
```
it will list all commits from newest to oldest starting from the top, to scroll press <space> and to exit press <q>

</br>
</br>

## Work with Branches

A Git Workflow is a recipe or recommendation for how to use Git to accomplish work in a consistent and productive manner. There are several well known workflows that can be used, the one that we will focus on is called **Git Feature Branch Workflow**

<p align="center">
<img src="https://www.atlassian.com/dam/jcr:746be214-eb99-462c-9319-04a4d2eeebfa/01.svg" width="500" />
</p>

The **Feature Branch Workflow** assumes a central repository, and master represents the official project history. Instead of committing directly on their local master branch, developers create a new branch every time they start work on a new feature. Feature branches should have descriptive names, like animated-menu-items or issue-#1061. The idea is to give a clear, highly-focused purpose to each branch.

In addition, feature branches can (and should) be pushed to the central repository. This makes it possible to share a feature with other developers without touching any official code. Since master is the only “special” branch, storing several feature branches on the central repository doesn’t pose any problems. Of course, this is also a convenient way to back up everybody’s local commits.

Here is a simple example of how the workflow works:

### 1. Update Master
When starting to work on a new feature / bug or any change to the base code, we first checkout to the master branch and then pull the latest commit to our local version of the repo.
```
git chekcout master
git fetch origin
```
### 2. Create a new branch
Use a separate branch for each feature or issue you work on. After creating a branch, check it out locally so that any changes you make will be on that branch. The -b flags is used to create a new branch if it doesn't already exists.
```
git checkout -b feature/duty-report
```

### 3. Update, add, commit, and push changes
```
git status
git add <filename>
\\ or to stage all changed files at the same time
git add .
git commit -m 'my commit message'
git push -u origin feature/duty-report
```
*the -u flag adds it as a remote tracking branch. After setting up the tracking branch, git push can be invoked without any parameters to automatically push the new-feature branch to the central repository*

### 4. Create a pull request
Once someone completes a feature, they don’t immediately merge it into master. Instead, they push the feature branch to the central server and file a pull request asking to merge their additions into master. This gives other developers an opportunity to review the changes before they become a part of the main codebase
To create a pull request follow this steps:
1. On GitHub, navigate to the main page of the repository.
2. In the "Branch" menu, choose the branch that contains your commits.
3. To the right of the Branch menu, click New pull request.
4. Use the base branch dropdown menu to select the branch you'd like to merge your changes into, then use the compare branch drop-down menu to choose the topic branch you made your changes in.
5. Type a title and description for your pull request. Additionally, on the description, you can add a reference number to an open issue which this pull request is related to. 
6. Add one or several reviewers, the assignees, and eventually a link to a project.
7. Click on Create Pull request to submit your pull request for review.
8. Reviewer can eventually add comments to the code or suggest changes.
9. The reviewer approves the pull request, here is **very important** to add the [shipit squirrel](https://github.blog/2012-09-24-how-we-ship-github-for-windows/) :shipit:
10. If there are no merge conflicts we can now merge the pull request and delete the now obsolete branch.

</br>
</br>

## Merging and Rebasing
When done working on a feature branch we will need to merge our changes back to master, we saw how to do this by creating a pull request.
Consider what happens when another team member updates the master branch with new commits. This results in a forked history.
<p align="center">
<img src="https://www.atlassian.com/dam/jcr:e229fef6-2c2f-4a4f-b270-e1e1baa94055/02.svg" width="500" />
</p>

Now, let’s say that the new commits in master are relevant to the feature that you’re working on. To incorporate the new commits into your feature branch, you have two options: merging or rebasing.

### Merge
The easiest option is to merge the master branch into the feature branch using something like the following:
```
git checkout feature
git merge master
```
Or, you can condense this to a one-liner:
```
git merge feature master
```
This creates a new “merge commit” in the feature branch that ties together the histories of both branches, giving you a branch structure that looks like this:
<p align="center">
<img src="https://www.atlassian.com/dam/jcr:e229fef6-2c2f-4a4f-b270-e1e1baa94055/02.svg" width="500" />
</p>

Merging is nice because it’s a non-destructive operation. The existing branches are not changed in any way. This avoids all of the potential pitfalls of rebasing (discussed below).

On the other hand, this also means that the feature branch will have an extraneous merge commit every time you need to incorporate upstream changes. If master is very active, this can pollute your feature branch’s history quite a bit. While it’s possible to mitigate this issue with advanced git log options, it can make it hard for other developers to understand the history of the project.

### Rebase
As an alternative to merging, you can rebase the feature branch onto master branch using the following commands:
```
git checkout feature
git rebase master
```
This moves the entire feature branch to begin on the tip of the master branch, effectively incorporating all of the new commits in master. But, instead of using a merge commit, rebasing re-writes the project history by creating brand new commits for each commit in the original branch.

<p align="center">
<img src="https://wac-cdn.atlassian.com/dam/jcr:5b153a22-38be-40d0-aec8-5f2fffc771e5/03.svg?cdnVersion=866" width="500" />
</p>

The major benefit of rebasing is that you get a much cleaner project history. First, it eliminates the unnecessary merge commits required by git merge. Second, as you can see in the above diagram, rebasing also results in a perfectly linear project history—you can follow the tip of feature all the way to the beginning of the project without any forks. This makes it easier to navigate your project with commands like git log.

Once you understand what rebasing is, the most important thing to learn is when not to do it. The golden rule of git rebase is to never use it on public branches.

See the section below for a more advanced type of rebase, interactive rebase.

</br>
</br>

## Undoing changes

### Changing the Last Commit

Premature commits happen all the time in the course of your everyday development. It’s easy to forget to stage a file or to format your commit message the wrong way. The --amend flag is a convenient way to fix these minor mistakes.
```
git commit --amend
```
or if you dont want to edit the commit message
```
git commit --amend --no-edit
```
Don’t amend public commits!

Amended commits are actually entirely new commits and the previous commit will no longer be on your current branch. This has the same consequences as resetting a public snapshot. Avoid amending a commit that other developers have based their work on. This is a confusing situation for developers to be in and it’s complicated to recover from.

### Reset A Specific Commit

Resetting is a way to move the tip of a branch to a different commit. This can be used to remove commits from the current branch. For example, the following command moves the hotfix branch backwards by two commits.
```
git checkout hotfix
git reset HEAD~2
```
The two commits that were on the end of hotfix are now dangling, or orphaned commits. This means they will be deleted the next time Git performs a garbage collection. In other words, you’re saying that you want to throw away these commits. This can be visualized as the following:

<p align="center">
<img src="https://wac-cdn.atlassian.com/dam/jcr:4c7d368e-6e40-4f82-a315-1ed11316cf8b/02-updated.png?cdnVersion=866" width="500" />
</p>

This usage of git reset is a simple way to undo changes that haven’t been shared with anyone else. It’s your go-to command when you’ve started working on a feature and find yourself thinking, “Oh crap, what am I doing? I should just start over.”

In addition to moving the current branch, you can also get git reset to alter the staged snapshot and/or the working directory by passing it one of the following flags:
```
--soft – The staged snapshot and working directory are not altered in any way.

--mixed – The staged snapshot is updated to match the specified commit, but the working directory is not affected. This is the default option.

--hard – The staged snapshot and the working directory are both updated to match the specified commit.
```

### Undo Public Commits with Revert
Reverting undoes a commit by creating a new commit. This is a safe way to undo changes, as it has no chance of re-writing the commit history. For example, the following command will figure out the changes contained in the 2nd to last commit, create a new commit undoing those changes, and tack the new commit onto the existing project.
```
git checkout hotfix
git revert HEAD~2
```

<p align="center">
<img src="https://wac-cdn.atlassian.com/dam/jcr:73d36b14-72a7-4e96-a5bf-b86629d2deeb/06.svg?cdnVersion=866" width="500" />
</p>

Contrast this with git reset, which does alter the existing commit history. For this reason, git revert should be used to undo changes on a public branch, and git reset should be reserved for undoing changes on a private branch.

</br>
</br>

### Interactive Rebase(pick, edit, squash and more)

Interactive rebasing gives you the opportunity to alter commits as they are moved to the new branch. This is even more powerful than an automated rebase, since it offers complete control over the branch’s commit history. Typically, this is used to clean up a messy history before merging a feature branch into master.

To begin an interactive rebasing session, pass the i option to the git rebase command:
```
git checkout feature
git rebase -i master
```

This will open a text editor listing all of the commits that are about to be moved:
```
pick 33d5b7a Message for commit #1
pick 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3

# Rebase 33d5b7a..5c67e61 onto 710f0f8
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

This listing defines exactly what the branch will look like after the rebase is performed. By changing the pick command and/or re-ordering the entries, you can make the branch’s history look like whatever you want. For example, if the 2nd commit fixes a small problem in the 1st commit, you can condense them into a single commit with the fixup command:
```
pick 33d5b7a Message for commit #1
fixup 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

When you save and close the file, Git will perform the rebase according to your instructions, resulting in project history that looks like the following:

<p align="center">
<img src="https://wac-cdn.atlassian.com/dam/jcr:fe6942b4-7a60-4464-9181-b67e59e50788/04.svg?cdnVersion=866" width="500" />
</p>

Eliminating insignificant commits like this makes your feature’s history much easier to understand. This is something that git merge simply cannot do.

</br>
</br>

### Chrerry Pick

[cherry pick](https://www.atlassian.com/git/tutorials/cherry-pick)
