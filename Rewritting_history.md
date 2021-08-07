# git commit --amend/ git rebase/ git reflog

## 1. git commit --amend

- The git commit --amend command is a convenient way to **modify the most recent commit.**
- It lets you combine staged changes with the previous commit instead of creating an entirely new commit.
- It can also be used to **simply edit the previous commit message** without changing its snapshot.
- But, amending does not just alter the most recent commit, it replaces it entirely, meaning the amended commit will be a new entity with its own ref.
- To Git, it will look like a brand new commit,
- Let's say you just committed and you made a **mistake in your commit log message.**

```
	$ git commit --amend
```

- Running this command w**hen there is nothing staged** lets you edit the previous commit’s message without altering its snapshot.
- Adding the -**m option** allows you to pass in a new message from the command line without being prompted to open an editor.

```
	$ git commit --amend -m "an updated commit message"
```

##### Changing committed files

- add files to staging area
- The -**-no-edit flag** will allow you to make the amendment to your commit without changing its commit message.

```
	$ git commit --amend --no-edit

```

- **Don’t amend public commits**
  - Amended commits are actually entirely new commits and the previous commit will no longer be on your current branch.
  - This has the same consequences as resetting a public snapshot.
  - Avoid amending a commit that other developers have based their work on. This is a confusing situation for developers to be in and it’s complicated to recover from.

## 2. git rebase

- Rebase is one of two Git utilities that **specializes in integrating changes from one branch onto another.**
- The primary reason for rebasing is to maintain a **linear project history.**
- Rebasing is a common way to integrate upstream changes into your local repository.
- The other change integration utility is **git merge.**
- Merge is always a forward moving change record.
- Pulling in upstream changes with **Git merge** results in a **superfluous merge commit** every time you want to see how the project has progressed.
- On the other hand, rebasing is like saying, “I want to base my changes on what everybody has already done.”
- **Alternatively, rebase has powerful history rewriting features.**
- **Don't rebase public history**
- Rebase itself has 2 main modes: **"manual" and "interactive" mode**
- **Git rebase in standard mode** will automatically take the commits in your current working branch and apply them to the head of the passed branch.

```
	 $ git rebase <base>
```

- This automatically rebases the current branch onto , which can be any kind of commit reference (for example an ID, a branch name, a tag, or a relative reference to HEAD).
- Running git rebase with the **-i flag** begins an interactive rebasing session.
- Instead of blindly moving all of the commits to the new base, interactive rebasing gives you the opportunity to alter individual commits in the process.
- This lets you clean up history by removing, splitting, and altering an existing series of commits. **It's like Git commit --amend on steroids.**

```
	$ git rebase -interactive <base>
```

##### OR

```
	$ git rebase -i <base>
```

- Rebasing is the process of moving or combining a sequence of commits to a new base commit.
- From a content perspective, rebasing is changing the base of your branch from one commit to another making it appear as if you'd created your branch from a different commit.
- . Internally, Git accomplishes this by creating new commits and applying them to the specified base.
- **It's very important to understand that even though the branch looks the same, it's composed of entirely new commits.**

```
	git rebase -i HEAD~4
```

- In our text editor, we'll see a list of commits alongside a list of commands that we can choose from. Here are a couple of the more commonly used commands:
  - **pick** - use the commit
  - **reword** - use the commit, but edit the commit message
  - **edit** - use commit, but stop for amending
  - **fixup** - use commit contents but meld it into previous commit and discard the commit message.
  - **drop** - remove commit

---

## 3. git reflog

- Git keeps track of **updates to the tip of branches** using a mechanism called **reference logs, or "reflogs."**
- Many Git commands **accept a parameter** for specifying a reference or "ref", which is a pointer to a commit.
- Common examples include:
  - git checkout
  - git reset
  - git merge
- Reflogs track when Git refs were updated in the local repository.
- In addition to branch tip reflogs, a special reflog is maintained for the Git stash.
- Reflogs are stored in directories under the **local repository's** .git directory.
- **git reflog directories** can be found at **.git/logs/refs/heads**/., **.git/logs/HEAD**, and also **.git/logs/refs/stash** if the git stash has been used on the repo.
- Reflog allows you to go back to commits even though they are not referenced by any branch or tag.
- After rewriting history, the reflog contains information about the old state of branches and allows you to go back to that state if necessary.
- Every time your branch tip is updated for any reason (by switching branches, pulling in new changes, rewriting history or simply by adding new commits), a new entry will be added to the reflog.
- **Reflogs also expire.** Git cleans out old entries **after around 90 days**, though this can be configured.

```
	$ git reflog
```

-     This displays the reflog for the local repository.

```
 $ git reflog --relative-date
```

- This shows the reflog with relative date information (e.g. 2 weeks ago).
- The git reflog command accepts subcommands **show,** **expire**, **delete**, and **exists**. **Show is the only commonly used variant, and it is the default subcommand.**
- git reflog show will show the log of a specific reference **(it defaults to HEAD**)
- The l**atest activity** is represented at the top labeled **HEAD@{0}.**
- **Show** is implicitly passed by **default.**

```
	$ git reflog show HEAD
```

- In addition to HEAD refs, other branches, tags, remotes, and the Git stash can be referenced as well.

```
	$ git reflog show main
```

- You can get a **complete reflog of all refs** by executing:

```
	$ git reflog show --all
```

- **Reflog References**

  - We can access specific git refs is **name@{qualifier}.**
  - We can use this syntax to access specific ref pointers and can pass them to other commands including checkout, reset, and merge

- **Timed References**
  - Every entry in the reference logs has a timestamp associated with it.
  - We can filter reflogs entries by time/date by using time qualifiers like:
    - 1.day.ago
    - 3.minutes.ago
    - yesterday
    - Fri, 12 Feb 2021 14:06:21 -0800

```
	$ git reflog master@{one.week.ago}
```

```
	$ git checkout bugfix@{2.days.ago}
```

```
	$ git diff main@{0} main@{yesterday}
```

```
	$ git diff main@{0} main@{1.day.ago}
```

##### Reflogs Rescue

- We can sometimes use reflog entries to access commits that seem lost and are not appearing in git log
- If it turns out that you accidentally moved back, the reflog will contain the commit main pointed to **(0254ea7)** before you accidentally dropped 2 commits.

```
	$ git reset --hard 0254ea7
```

- **Using Git reset,** it is now possible to change main back to the commit it was before.
- This provides a safety net in case the history was accidentally changed.**Using Git reset,**
- It's important to note that the reflog only provides a safety net if changes have been committed to your local repository and that it only tracks movements of the repositories branch tip.
- Additionally reflog entries have an expiration date. The default expiration time for reflog entries is 90 days.
