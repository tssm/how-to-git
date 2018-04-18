# How to Git

## Choose what to keep during a conflict

### While merging

To keep the changes of the branch where you are located in:

```
git checkout --ours <file>
```

To keep the changes of the other branch:

```
git checkout --theirs <file>
```

### While rebasing

It is exactly the other way around 🤷🏻‍♀️

## Choose what to stage

```
git add -p
```

## Copy commits between branches

On the branch where you want to apply the commits:

```
git cherry-pick <commit1-id> <...> <commitn-id>
```

## Create a tag

```
git tag <tag>
```

or for annotated tags:

```
git tag -a <tag> -m "<message>"
```

## Delete a commit

### Keeping the changes

```
git reset HEAD^
```

### Discarging changes

```
git reset --hard <commit-id>
```

Instead of the commit id you can use `HEAD~<n>` to specify how
many commits from head you want to delete.

## Delete a local branch

### Merged

```
git branch -d <branch>
```

### Unmerged

```
git branch -D <branch>
```

## Delete a remote branch

```
git push <remote> :<branch>
```

## Delete a tag

### Local

```
git tag -d <tag>
```

### Remote

```
git push <remote> :refs/tags/<tag>
```

## Delete stashed changes

### All of them

```
git stash drop
```

### A specific one

```
git stash drop stash@{<n>}
```

## Diff a file across branches

```
git diff <branch1> <branch2> <file>
```

If one branch is `HEAD` then `..` can be used as a shortcut, for
example asuming `branch1` is `HEAD` then:

```
git diff ..<branch2> <file>
```

## Diff stashed changes

### All of them

```
git stash show -p
```

### A specific one

```
git stash show -p stash@{<n>}
```

## Edit a commit

First stage your changes and then:

### The most recent one

```
git commit --amend
```

or

```
git commit --amend --no-edit
```

if you don't want to change the commit message.

### Any other

Copy the id (or the first 7 characters) of the oldest commit you
want to edit and then run:

```
git rebase --interactive <id>^
```

This will launch your editor. Change the word _pick_ to _edit_ in
front of the commits you want to edit and then save and quit. Now
you are free to work as usual. Once you are ready stage the
changes and commit them with the `--amend` option just as if you
were editing your most recent commit. After that run:

```
git rebase --continue
```

You could need to resolve conflicts as a result of the previous
edition. Once everything is ok you will be ready to edit the other
chosen commits.

## Get remote changes

[Do not `git pull`][do-not-pull]. Do instead:

```
git fetch <remote> --prune
git diff <branch> <remote>/<branch>
git merge --ff-only <remote>/<branch>
```

If the last command fails:

```
git rebase --preserve-merges <remote>/<branch>
```

## Push a new branch

```
git push -u <remote> <branch>
```

## Push amended commits

```
git push -f
```

**Never do this on a public or shared branch!**

## Rename a branch

```
git branch -m old-name new-name
```

Then delete the old remote branch and push the new branch.

## Show tags

```
git tag
```

## Split a commit

Just [delete it keeping the changes](#keeping-the-changes):

```
git reset HEAD^
```

Then create as many new commits as you like. Thats all if you want
to split the latest commit of the branch. I you want to split an
earlier commit first you need to copy the id (or the first 7
characters) of the oldest commit you want to split and then run:

```
git rebase --interactive <id>^
```

This will launch your editor. Change the word _pick_ to _edit_ in
front of the commits you want to split and then save, quit and
split it as explained above. Once you have created all your new
commits run:

```
git rebase --continue
```

## Stash staged changes

```
git stash --keep-index
```

## Switch back to the latest commit (the `HEAD`)

```
git checkout <branch>
```

If this fails stash the changes, discard them or commit in a new
branch.

## Switch to an specific commit

```
git checkout <commit>
```

## View all commits of a specific branch

On the parent branch:

```
git log ..<branch-whose-commits-i-want-to-see>
```
[do-not-pull]: https://adamcod.es/2014/12/10/git-pull-correct-workflow.html
