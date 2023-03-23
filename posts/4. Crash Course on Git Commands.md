# Crash Course on Git Commands.md

Welcome to the wonderful word of git where a `-f` during a `git push` can cause you so much pain. If you are working as a Software Developer you most likely have to use `git` at some point. [Git](https://git-scm.com/) is a version control system that many teams use to collaborate and build software together. 

In this article I am going to go over what I think are essential git commands you will need to know to get started. 
There is more documentation available all for free [here](https://git-scm.com/doc) and I recommend giving it a read whenever trying out new commands. [Atlassian](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud) has some great easy to digest tutorials as well.  

---

```
git clone your-repository-path
```
This is the command you use to download your remote repository. This can be done via ssh or https. 

---

```
git checkout branch 
```
Git uses branches to store work and with this command you will will be able to change your local files to match a specified branch.

---

```
git checkout -b new-branch
```
Creates a new branch from the current branch you are working in. If you wish to specify a specific branch to branch from, you can use the `u` flag. Example: `git checkout -b new-branch -u origin/main`. 

---

```
git status
```
This will simply give the current state of your branch. It will display what changes are staged for a commit, which are not and what is not tracked currently by git. If you are pointing to a specific branch it can also tell how far ahead/behind you are from the the given branch. 

---

```
git pull 
```
Use this if you wish to "pull" in code from remote repository's branch into your current local branch. By default this will perform a merge commit. If you have conflicts git will be sure to flag them. Once resolved you can just `git add` and `git commit` to complete the pull. 

---

```
git pull --rebase
```
Similar to the above command but instead of doing a merge commit this will attempt to rebase the commits from the remote branch on top of the local commits in your branch. If you are not sure which to use a merge commit tends to be less dangerous/easier to deal with than a rebase which acts a lot like a more automated/fancy cherry picking. 

---

```
git cherry-pick commit-hash
```
On the note of cherry picking, if you wish to just grab a single commit and put it on top of your current branch this is where cherry-pick is useful. Give it a commit hash and it will add that commit to your branch. 

---

```
git fetch --all
```
Use this command to make sure your local machine is up to date with the remote repository. Try to do this as often as you can to be sure your local branches that you checkout / branch from are up to date. A `git status` is always helpful to know if you are not in sync with the remote repository. 

---

```
git stash
```
This will save your local modifications and reverts the working directory to match the HEAD commit. This can be useful if you do not wish to commit the changes but want to go back to a clean state to checkout another branch. When you wish to get back the stashed changes simply do `git stash pop`. 

---

```
git branch -u origin/branch
```
Sometimes you may want to change the branch your branch is pointing to/comparing to. This command will let you do this and when you run `git status` it will tell you if you are ahead or behind. 

---

```
git add filename
```
This will add a new/deleted/modified file which to be staged to then be committed. If you wish to add all changes from your current directory just run `git add .`.  

---

```
git diff
```
By default this will show you what changes have been done compared to the previous commit. However this command can even be used to compare specific files between commits and branches. This is useful to see what changes you have before committing.  

---

```
git commit -m "My commit message."
```
An essential classic. This will create a commit with everything that has been staged after running `git add`. The `-m` allows you to add a message to explain the changes. It is good practice to give as much details as you can but every project/organization tends to have their own policy on commit messages. 

---

```
git push origin branch
```
Use this command once your branch's commits are ready to be pushed to the remote repository. It is worth noting `origin` is just the default alias name for your remote repository but it can be called anything. 

Earlier I mentioned not to use `-f` with `git push`. This is because `-f` stands for "force" and when used will override the remote repository's branch with what is being pushed. Be careful and avoid `-f` as much as possible. 

---

Hopefully these commands help you and this small tutorial is useful in your quest to better understand git. Did I miss any commands you think are essential? Let me know but otherwise happy coding! 
