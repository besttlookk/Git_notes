# git checkout/ git clean /git revert/ git reset / git rm / git restore

- It is first important to note that Git does not have a traditional 'undo' system like those found in a word processing application.
- Git has its own nomenclature for 'undo' operations that it is best to leverage in a discussion. This nomenclature includes terms like reset, revert, checkout, clean, and more.

## 1. git checkout

- The git checkout command is like a Git Swiss Army knife. Many developers think it is overloaded, which is what lead to he addition of the **git switch** and **git restore** commands
- We can use checkout to **create branches**, **switch to new branches**, **restore files**, and **undo history!**

##### Finding what is lost: Reviewing old commits

- The whole idea behind any version control system is to store “safe” copies of a project so that you never have to worry about irreparably breaking your code base.
- Once you’ve built up a project history of commits, you can review and revisit any commit in the history.
- **Each commit has a unique SHA-1 identifying hash.** These IDs are used to travel through the committed timeline and revisit commits.
- By default, git log will only show commits for the currently selected branch.
- It is entirely possible that the commit you're looking for is on another branch.
- You can view all commits across all branches by executing **git log --branches=\***

```
	$ git log --branches=*
```

- The command **git branch** is used to view and visit other branches.
- Invoking the command, **git branch -a** will return a list of all known branch names. One of these branch names can then be logged using git log.

```
	$ git checkout d8382d7
```

- During the normal course of development, the **HEAD usually points to main or some other local branch,**
- but when you check out a previous commit, **HEAD no longer points to a branch—it points directly to a commit.**
- This is called a **“detached HEAD” state,**
- in **'detached HEAD' state**. You can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by switching back to a branch.
- When we checkout a particular commit, **HEAD points at that commit rather than at the branch pointer.**
- While in detached HEAD, if we want to work from here,make a new branch and switch to it.
- Head is now back to pointing at a branch reference!
- Now on the new branch, make as many new commits as you want!
- It's like you time traveled! We went back to an old commit and made a new branch based on it
- If you checkout an old commit and decide you want to **return to wh**ere you were before....
- Simply switch back to whatever branch you were on before (main in this example)

```
	$ git checkout main
```

###### OR

```
	$ git switch main
```

- **git checkout** supports a slightly odd syntax for **referencing previous commits relative to a particular commit**

```
	$ git checkout HEAD~1
```

- **HEAD~1** refers to the **commit before HEAD (parent)**, HEAD~2 refers to 2 commits before HEAD (grandparent)

###### Discarding changes in file

- Suppose you have made some changes to a file but dont want to keep them.
- To revert the file back to whatever it looked like **when you last commited** ..
- **git checkout HEAD <filename>** to discard any changes in that file, reverting back to the HEAD.

```
	$ git checkout HEAD <file>
```

###### OR

```
	$ git checkout -- <file>
```

---

## 2. Restore

- **git restore** is a brand new Git command that helps with undoing operations.
- Suppose you've made some changes to a file since your last commit. You've saved the file but then realize you definitely **do NOT want those changes an**ymore!

```
	$ git restore <file-name>
```

<div style="color: red">
NOTE: The above command is not "undoable" ,If you have uncommited changes in the file, they will be lost!
</div>

- **git restore <file-name>** restores **using HEAD as the default source**, but we can change that using the -**-source option**.

```
	$ git restore --source HEAD~1 app.js
```

- For example, git restore --source HEAD~1 home.html will restore the contents of home.html to its state from the commit prior to HEAD. You can also use a particular commit hash as the source.
- **Unstaging Files with Restore**
  - If you have accidentally added a file to your staging area with git add and you **don't wish to include it in the next commit, you can use git restore o remove it from staging.**

```
	$ git restore --staged <file-name>
```

## 3. git reset

- Suppose you've just made a couple of commits on the master branch, **but you actually meant to make them on a separate branch instead**. To undo those commits, you can use git reset.
- **git reset <commit-hash>** will reset the repo back to a specific commit. **The commits are gone**

```
	$ git reset <commit-hash>
```

<div style="color: red">
* The file content will move to working directory
</div>

- If you want to **undo both the commits AND the actual changes in your files**, you can use the **--hard option**.

```
	$ git reset --hard <commit>
```

- for example, **git reset --hard HEAD~1** will delete the last commit and associated changes.
<div style="color: red">
- The changes in the file(s) are gone too!
</div>

---

## 4. git revert

- **git revert** is similar to **git reset** in that they both "**undo" changes**, but they accomplish it in different ways.
- git revert instead creates a brand new commit which reverses/undos the changes from a commit. Because it results in a new commit, you will be **prompted to enter a commit message.**

```
	$ git revert <commit-hash>
```

##### OR

```
	$ git revert -e <commit-hash>
```

#### OR

```
	$ git revert --edit <commit-hash>
```

- **--no-edit** is the inverse of the -e option. **The revert will not open the editor.**

```
	$ git revert --no-edit <commit-hash>
```

- **NO new commit**

```
	$ git revert -n <commit-hash>
```

##### OR

```
	$git revert --no-commit <commit-hash>
```

- Passing this option will prevent git revert from creating a new commit that inverses the target commit.
- Instead of creating the new commit this option will add the inverse changes to the Staging Index and Working Directory.
- **Which One Should I Use?**
  - If you want to reverse some commits that other people already have on their machines, you should use revert.
  - If you want to reverse commits that you haven't shared with others, use reset and no one will ever know

