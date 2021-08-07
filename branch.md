- List all of the branches in your repository. **This is synonymous with git branch --list.**

```
	$ git branch
```

- Create a new branch called ＜ branch ＞. **This does not check out the new branch.**

```
	$ git branch <branch-name>
```

- **Delete** the specified branch

```
	$ git branch -d <branch-name>
```

- **This is a “safe” operation** in that Git prevents you from deleting the branch if it has unmerged changes.
- **Force delete** the specified branch, even if it has unmerged changes.

```
	$ git branch -D <branch-name>
```

- This is the command to use if you want to permanently throw away all of the commits associated with a particular line of development.
- **Rename the current branch to ＜ new-branch-name ＞.**

```
	$ git branch -m <new-branch-name
```

- **List all remote branches.**

```
	$ git branch -a
```

- **View more info about branch**

```
	$ git branch -v
```

##### Branch switch

- Once you have created a new branch, use **git switch [branch-name]** to switch to it

```
	$ git switch <branch-name>
```

- **Another way of switching??**
  - Historically, we used git checkout <branch-name> to switch branches. This still works.

```
	$ git checkout <branch-name>
```

- **Creating and switching at the same time**

```
	$ git switch -c <branch-name>
```

##### OR

```
	$ git switch --create <branch-name>
```

## Checkout

- The git checkout command lets you navigate between the branches created by git branch.

```
	$ git checkout <branch>
```

- **Create branch & switch**

```
	$ git checkout -b <new-branch>
```

- **By default git checkout -b will base the new-branch off the current HEAD.**
- An optional additional branch parameter can be passed to git checkout.

```
	$ git checkout -b ＜new-branch＞ ＜existing-branch＞
```

- ＜ existing-branch ＞ is passed which then bases new-branch off of existing-branch instead of the current HEAD.

## Merge

- Merging is Git's way of putting a forked history back together again.
- The git merge command lets you take the independent lines of development created by git branch and integrate them into a single branch.
- The current branch will be updated to reflect the merge, but the target branch will be completely unaffected
