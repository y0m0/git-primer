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

[feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow
)

</br>
</br>

## Undoing changes

[cherry pick](https://www.atlassian.com/git/tutorials/undoing-changes)

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

