# Git Primer

<img src="https://imgs.xkcd.com/comics/git.png" alt="img" style="zoom:70%;" />


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

</br>
</br>

## Work with Branches

A Git Workflow is a recipe or recommendation for how to use Git to accomplish work in a consistent and productive manner. There are several well known workflows that can be used, the one that we will focus on is called **Git Feature Branch Workflow**

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

## Undoing changes

[Undoing Commits](https://www.atlassian.com/git/tutorials/undoing-changes)

</br>
</br>

## Rebase

[rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)

</br>
</br>

## Rebase vs Merge

[rebase vs merge](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

</br>
</br>

## Revert, Reset and Checkout

[revert reset checkout](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)

</br>
</br>

### Chrerry Pick

[cherry pick](https://www.atlassian.com/git/tutorials/cherry-pick)

