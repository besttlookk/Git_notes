# git remote / fit fetch / git push / git pull

## 1. Git remote

- The **git remote** command lets you **create**, **view**, and **delete** **connections to other repositories.**
- Remote connections are more like bookmarks rather than direct links into other repositories.
- Instead of providing real-time access to another repository, they serve as **convenient names** that can be used to reference a **not-so-convenient URL.**
- The git remote command is essentially an **interface for managing a list of remote** entries that are stored in the repository's **./.git/config file.**
- To liist the remote connections you have to other repositories.

```
	$ git remote
```

- include the URL of each connection.

```
	$ git remote -v
```

##### Creating and modifying git remote configurations

- The git remote command is also a convenience or 'helper' method for modifying a repo's **./.git/config file.**
- The following commands will modify the repo's /.git/config file. The result of the following commands can also be achieved by directly editing the ./.git/config file with a text editor.

```
	$ git remote add <name> <url>
```

- After adding a remote, you’ll be able to use ＜ name ＞ as a convenient shortcut for ＜ url ＞ in other Git commands.
- **Remove** the connection to the remote repository called ＜ name ＞.

```
			$ git remote rm <name>
```

- **Rename** a remote connection from ＜ old-name ＞ to ＜ new-name ＞.

```
	$ git remote rename <old-name> <new-name>
```

- Git is designed to give each developer an entirely isolated development environment.
- This means that information is not automatically passed back and forth between repositories.
- Instead, developers need to manually pull upstream commits into their local repository or manually push their local commits back up to the central repository.
- The **git remote command** is really just an easier way to pass URLs to these "sharing" commands.
- When you **clone a repository with git clone**, it a**utomatically creates a remote connection called origin pointing back to the cloned repository.**
- This is useful for developers creating a local copy of a central repository, since it provides an easy way to pull upstream changes or publish local commits
- **This behavior is also why most Git-based projects call their central repository origin.**

---

## 2. git fetch

- The git fetch command downloads c**ommits, files, and refs** from a remote repository into your local repo.
- Fetching is what you do when you want to see what everybody else has been working on. **but it doesn’t force you to actually merge the changes into your repository.**
- Git isolates fetched content from existing local content; it has absolutely no effect on your local development work.
- Fetched content has to be **explicitly checked** out using the **git checkout** command.
- This makes fetching a safe way to review commits before integrating them with your local repository.
- When downloading content from a remote repo, **git pull and git fetch** commands are available to accomplish the task. You can consider git fetch the 'safe' version of the two commands.
- **git pull is the more aggressive alternative;** it will download the remote content for the active local branch and immediately execute git merge to create a merge commit for the new remote content.
- If you have pending changes in progress this will cause conflicts and kick-off the merge conflict resolution flow.
- Behind the scenes, in the **repository's ./.git/objects directory, Git stores all commits, local and remote.**
- Git keeps remote and local branch commits distinctly separate through the use of branch refs.
- The **refs for loca**l branches are stored in the **./.git/refs/heads/.**
- Executing the **git branch** command will output a list of the local branch refs.
- Remote branches are just like local branches, except they map to commits from somebody else’s repository.
- Remote branches are **prefixed by the remote** they belong to so that you don’t mix them up with local branches.
- Like local branches, Git also has refs for remote branches.
- **Remote branch** refs live in the **./.git/refs/remotes/ directory.**

```
	$ git branch -r
```

###### output

<div style="background-color: #282A35; color: white; padding:10px; margin:20px; border-radius:5px"> 
			# origin/main<br>
# origin/feature1<br>
# origin/debug2<br>
# remote-repo/main<br>
# remote-repo/other-feature
	</div>
	
* This output displays the local branches we had previously examined but now displays them p**refixed with origin/.**
* Additionally, we now see the remote branches **prefixed with remote-repo.**
* You can check out a remote branch just like a local one, **but this puts you in a detached HEAD state** (just like checking out an old commit). 
* You can think of them as **read-only branches.** 
* To view your remote branches, simply pass the **-r flag** to the git branch command.
* You can inspect remote branches with the usual **git checkout** and **git log commands.**
* If you approve the changes a remote branch contains, you can merge it into a local branch with a **normal git merge.**
* The git pull command is a convenient shortcut for this process.
* **Fetch all of the branches from the repository.** 
```
	$ git fetch <remote>
```
* This also downloads all of the required commits and files from the other repository.
* **only fetch the specified branch.**
```
	$ git fetch <remote> <branch>
```
* **A power move which fetches all registered remotes and their branches:**
```
	$ git fetch --all
```
* **The --dry-run option will perform a demo run of the command.** It will output examples of actions it will take during the fetch but not apply them.
```
	$ git fetch --dry-run
```
* When we clone, By default, my master branch is already tracking origin/master
* But we do not get files for other remote branches. We get only names of remote branch only
* We could checkout origin/puppies, but that puts me in detached HEAD.
* I want my own local branch called puppies, and I want it to be connected to origin/puppies, just like my local master  branch is connected to origin/master.
* Run **git switch** to create a new local branch from the remote branch of the same name.
```
	$ git switch puppies
```
#### OR
```
	$ git checkout --track origin/puppies
```
* **git switch puppies** makes me a local puppies branch AND sets it up to track the remote branch origin/puppies.

---

## 3. git pull

- The git pull command is used to **fetch and download conten**t from a remote repository and immediately update the local repository to match that content.
- The git pull command is actually a **combination** of two other commands, **git fetch** followed by **git merge**
- A new merge commit will be-created and **HEAD updated to point at the new commit.**
- A **--rebase option** can be passed to git pull to use a rebase merging strategy instead of a merge commit.
- To pull, we specify the particular remote and branch we want to pull

```
	$ git pull <remote> <branch>
```

#### OR

```
	$ git pull <remote>
```

- This is the same as **git fetch [remote]** followed by **git merge origin/＜ current-branch ＞**
- J**ust like with git merge, it matters WHERE we run this command from.**
- Whatever branch we run it from is where the changes will be merged into.
- **git pull origin master** would fetch the latest information from the origin's master branch and merge those changes into our current branch.
- Fetches the remote content but does not create a new merge commit.

```
	$ git pull --no-commit <remote>
```

- use rebase instead of merge

```
	$ git pull --rebase <remote>
```

- Gives verbose output during a pull which displays the content being downloaded and the merge details.

```
	$ git pull --verbose
```

###### pulls can result in merge conflicts!!

```
	$ git pull
```

- If we **run git pull without specifying a particular remote or branch to pull from,** git assumes the following:
_ remote will default to origin
_ branch will default to whatever tracking connection is configured for your current branch.
<div style="color: red">
	Note: this behavior can be configured, and tracking
connections can be changed manually. Most people dont
mess with that stuff
</div>

---

## 4. git push

- The git push command is used to upload local repository content to a remote repository
- It's the counterpart to git fetch,
- Pushing has the potential to overwrite changes, caution should be taken when pushing.
- **Push the specified branch to , along with all of the necessary commits and internal objects**.

```
	$ git push <remote> <branch>
```

- This creates a local branch in the destination repository.
- To prevent you from overwriting commits, Git won’t let you push when it results in a non-fast-forward merge in the destination repository.
- **force the push even if it results in a non-fast-forward merge**

```
	$ git push <remote> --force
```

<div style="color: red">
Do not use the --force flag unless you’re absolutely sure you know what you’re doing.
</div>

- **Push all of your local branches to the specified remote.**

```
	$ git push <remote> --all
```

- ags are not automatically pushed when you push a branch or use the --all option. The **--tags flag** sends all of your local tags to the remote repository.

```
	$ git push <remote> --tags
```
