# git status/ git tag/ git blame/ git log

## 1.git status

- The git status command displays the **state** of the **working directory** and **the staging area**
- It lets you see which changes have been staged, which haven’t, and which files aren’t being tracked by Git.
- Status output **does not** show you any information regarding the **committed project history**. For this, you need to use git log.

```
	$ git status

```

## 2. git log

- The git log command displays committed snapshots.
- It lets you **list the project history, filter it, and search for specific changes.**
- While git status lets you inspect the working directory and the staging area, **git log only operates on the committed history.**
- Log output can be customized in several ways, from simply filtering commits to displaying them in a **completely user-defined format.**
- .Some of the most common configurations of git log are presented below.

- **Display the entire commit history using the default formatting.**
  - If the output takes up more than one screen, you can use Space to scroll and q to exit.

```
	$ git log

```

- **Limit the number of commits by**
  - For example, **git log -n 3** will display only 3 commits.

```
	$ git log -n <limit>
```

- **Condense each commit to a single line.**
  - This is useful for getting a high-level overview of the project history.

```
	$ git log --oneline
```

- **include which files were altered and the relative number of lines**
  - Along with the ordinary git log information, include which files were altered and the relative number of lines that were added or deleted from each of them.

```
	$ git log --stat
```

- **Display the patch representing each commit.**
  - This shows the full diff of each commit, which is the most detailed view you can have of your project history.

```
	$ git log -p
```

- **Search for commits by a particular author.**
  - The argument can be a plain string or a regular expression.

```
	$ git log --author="<pattern>"
```

- **Search for commits with a commit message that matches , which can be a plain string or a regular expression.**

```
	$ git log --grep="<pattern>"
```

- **Show only commits that occur between < since > and < until >.**
  - Both arguments can be either a **commit ID**, a **branch name**, **HEAD**, or any other kind of revision reference.
- **Only display commits that include the specified file.**
  - This is an easy way to see the history of a particular file.

```
	$ git log <file>
```

- **A few useful options to consider.**
  - The **--graph** flag that will draw a text based graph of the commits on the left hand side of the commit messages.
  - **--decorate** adds the names of branches or tags of the commits that are shown.
  - **--oneline** shows the commit information on a single line making it easier to browse through commits at-a-glance.

---

## 3. git tag

- Tags are ref's that point to specific points in Git history.
- Tagging is generally used to capture a point in history that is used for a marked version release (i.e. v1.0.1).
- A tag is like a branch that doesn’t change. Unlike branches, tags, after being created, have no further history of commits.

###### Creating a tag

```
	$ git tag <tagname>
```

- Replace < tagname > with a semantic identifier to the state of the repo at the time the tag is being created.
- Git supports **two different types** of tags, **annotated** and **lightweight tags.**
- The previous example created a lightweight tag.
- Lightweight tags and Annotated tags differ in the amount of accompanying **meta data** they store.
- **A best practice is to consider Annotated tags as public, and Lightweight tags as private.**
- **Annotated tags** store extra meta data such as: the t**agger name**, **email**, and **date**. This is important data for a public release.
- Lightweight tags are essentially 'bookmarks' to a commit, they are just a name and a pointer to a commit, useful for creating quick links to relevant commits.

###### Annotated Tags

- Annotated tags are stored as full objects in the Git database.
- . Similar to commits and commit messages **Annotated tags have a tagging message.**
- Additionally, for security, annotated tags can be signed and verified with GNU Privacy Guard (GPG).
- Suggested best practices for git tagging is to prefer annotated tags over lightweight so you can have all the associated meta-data.

```
	$ git tag -a v1.4
```

- The command will then o**pen up the configured default text editor t**o prompt for further meta data input.
- There is a convenience method similar to git **commit -m** that will immediately create a new tag and forgo opening the local text editor in favor of saving the message passed in with the -m option.

```
	$ git tag -a v1.4 -m "my version 1.4"
```

###### Lightweight Tags

```
	$ git tag v1.4-lw
```

- Executing this command creates a lightweight tag identified as v1.4-lw.
- Lightweight tags are created with the **absence of the -a, -s, or -m options.**
- Lightweight tags create a new tag checksum and store it in the .git/ directory of the project's repo.

###### Listing Tags

```
	$ git tag
```

- This will output a list of tags:
- To refine the list of tags the -l option can be passed with a wild card expression:

```
	$ git tag -l "*beta*"

```

- Returns a list of all tags marked with a -beta prefix,

###### Tagging Old Commits

- By default, git tag will create a tag on the commit that **HEAD** is referencing.
- Alternatively git tag can be **passed as a ref to a specific commi**t.
- This will tag the passed commit instead of defaulting to HEAD.

```
	$ git tag -a v1.2 15027957951b64cf874c3557a0f3547bd83b3ff6
```

- Executing the above git tag invocation will create a new annotated commit identified as v1.2 for the commit we selected

###### ReTagging/Replacing Old Tags

- If you try to create a tag with the same identifier as an existing tag, Git will throw an error like:
<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
			fatal: tag 'v0.4' already exists
	</div>

- Additionally if you try to tag an older commit with an existing tag identifier Git will throw the same error.
- In the event that you must update an existing tag, the -f FORCE option must be used.

```
	$ git tag -a -f v1.4 15027957951b64cf874c3557a0f3547bd83b3ff6

```

- Executing the above command will map the 15027957951b64cf874c3557a0f3547bd83b3ff6 commit to the v1.4 tag identifier. It will override any existing content for the v1.4 tag.

###### Sharing: Pushing Tags to Remote

- Sharing tags is similar to pushing branches. **By default, git push will not push tags.** Tags have to be explicitly passed to git push.

```
	$ git push origin v1.4
```

- To push multiple tags simultaneously pass the --tags option to git push command.
- When another user clones or pulls a repo they will receive the new tags.

```
	$ git push --tags
```

###### Checking Out Tags

- You can view the state of a repo at a tag by using the git checkout command.

```
	$ git checkout v1.4
```

- The above command will checkout the v1.4 tag. **This puts the repo in a detached HEAD state.**
- This means any changes made will not update the tag. They will create a new detached commit.
- This new detached commit will not be part of any branch and will only be reachable directly by the commits SHA hash.
- Therefore it is a best practice to **create a new branch anytime you're making changes in a detached HEAD state.**

###### Deleting Tags

- Deleting tags is a straightforward operation. Passing the **-d** option and a tag identifier to git tag will delete the identified tag.

```
	$ git tag -d v1
```
