## What is in .git file

- objects
- config
- HEAD
- index
- refs
- and many more..............

### refs Folder

- Inside of refs, you'll find a **heads directory** : **refs/heads**
- refs/heads contains one file per branch .
- Each file is named after a branch and contains the hash of the commit at the tip of the branch.
- Refs also contains a **refs/tags** folder which conatins one file for each tag in the repo.

### HEAD

- HEAD is just a text file that keeps **track of where HEAD points.**
- If it contains **refs/heads/master,** this means that **HEAD is pointing to the master branch.**
- In **detached HEAD,** the HEAD file contains a commit hash instead of a branch reference

### Index

- The index file is a binary file that contains a list of the files the repository is tracking.
- It stores the file names as well as some metadata for each file.
- Not that the index does NOT store the actual contents of files. It only contains references to files.

### Objects Folder

- The objects directory contains all the repo files.
- This is where Git stores the backups of files, the commits in a repo, and more.
- The files are all compressed and encrypted, so they won't look like much!
- **4 Types of Git Objects :**
  - **commit**
  - **tree**
  - **blob**
  - **annotated tag**
- Git uses a **hashing function called SHA-1** (though this is set to change eventually).
- SHA-1 always generates **40-digit hexadecimal numbers.**
- **Git Database**
  - Git is a **key-value** data store.
  - We can insert any kind of content into a Git repository, and Git will hand us back a unique key we can later use to retrieve that content.
  - These keys that we get back are **SHA-1 checksums.**
- **Git uses SHA-1 to hash our files, directories, and commits.**
- The **git hash-object command** takes some data, stores in our .git/objects directory and give us back the unique SHA-1 hash that refers to that data objects.

```
	$ git hash-object <file>
```

```
	$ echo "hello" | git hash-object --stdin
```

- Rather than simply outputting the key that git would store our objectunder, we can use th**e -w option** to tell git to actually write the object to the database.

```
		$ echo "hello" | git hash-object --stdin -w
```

- After running this command, check out the contents of .git/objects
- Now that we have data stored in our Git object database, we can try **retrieving** it using the **git cat-file command.**

```
	$ git cat-file -p <object-hash>
```

- The **-p option** tells Git to pretty print the contents of the object based on its type.

### Blobs

- Git **blobs (binary large object)** are the object type Git uses to **store the contents of files** in a given repository.
- **Blobs don't even include the filenames** of each file or any other data. They just store the contents of a file!

### Trees

- Trees are Git objects used to store the **contents of a directory.**
- Each tree contains pointers that can refer to blobs and to other trees.
- Each entry in a tree contains the SHA-1 hash of a blob or tree, as well as the mode, type, and filename.

**tree**
| blob | 1f7a7a47... | app.js |
|------|-------------|------------|
| tree | 982871aa... | images_dir |
| blob | be321a77... | README |

- **Viewing Trees**

```
$ git cat-file -p master^{tree}
```

- Remember that **git cat-file prints** out Git objects. In this example, the master^{tree} syntax specifies the tree object that is pointed to by the tip of our master branch.

## commit

- Commit objects combine a tree object along with information about the context that led to the current tree.
- Commits store **a reference to parent commit(s), the author, the commiter**, and of course the **commit message!**
- When we run **git commit**, Git creates a new commit object whose parent is the current HEAD commit and **whose tree is the cuntent of the current content of the index**
