# git add / git commit / git diff / git stash / .gitignore

- The traditional software expression of "**saving**" is synonymous with the Git term **"committing"**.
- A commit is the Git equivalent of a "save".
- Git has an additional saving mechanism called '**the stash'.** The stash is an ephemeral storage area for changes that are **not ready to be committed.**
- The **stash** operates on the working directory, the first of the three trees and has extensive usage options.
- A Git repository can be configured to ignore specific files or directories.
- This will prevent Git from saving changes to any ignored content. Git has multiple methods of configuration that manage the ignore list.

---

## 1. git add

- The git add command adds a change in the working directory to the **staging area.**

- It tells Git that you want to include updates to a particular file in the next commit.
- However, git add doesn't really affect the repository in any significant way—changes are not actually recorded until you run git commit.
- In conjunction with these commands, you'll also need **git status** to view the state of the working directory and the staging area.
- Developing a project revolves around the basic **edit/stage/commit** pattern.
- First, you edit your files in the working directory. When you’re ready to save a copy of the current state of the project, you stage changes with **git add.**
- After you’re happy with the staged snapshot, you commit it to the project history with **git commit.**
- The **git reset** command is used to undo a commit or staged snapshot.
- In addition to **git add** and **git commit**, a third command **git push** is essential for a complete collaborative Git workflow.
- **git push** is utilized to send the committed changes to remote repositories for collaboration.

###### Stagging Area:

- The staging area is considered one of the **"three trees"** of Git, along with, the **working directory,** and the **commit history.**
- Instead of committing all of the changes you've made since the last commit, the stage lets you **group related changes** into highly focused snapshots before actually committing it to the project history.
- As in any revision control system, it’s important to create **atomic commits** so that it’s easy to track down bugs and revert changes with minimal impact on the rest of the project.

##### Common options:

- **Stage all changes:**

```
	$ git add .
```

- **Stage all changes in <file> for the next commit.:**

```
	$ git add <file>
```

- **Stage all changes in <directory> for the next commit.:**

```
	$ git add <Directory>
```

- **Begin an interactive staging session that lets you choose portions of a file to add to the next commit.**
  - This will present you with a chunk of changes and prompt you for a command.
  - Use **y** to stage the chunk,
  - **n** to ignore the chunk,
  - **s** to split it into smaller chunks,
  - **e** to manually edit the chunk,
  - and **q** to exit.

---

## 2. git commit

- The **git commit** command **captures** a snapshot of the project's currently staged changes.
- Committed snapshots can be thought of as **“safe” versions of a project**—**Git will never change them** unless you explicitly ask it to.
- Commits can be thought of as snapshots or milestones along the timeline of a Git project.
- Commits are created with the git commit command to capture the state of a project at that point in time.
- Git doesn’t force you to interact with the **central repository** until you’re ready.
- Just as the staging area is a buffer between the working directory and the project history, each **developer’s local repository** is a buffer between **their contributions** and the **central repository.**

##### Common option:

- **Commit the staged snapshot.**
  - This will launch **a text editor** prompting you for a commit message.
  - After you’ve entered a message, save the file and close the editor to create the actual commit.

```
	$ git commit

```

- **Commit a snapshot of all changes in the working directory.**
  - This only includes modifications to tracked files (those that have been added with git add at some point in their history).

```
	$ git commit -a
```

- **A shortcut command that immediately creates a commit with a passed commit message.**
  - By default, git commit will open up the locally configured text editor, and prompt for a commit message to be entered. Passing the -m option will forgo the text editor prompt in-favor of an inline message.

```
	$ git commit -m "<commit message>"
```

- **A power user shortcut command that combines the -a and -m options.**
  - This combination immediately creates a commit of all the staged changes and takes an inline commit message.

```
	$ git commit -am "<commit message>"
```

- **Amend last commit:**
  - This option adds another level of functionality to the commit command.
  - Passing this option will modify the last commit. Instead of creating a new commit, staged changes will be added to the previous commit.
  - This command wil**l open up the system's configured text editor a**nd prompt to change the previously specified commit message.
  - In case you want to **add files to old commit** .First add those files on the Staging area.

