Plumbing tools in Git are low-level commands that interact directly with the Git repository's internal data structures, providing access to detailed information about commits, branches, and other objects. They are primarily used for scripting, automation, or accessing detailed repository information that may not be readily available with porcelain commands (higher-level commands). They provide direct access to the Git object database and are essential for building more complex Git workflows and tools.

## **`git rev-parse`**

This command is used to parse Git references such as branch names, commit hashes, tags, and symbolic references into commit identifiers or other meaningful information.
It can be used to retrieve the SHA-1 hash of commits, tags, and other Git objects, or to resolve symbolic references like branch names to their corresponding commit hashes.

```bash
git rev-parse HEAD         # Get the commit hash of the current HEAD
git rev-parse main         # Get the commit hash of the 'main' branch
git rev-parse v1.0         # Get the commit hash of the tag 'v1.0'
```

## **`git reflog`**

This command displays the commit history of the local repository's HEAD reference, showing a chronological list of all recent HEAD movements and associated commit identifiers.
It's useful for recovering lost commits or understanding changes to the repository's history, especially after potentially destructive operations like rebasing or resetting.

```bash
git reflog                # Show the reflog for the current branch
git reflog main           # Show the reflog for the 'main' branch
```

## **`git cat-file`**

This command provides various information about Git objects (commits, trees, tags, or blobs).
It can be used to display the contents, type, size, or even raw data of a specific Git object.

```bash
git cat-file -p HEAD      # Show the contents of the commit pointed to by HEAD
git cat-file -t v1.0      # Show the type of the tag 'v1.0'
```

## **`git hash-object`**
This command computes the SHA-1 hash of a given file or data and optionally stores it in the Git object database. It's commonly used for hashing file contents before storing them as Git objects or verifying data integrity.

```bash
git hash-object file.txt  # Compute the SHA-1 hash of 'file.txt'
```

## **`git ls-tree`**
This command lists the contents of a tree object (directory) in the Git object database.
It can be used to view the files and subdirectories stored within a specific tree object.

```bash
git ls-tree HEAD          # List the contents of the commit pointed to by HEAD
```

## **`git update-ref`**
This command allows you to update the references in the Git repository, such as branches or tags, directly. It's commonly used for advanced Git operations or scripting workflows that involve manipulating references.
   
```bash
git update-ref refs/heads/main <commit>  # Update the 'main' branch to point to a specific commit
```

## **`git commit-tree`**
This command creates a new commit object based on the provided tree object and parent commit(s) and optionally stores it in the Git object database.It's typically used in advanced Git workflows or scripting scenarios where manual creation of commits is necessary.

```bash
git commit-tree <tree> -p <parent> -m "Commit message"  # Create a new commit object with the given tree and parent commit
```
