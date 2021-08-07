# GIT CONFIG

- The git config command can accept arguments to specify which configuration level to operate on.
- The following configuration levels are available:
  - --local
  - --global
  - --system
- By default, git config will write to a local level if no configuration option is passed.
- **Local** configuration values are stored in a file that can be found in the repo's .git directory: **.git/config**

- **Globa**l level configuration is user-specific, meaning it is applied to an operating system user.
- **Global** configuration values are stored in a file that is located in a user's home directory.
- ~ **/.gitconfig** on unix systems and **C:\Users\\.gitconfig** on windows
- **System-level configuration** is applied across an entire machine. This covers all users on an operating system and all repos.
- The **system** level configuration file lives in a gitconfig file off the system root path.
- $(prefix)/etc/gitconfig on unix systems.
- Thus the **_order of priority_ f**or configuration levels is: local, global, system.

---

## Writing a value

- **User Details**

```
	$ git config --global user.email "your_email@example.com"
```

- **Default Editor**
  - Many Git commands will launch a text editor to prompt for further input. One of the most common use cases for git config is configuring which editor Git should use.

```
	 $ git config --global core.editor "code --wait"
```

- **Merge tools**
  - In the event of a merge conflict, Git will launch a "merge tool." By default.
  - Git uses an internal implementation of the common Unix diff program.
  - The internal Git diff is a minimal merge conflict viewer. There are many external third party merge conflict resolutions that can be used instead

```
	$ git config --global merge.tool kdiff3
```

- **Colored outputs**
  - Git supports colored terminal output which helps with rapidly reading Git output.
  - You can customize your Git output to use a personalized color theme.
- **Aliases**

```
	$ git config --global alias.ci commit
	$ git config --global alias.amend ci --amend

```