```
	git commit --amend
```

```
	$ git commit -m 'some commit'
	$ git add forgotten_file
	$ git commit --amend
```

###### NOTE:

- Make commits **atomic** (group similar changes together, don't
  commit a million things at once)
- Write **meaningful** but concise commit messages

---

## 3. git diff

- We can use the git diff command to view **changes** between **commits**, **branches**, **files,** our **working directory**, and more!
- We often use **git diff** alongside commands like **git status** and **git log**, to get a better picture of a repository and how it has changed over time.
- **Compares Staging Area and Working Directory[tracked files]**
  - **Without additional options**, **git diff** lists all the changes in our **working directory** that are **NOT staged** for the next commit.
  - git diff wil not show **untracked files.**
  - When we provide **file name** it shows only difference in that file.

```
	$ git diff
```

```
	$ git diff index.js
```

- **Lists all changes in the working tree since your last commit.(git diff HEAD)**
  - **git diff HEAD** again wil not show anything from **untracked files**
  - We'll often come across the term **HEAD** in Git.
  - HEAD is simply a pointer that refers to the current "location" in your repository. It points to a particular branch reference.
  - HEAD always points to the latest commit you made on the master branch, but soon we'll see that we can move around and HEAD will change!
    - .**git/refs/head** diretory stores list of branch file which in turn store the commit that they all are pointing

```bash
	$ cat .git/HEAD
```

<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
			ref: refs/heads/main
</div>

```bash
	$ git switch  <branch_2>
	$ cat .git/HEAD
```

<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
			ref: refs/heads/brach_2
</div>

```bash
	$ cat .git/refs/head/main
```

<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
			59f572c4d9a13ac4d2edcaf713610cd07ef2b17e
</div>

```
	$ git diff HEAD
```

```
	$ git diff HEAD index.js
```

- **Changes between the staging area and our last commit.**
  - "Show me what will be included in my commit if I run git commit right now".
  - We can view the changes within a specific file by providing **git diff with a filename.**

```
	$ git diff --staged
```

```
	$ git diff --staged index.js
```

###### OR

```
	$ git diff --cached
```

```
	$ git diff --cached index.js
```

- **Comparing Branches**

```
	$ git diff branch1 ..branch2
```

- **Comparing Commits**
  - To compare two commits, provide git diff with the **commit hashes** of the commits in question.

```
	$ git diff commit1 ..commit2
```

###### Git Diff Summary:

- **git diff** = Staging Area - tracking files.
- **git diff --staged** = commit - staging area.
- **git diff HEAD** = [git diff] + [git diff --staged] = commit - tracking files

---

- **Detailed breakdown of the diff output.**

1. **Comparison input:** \* This line displays the input sources of the diff.
<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
		diff --git a/diff_test.txt b/diff_test.txt
</div>

2. **Meta data**
   <div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
   	index 6b0c6cf..b37e70a 100644
   </div>

   - This line displays some internal Git metadata.
   - You will most likely not need this information.
   - The numbers in this output correspond to Git object version hash identifiers.

3. **Markers for changes**
   <div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
   	---  a/diff_test.txt<br />
   	+++ b/diff_test.txt
   </div>

   - These lines are a **legend** that assigns symbols to each diff input source.
   - In this case, changes from **a/diff_test.txt** are marked with **a ---** symbol
   - and the changes from **b/diff_test.txt** are marked with the **+++ symbol**.

4. **Diff chunks**
   <div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
   	@@ -34,6 +34,8 @@ <br>
   	<span style="color:red">-this is a git diff test example</span><br>
   	<span style="color:green">+this is a diff examplee</span>
   </div>

   - The remaining diff output is a list of diff 'chunks'.
   - A diff only displays the sections of the file that have changes.
   - The **first line** is the **chunk header.** Each chunk is prepended by a header inclosed within **@@ symbols.**
   - The content of the header is a **summary of changes** made to the file.
   - **In this header example, 6 lines have been extracted starting from line number 34. Additionally, 8 lines have been added starting at line number 34.**

---

## 4. git stash

