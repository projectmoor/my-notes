## Basic Commands
```
git init
git add .
git commit -m ""
git branch
git branch newbranchname
git checkout -b newbranchname
git checkout branchname
git checkout -
```

Check remote repo config
`git remote -v`

## Merge branch to master (local)
`git checkout master`
`git merge features/navigation`

merge features/navigation branch into master


## Git Status
If we run `git status` in the master branch, and get the message below:
```
Your branch is ahead of 'origin/master' by 2 commits
```
It means there are 2 new commits in our local master branch, thus it is ahead of the remote master branch in Github by 2 commits.

Run `git push remote-repo your-local-branch` to push local commits to Github. Often time you will see this command as `git push origin master`, where origin is the remote Github repo and master is your local master branch.

Once push into Github, you can delete the local branch
`git branch -d features/navigation`

----

## git stash
Put my changes aside, checkout another branch, apply my changes later
First need to stage changes. It's those staged changes that will be stashed. The stashed changes won't be affected due to merge, check out, or create other branch.


How to stash:
`git stash`

`git stash -u`

`git stash push -m ""`


How to view stashed content:
`git stash list`

`git stash show 0`

`git stash show -p 0`


How to apply stashed content back:
`git stash apply 0`

How to create a branch from stash:
`git stash branch new-branch 0`

Take the last stashed content and apply to the current directory
`git stash pop`

Discard one stash:
`git stash drop 0`

Discard all stashes:
`git stash clear`

## Advanced Commands
`git reflog`

`git rebase his-branch`

`git reset flogId`

----

## git rebase
`git rebase` and `git merge` are similar, they help to bring changes from one branch to master.

Scenario
master branch commits: A -> B -> C
his branch: AA(take off from B) -> BB

`git rebase his-branch` 
- turns out to be A -> B -> AA -> BB -> D (D replaced C, rewriting history)

`git rebase -i his-branch` 
- `-i` adds interaction


## How to squash git commits
rebase the last three commits, it will bring you into an interactive page, you can use the commands listed on the page to squash `s` some of the commits
`git rebase -i HEAD~3`

```
pick 27cfb9a commitA
s 27cfb9a commitB
s 27cfb9a commitB
```
Here we applied the squash command `s` to the previous two commits and keep the latest one.
Then you can press `ESC` key and type in `:wq`, this takes you to the next step, where you can choose a commit message from the three commits. Add `#` in front of the commit message will ignore the commit message.

Then you can press `ESC` key and type in `:wq`, done.

You can view your commits by `git log`.