---

## 5. git clean

- Git clean can be considered complementary to other commands like git reset and git checkout.
- Whereas these other commands operate on files previously added to the Git tracking index, **the git clean command operates on untracked files.**
- **Untracked files** are files that have been created within your repo's working directory but have not yet been added to the repository's tracking index using the git add command.
- By default, Git is globally configured to require that git clean be passed a **"force" option to initiate.**
- This is an important safety mechanism. **When finally executed git clean is not undo-able.**
- When fully executed, **git clean will make a hard filesystem deletion,** similar to executing the command line rm utility.
- Make sure you really want to delete the untracked files before you run it.
- The **-n option** will perform a “dry run” of git clean. This wil**l show you which files are going to be removed without actually removing them.**
- It is a best practice to always first perform a dry run of git clean.

```
	$ git clean -n
```

- **By default git clean will not operate recursively on directories.**
- This is another safety mechanism to prevent accidental permanent deletion.
- The force option initiates the actual deletion of untracked files from the current directory.

```
	$ git clean -f
```

##### OR

```
	$ git clean --force
```

- Force is required unless the clean.requireForce configuration option is set to false.
- This will not remove untracked folders or files specified by .gitignore.
- By default **git clean -f** will operate on all the **current directory untracked files.**
- Additionally, a **< path >** value can be passed with the -f option that will remove a specific file.

```
	$ git clean -f <path>
```

- The -**d option** tells git clean that you also **want to remove any untracked directories,** by default it will ignore directories.

```
	$ git clean -nd
	$ git clean -df
```

- A common software release pattern is to have a build or distribution directory that is not committed to the repositories tracking index.
- The build directory will contain ephemeral build artifacts that are generated from the committed source code.
- This build directory is usually added to the repositories **.gitignore file.**
- It can be convenient to also **clean this directory with other untracked files.**
- The -**x option** tells git clean to also include any ignored files.
- As with previous git clean invocations, it is a best practice to execute **a 'dry run' first,** before the final deletion.
- The **-x option will act on all ignored files**, not just project build specific ones. This could be unintended things like ./.idea IDE configuration files.

```
	$ git clean -xf
```

- git clean has an **"interactive" mode** that you can initiate by passing the **-i option.**

```
	$ git clean -di
```

---

## 6. git rm

- A common question when getting started with Git is **"How do I tell Git not to track a file (or files) any more?**"
- T**he git rm command is used to remove files from a Git repository.**
- It can be thought of as the **inverse of the git add command.**
- The **primary function of git rm** is to remove tracked files from the Git index.
- **Additionally,** git rm can be used to **remove files from both the staging index and the working directory.**
- There is no option to remove a file from only the working directory. The files being operated on must be identical to the files in the current HEAD.
- If there is a discrepancy between the HEAD version of a file and the staging index or working tree version, Git will block the removal.
- This block is a safety mechanism to prevent removal of in-progress changes.
- **Note that git rm does not remove branches.**

```
	$ git rm <file>...
```

- Specifies the target files to remove. The option value can be an **individual file,** a **space delimited list of files file1 file2 file3, or a wildcard file glob (~./directory/\*).**
- The **-f option** is used to **override the safety check** that Git makes to ensure that the files in HEAD match the current content in the staging index and working directory.

```
	$ git rm -f <file>...
```

###### OR

```
	$ git rm --force <file>...
```

- The **"dry run" option** is a safeguard that will execute the git rm command but not actually delete the files. **Instead it will output which files it would have removed.**

```
	$ git rm -n <file>...
```

###### OR

```
	$ git rm --dry-run <file>...
```

- The **-r option is shorthand for 'recursive'.** When operating in recursive mode git rm will remove a target directory and all the contents of that directory.

```
	$ git rm -r <file>
```

- The **cached option** specifies that the removal should happen only on the staging index. Working directory files will be left alone.

```
	$ git rm --cached
```

- **--ignore-unmatch**
  - This causes the **command to exit with a 0 sigterm status even if no files matched.**
  - This is a Unix level status code. The code 0 indicates a successful invocation of the command.
  - The --ignore-unmatch option can be helpful when using git rm as part of a greater shell script that needs to fail gracefully.
- The **quiet option hides** the output of the git rm command. The command normally outputs one line for each file removed.

```
	$ git rm -q <file>..
```

###### OR

```
	$ git rm --quiet <file>
```

##### How to undo git rm

- Executing **git rm is not a permanent update.**
- The **command will update the staging index and the working directory**. These changes will not be persisted until a new commit is created and the changes are added to the commit history.
- This means that the changes here can be "undone" using common Git commands.
- A **reset will revert** the current staging index and working directory back to the HEAD commit. This will **undo a git rm**

```
	$ git reset HEAD

```

- A checkout will have the same effect and restore the latest version of a file from HEAD.

```
	$ git checkout .
```

- I**n the event that git rm was executed and a new commit was created which persist the removal, git reflog can be used to find a ref that is before the git rm execution**.
- T**he git rm command operates on the current branch only**. The removal event is only applied to the working directory and staging index trees. The file removal is not persisted to the repository history until a new commit is created.