- Imagine:

  - I'm on master brach. I make a new branch and switch to it.
  - I do some new work, but don't make any commits.
  - **What happens when I switch back to master**?
    - My changes come with me to the destination branch, or
    - **Git won't let me switch if it detects potential conflicts**

- Git provides an easy way of **stashing these uncommitted changes** so that we can return to them later, without having to make unnecessary commits.
- Running git stash will take all uncommitted changes **(staged and unstaged but not untracked files**) and stash them, reverting the changes in your working copy.
- USe **git stash pop** to **remove the most recently stashed changes** in your stash and **re-apply them to your working copy.**

```
	$ git stash
```

###### OR

```
	$ git stash save
```

- **Note that the stash is local to your Git repository; stashes are not transferred to the server when you push.**
- **By default Git won't stash changes made to untracked or ignored files.**
- Adding the **-u option** (or --include-untracked) tells git stash to also **stash your untracked files:**

```
	$ git stash -u
```

- You can include changes to **ignored files as well by passing** the -a option (or --all) when running git stash.

```
	$ git stash -a
```

```
	$ git stash --all
```

- You can **reapply previously** stashed changes with git stash pop:
- **Popping** your stash **removes** the changes from your stash and **reapplies** them to your working copy.

```
	$ git stash pop
```

- **Alternatively,** you can **reapply** the changes to your working copy and k**eep** them in your stash with git stash apply:
- This is useful if you want to **apply** the **same stashed change**s to **multiple branches.**

```
	$ git stash apply
```

##### Managing multiple stashes

- You aren't limited to a single stash. You can run git stash several times to create multiple stashes, and then use **git stash list** to view them.
- By default, stashes are identified simply as a **"WIP" – work in progress –** on **top of the branc**h and commit that you created the stash from.

```
	$ git stash list
```

<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
		stash@{0}: WIP on main: 5002d47 our new homepage<br>
		stash@{1}: WIP on main: 5002d47 our new homepage<br>
		stash@{2}: WIP on main: 5002d47 our new homepage
</div>

- To provide a bit more context, it's good practice to annotate your stashes with a description, using

```
	$ git stash save "add style to our site"
	$ git stash list
```

<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
		stash@{0}: On main: add style to our site<br>
		stash@{1}: WIP on main: 5002d47 our new homepage<br>
		stash@{2}: WIP on main: 5002d47 our new homepage
</div>

- **By default**, **git stash pop** will re-apply the **most recently** created stash: **stash@{0}**
- You can choose which stash to re-apply by passing its identifier as the last argument,

```
	$ git stash pop stash@{2}
```

##### Viewing stash diffs

- You can view a summary of a stash with git stash show:

```
	$ git stash show
```

- Or pass the -p option (or --patch) to view the full diff of a stash:

```
	$ git stash show -p
```

##### Partial stashes

- You can also choose to stash just a **single file**, **a collection of files**, or **individual changes from within files.**
- If you pass the **-p option (or --patch) to git stas**h, it will iterate through each changed "hunk" in your working copy and ask whether you wish to stash it:
- You can hit ? for a full list of hunk commands.
- There is no explicit "abort" command, but hitting CTRL-C(SIGINT) will abort the stash process.

###### Creating a branch from your stash

- If the changes on your **branch diverge from the changes in your stash**, you may run into conflicts when popping or applying your stash.
- Instead, you can use **git stash branch** to create a new branch to apply your stashed changes to:

```
	$ git stash branch
```

- This checks out a new branch based on the commit that you created your stash from, and then pops your stashed changes onto it.

###### Cleaning up your stash

- If you decide you no longer need a particular stash, you can delete it with git stash drop:

```
	$ git stash drop stash@{1}
```

- Or you can delete all of your stashes with:

```
	$ git stash clear
```

###### How git stash works

- Stashes are actually encoded in your repository as commit objects.
- The special ref at **.git/refs/stash** points to your most **recently created stash,** and **previously created stashes are referenced by the stash ref's reflog.**

```
	$ cat .git/refs/stash
```

<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
	3f07bd8625c7ee13e05dfc4b2cf4f1444cd73ca4
</div>

- This is why you refer to stashes by **stash@{n}:** you're actually referring to the **nth reflog** entry for the stash ref.
- Since a stash is just a commit, you can inspect it with git log:

```
	$ git log --oneline --graph stash@{0}
```

---

## 4. .gitignore

- Git sees every file in your working copy as **one of three things**:

  1. **tracked** - a file which has been previously staged or committed;
  2. **untracked** - a file which has not been staged or committed; or
  3. **ignored** - a file which Git has been explicitly told to ignore.

- Ignored files are usually build artifacts and machine generated files that can be derived from your repository source or should otherwise not be committed. Some common examples are:

  - dependency caches, such as the contents of **/node_modules** or **/packages**
  - compiled code, such as .**o, .pyc, and .class** files.
  - build output directories, such as **/bin,** **/ou**t, or **/target**
  - files generated at runtime, such as **.log**, **.lock, or .tmp**
  - hidden system files, such as **.DS_Store or Thumbs.db**
  - personal IDE config files, such as **.idea/workspace.xml**

- Ignored files are tracked in a special file named .gitignore that is checked in at the root of your repository.

##### Git ignore patterns

    * **.gitignore** uses **globbing patterns** to match against file names.

| Pattern                         | Example matches                                                                   | Explanation\*                                                                                                                                                                                        |
| ------------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \*\*/logs                       | logs/debug.log logs/monday/foo.bar build/logs/debug.log                           | You can prepend a pattern with a double asterisk to match directories anywhere in the repository.                                                                                                    |
| \*\*/logs/debug.log             | logs/debug.log build/logs/debug.log but not logs/build/debug.log                  | You can also use a double asterisk to match files based on their name and the name of their parent directory.                                                                                        |
| \*.log                          | debug.log foo.log .log logs/debug.log                                             | An asterisk is a wildcard that matches zero or more characters.                                                                                                                                      |
| \*.log !important.log           | debug.log trace.log but not important.log logs/important.log                      | Prepending an exclamation mark to a pattern negates it. If a file matches a pattern, but also matches a negating pattern defined later in the file, it will not be ignored.                          |
| _.log !important/_.log trace.\* | debug.log important/trace.log but not important/debug.log                         | Patterns defined after a negating pattern will re-ignore any previously negated files.                                                                                                               |
| /debug.log                      | debug.log but not logs/debug.log                                                  | Prepending a slash matches files only in the repository root.                                                                                                                                        |
| debug.log                       | debug.log logs/debug.log                                                          | By default, patterns match files in any directory                                                                                                                                                    |
| debug?.log                      | debug0.log debugg.log but not debug10.log                                         | A question mark matches exactly one character.                                                                                                                                                       |
| debug[0-9].log                  | debug0.log debug1.log but not debug10.log                                         | Square brackets can also be used to match a single character from a specified range                                                                                                                  |
| debug[01].log                   | debug0.log debug1.log but not debug2.log debug01.log                              | Square brackets match a single character form the specified set.                                                                                                                                     |
| debug[!01].log                  | debug2.log but not debug0.log debug1.log debug01.log                              | An exclamation mark can be used to match any character except one from the specified set.                                                                                                            |
| logs                            | logs logs/debug.log logs/latest/foo.bar build/logs build/logs/debug.log           | If you don't append a slash, the pattern will match both files and the contents of directories with that name. In the example matches on the left, both directories and files named logs are ignored |
| logs/                           | logs/debug.log logs/latest/foo.bar build/logs/foo.bar build/logs/latest/debug.log | Appending a slash indicates the pattern is a directory. The entire contents of any directory in the repository matching that name – including all of its files and subdirectories – will be ignored  |
| logs/\*\*/debug.log             | logs/debug.log logs/monday/debug.log logs/monday/pm/debug.log                     | A double asterisk matches zero or more directories.                                                                                                                                                  |
| logs/\*day/debug.log            | logs/monday/debug.log logs/tuesday/debug.log but not logs/latest/debug.log        | Wildcards can be used in directory names as well.                                                                                                                                                    |
| logs/debug.log                  | logs/debug.log but not debug.log build/logs/debug.log                             | Patterns specifying a file in a particular directory are relative to the repository root. (You can prepend a slash if you like, but it doesn't do anything special.)                                 |
