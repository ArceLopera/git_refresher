## The Lifecycle of Git files

Understanding the lifecycle of the status of files in Git is crucial for effectively managing changes in your repository. The lifecycle consists of several stages that files can transition through as you work with Git. These stages are reflected in the output of `git status` and represent the state of files in relation to the repository. 

- **Transition between Stages:** Files transition between these stages as you work with Git commands like `git add`, `git commit`, and others.
- **Workflow:** The typical workflow involves modifying files in the working directory, staging the changes with `git add`, and committing the changes with `git commit`.
- **Visibility:** Use `git status` to view the current status of files in your repository and identify which stage they are in.

| Stage         | Description                 |  Representation                   | Action                       |
|---------------|----------------------------|----------------------|-----------------------------------|
| Untracked     | Files that exist in your working directory but are not yet tracked by Git. These files have not been added to the repository. |Shown in red in the output of `git status`.|Use `git add <file>` to stage untracked files for the next commit. |
| Tracked  (or Modified)     | Files that have been modified in the working directory after being staged or committed. | Shown in red in the output of `git status`.| Use `git add <file>` to stage the modified files, then commit the changes with `git commit`.                               |
|Deleted|Files that have been deleted from the working directory after being staged or committed.|Shown in red in the output of `git status`.|Use `git add <file>` to stage the deletion, then commit the changes with `git commit`.|
|Renamed or Moved| Files that have been renamed or moved in the working directory after being staged or committed.|Shown as both deleted and untracked files in the output of `git status`.|Use `git add <file>` to stage the rename or move, then commit the changes with `git commit`.|
| Staged (or Indexed)        |  Files that have been added to the staging area. These changes are ready to be included in the next commit. |Shown in green in the output of `git status`.| Use `git commit` to create a commit containing the staged changes.   |
| Committed (or Unmodified)    | Files are stored securely in the Git repository. They are unchanged and represent a specific snapshot in the project's history. Files that have not been modified since the last commit. | Typically not displayed in the output of `git status` unless using certain flags.|   No action required unless you intend to stage or modify the files.   |

![](../Images/gitstcycle.png)

The **working tree** is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.

The **staging area** is a file, generally contained in your Git directory, that stores information about what will go into your next commit. 

The **Git directory** is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you clone a repository from another computer.


**Working with snapshots and the Git staging area**

For the basic workflow of staging content and committing it to your history, there are only a few basic commands.

|  Command |  Example |
|:-------------:|:----------------:|
| [git status](#git-status)       |  ``` git status```  |
| [git add](#git-add)        |  ``` git add myfile.txt```  |
| [git reset](#git-reset)        |  ``` git reset myfile.txt```  |
| [git diff](#git-diff)        |  ``` git diff```  |
| [git commit](#git-commit)        |  ``` git commit -m "Add new feature"```  |

## Snapshooting
The difference between Git's snapshooting and delta-based version control systems lies in how git store and track changes within a repository.

1. **Git's Snapshooting**
 Git is a distributed version control system (DVCS) that operates based on snapshots of the entire repository at each point in time.
 When a commit is made in Git, the entire state of the project at that moment is captured as a snapshot, including all files and their contents. Each commit creates a new snapshot, and Git tracks the changes between snapshots rather than individual file changes.
    - **Advantages:**
        - Offers robustness and data integrity since each commit is self-contained and immutable.
        - Allows for efficient branching and merging operations since entire snapshots are managed independently.
        - Facilitates offline work and collaboration by enabling users to work with their local repositories and synchronize changes later.

2. **Delta-based Version Control (CVS, Subversion, Perforce)**
 Delta-based version control systems track changes by storing the differences (or deltas) between successive versions of a file or repository.
 Instead of storing complete snapshots of files or repositories, delta-based systems store the changes (insertions, deletions, modifications) made to files over time. Each new version is represented as a set of changes applied to the previous version.
    - **Advantages:**
        - Can be more space-efficient for repositories with large binary files or frequent, small changes since only the differences are stored.
        - Allows for quicker network operations when transmitting changes since only the deltas need to be transferred.
        - May provide more granular control over individual file versions, especially for large files with small modifications.

**Key Differences**

+ **Granularity:** Git's snapshot-based approach captures the entire state of the repository at each commit, offering a holistic view of the project's history. Delta-based systems track changes at a finer granularity, focusing on individual file modifications.

+ **Efficiency:** Git's approach can be less space-efficient for repositories with frequent changes or large binary files since it stores complete snapshots. Delta-based systems may offer better efficiency in such cases by only storing the differences between versions.

+ **Complexity:** Managing deltas and applying them to reconstruct files can introduce complexity and potential performance overhead in delta-based systems. Git's snapshot-based approach simplifies operations like branching and merging since each commit represents a complete, self-contained snapshot.

+ **Network Operations:** Git's approach may lead to larger repository sizes due to storing complete snapshots, but it offers efficient network operations since it transfers complete commits during push and pull operations. Delta-based systems may have smaller repository sizes but may require more processing to reconstruct files during network operations.

Overall, Git's snapshot-based approach provides robustness, simplicity, and efficiency for version control, making it a popular choice for software development projects of all sizes. Delta-based version control systems offer advantages in specific use cases where space efficiency or granular control over individual file versions is paramount.


### git status

Shows the status of your working directory, indicating which files are modified, untracked, and staged for the next commit.
```bash
git status
```

This command provides a detailed summary of the changes in your working directory. Files listed under "Changes not staged for commit" are modified but not staged, and files under "Changes to be committed" are staged for the next commit.

### git add

![](../Images/add.png)

Adds changes in the specified file to the staging area, preparing them for the next commit.

```bash
# Create or modify a file
echo "Hello, Git!" > myfile.txt

# Stage the changes
git add myfile.txt
```

This sequence of commands creates or modifies `myfile.txt` and stages the changes for the next commit.

Use `git add .` to stage all changes in the working directory.

### git reset

![](../Images/reset.png)

 The `git reset` command in Git is a powerful tool for manipulating the commit history, moving branches, and resetting the staging area. It has several options, each affecting different parts of the Git repository. 

#### **git reset --soft <commit\>**

Resets the current branch's HEAD to the specified commit, leaving the changes staged.
Moves the HEAD pointer to the specified commit, keeping the changes from commits after that commit staged for a new commit.

```bash
git reset --soft HEAD~3
```
This command resets the current branch's HEAD to three commits behind the current position, keeping the changes staged. It essentially undid the last 3 git commit commands.You could now update the index (by adding or removing files) and run git commit again to accomplish what git commit --amend would have done.

#### **git reset --mixed <commit\>**
Resets the current branch's HEAD to the specified commit, unstaging the changes. This is also the default, so if you specify no option at all (just git reset HEAD~3 in this case).
Moves the HEAD pointer to the specified commit, unstaging the changes from commits after that commit.

```bash
git reset --mixed HEAD~3
```
This command resets the current branch's HEAD to three commits behind the current position, unstaging the changes.  You rolled back to before you ran all your git add and git commit commands.

#### **git reset --hard <commit\>**
Resets the current branch's HEAD to the specified commit, discarding all changes.
Moves the HEAD pointer to the specified commit, discarding all changes made after that commit.

```bash
git reset --hard HEAD~3
```
This command resets the current branch's HEAD to three commits behind the current position, discarding all changes. Use when you want to completely replace the current branch with the specified commit by discarding all changes made after that commit.

Remember to use `git reset` with caution, especially the `--hard` option, as it can lead to irreversible data loss. Always make sure you understand the consequences of the reset operation before executing it.

#### **git reset file.txt**
The command git reset with a path to a file (since a commit SHA-1 or branch is not specified) is shorthand for git reset --mixed HEAD file.txt. It will:

1. Move the branch HEAD points to (skipped).

2. Make the index look like HEAD (stop here).

So it essentially just copies file.txt from HEAD to the index. This has the practical effect of unstaging the file. If we look at the diagram for that command and think about what git add does, they are exact opposites. This is why the output of the git status command suggests that you run this to unstage a file. 

We could just as easily not let Git assume we meant “pull the data from HEAD” by specifying a specific commit to pull that file version from. We would just run something like:
```	bash
git reset eb43bf file.txt
```

#### git reset vs git checkout

##### Without Paths
Running git checkout [branch] is pretty similar to running git reset --hard [branch] in that it updates all three trees for you to look like [branch], but there are two important differences.

1. Unlike git reset --hard, git checkout is working-directory safe.  It tries to do a trivial merge in the  working directory, so all of the files you haven’t changed will be updated. git reset --hard, on the other hand, will simply replace everything across the board without checking.

2. git checkout will move HEAD itself to point to another branch or commit, while git reset will move the branch that HEAD points to. That is why when using git checkout you may end up with a detached HEAD, which is not possible with git reset. 

##### With Paths 

The other way to run checkout is with a file path, which, like reset, does not move HEAD. It is just like git reset [branch] file in that it updates the index with that file at that commit, but it also overwrites the file in the working directory. It would be exactly like git reset --hard [branch] file (if reset would let you run that) — it’s not working-directory safe, and it does not move HEAD.

#### Cheat-sheet 

This table shows a cheat-sheet for which commands affect which trees. The “HEAD” column reads “REF” if that command moves the reference (branch) that HEAD points to, and “HEAD” if it moves HEAD itself. Pay especial attention to the 'WD Safe?' column — if it says NO, take a second to think before running that command.

||HEAD|	Index|	Workdir|	WD Safe?|
|---|---|---|---|---|
|Commit Level|||||
|reset --soft [commit]|REF|NO|NO|YES|
|reset [commit]|REF|YES|NO|YES|
|reset --hard [commit]|REF|YES|YES|NO|
|checkout <commit>|HEAD|YES|YES|YES|
|File Level|||||
|reset [commit] <paths>|NO|YES|NO|YES|
|checkout [commit] <paths>|NO|YES|YES|NO|

### git diff

The git diff command is used when you want to see differences between any two trees. This could be the difference between your working environment and your staging area (git diff by itself), between your staging area and your last commit (git diff --staged), or between two commits (git diff master branchB).


```bash
# Make changes to myfile.txt
echo "Updated content" >> myfile.txt

# View the differences
git diff
```

This command displays the unstaged changes in `myfile.txt` since the last commit.

#### git diff --staged

Shows the differences between the staging area and the last commit (changes that are staged but not yet committed).

```bash
# Stage the changes in myfile.txt
git add myfile.txt

# View the differences in the staging area
git diff --staged
```

This command displays the changes that are currently staged for the next commit.

#### git difftool
The git difftool command simply launches an external tool to show you the difference between two trees in case you want to use something other than the built in git diff command.

### git commit 

The `git commit` command in Git is a powerful tool for creating and committing changes. It has several options, each affecting different parts of the Git repository.

```bash
git commit
```
When you run `git commit` without any arguments, it opens your editor of choice to create a commit message.

You can see that the default commit message contains the latest output of the git status command commented out and one empty line on top. You can remove these comments and type your commit message, or you can leave them there to help you remember what you’re committing.
When you exit the editor, Git creates your commit with that commit message (with the comments and diff stripped out).


Alternatively, you can type your commit message inline with the commit command by specifying it after a -m flag. This commits the changes that are staged in the snapshot, with a descriptive commit message.

```bash
# Commit the changes with a message
git commit -m "Add new feature"
```

This command creates a new commit with the staged changes and the specified commit message.

- Combine `git add` and `git commit` in one step using `git commit -am "[descriptive message]"` to stage and commit changes.

For an even more explicit reminder of what you’ve modified, you can pass the -v option to git commit. Doing so also puts the diff of your change in the editor so you can see exactly what changes you’re committing.

#### Committing Specific Files

```bash
# Stage changes in specific files
git add file1.txt file2.txt

# Commit the changes with a message
git commit -m "Fix issues in file1.txt and file2.txt"
```

Here, only specific files (`file1.txt` and `file2.txt`) are staged and then committed with a message describing the fix.

#### Amending the Last Commit

```bash
# Make additional changes
echo "Additional content" >> myfile.txt

# Amend the last commit with new changes
git add myfile.txt
git commit --amend -m "Update myfile.txt with additional content"
```

This example adds new content to `myfile.txt` and then amends the last commit with the additional changes.

#### [Interactive Commit](../Advanced/git_interactivestaging.md)
```bash
# Interactively stage changes
git add -i

# Follow the interactive prompts to stage changes and commit
```

Running `git add -i` opens an interactive mode where you can choose which changes to stage, unstage, or commit. This provides a more granular control over the commit process.

#### Committing with Multiple Messages

```bash
# Stage changes in files
git add file1.txt file2.txt

# Commit changes with multiple messages
git commit -m "Fix issues in file1.txt" -m "Add new feature in file2.txt"
```

You can use the `-m` option multiple times to provide multiple commit messages. Each `-m` adds a new paragraph to the commit message.

#### Committing with a Future Date

```bash
# Stage changes
git add .

# Commit with a future date
git commit --date="2023-01-01T12:00:00" -m "Commit with a future date"
```

This example commits changes with a specified future date using the `--date` option.

#### Committing Only Tracked Changes

```bash
# Stage changes in tracked files
git add -u

# Commit only tracked changes
git commit -m "Commit tracked changes"
```

The `git add -u` command stages only tracked files, and then a commit is made with a message indicating the commit of tracked changes.

## Tracking Path Changes

Versioning file removes and path changes

|  Command |  Example |
|:-------------:|:----------------:|
| [git rm](#git-rm)        |  ``` git rm file.txt```  |
| [git mv](#git-mv)        |  ``` git mv oldfile.txt newfile.txt```  |
| [git clean](#git-clean)        |  ``` git clean -ffd```  |
| [git log](#git-log---stat--m)        |  ``` git log --stat -M```  |

![](../images/rm.png)

### git rm

Removes files from both your working directory and the Git repository. It stages the removal of the specified files, and you need to commit to apply the changes.

```bash
# Remove a file from the working directory and stage the removal
git rm file.txt

# Commit the removal
git commit -m "Remove file.txt"
```

This sequence removes `file.txt` from both the working directory and the Git repository, and the removal is committed.

### git mv

Renames or moves files in both your working directory and the Git repository. Similar to `git rm`, it stages the rename/move, and you need to commit to apply the changes.

```bash
# Rename a file and stage the change
git mv oldfile.txt newfile.txt

# Commit the rename
git commit -m "Rename oldfile.txt to newfile.txt"
```

This example renames `oldfile.txt` to `newfile.txt`, stages the change, and commits the rename.

- With `git mv`, you can also use it to move files to a different directory, like `git mv file.txt new_directory/`.
- Use `-r` with `git rm` and `git mv` to handle removals or moves recursively in directories.

### git clean

Removes untracked files from your working directory. It's useful for cleaning up untracked files that haven't been staged or committed.

```bash
# List untracked files that will be removed (dry run)
git clean -n

# Remove untracked files
git clean -f
```

The first command (`git clean -n`) is a dry run that shows you what files would be removed. The second command (`git clean -f`) actually removes the untracked files.

- `git clean` has additional options, such as `-i` for an interactive mode where you can choose which files to clean.

### git log --stat -M

This command is used to display the commit history along with statistics about the changes introduced in each commit, including information about file modifications, additions, and deletions. The `-M` option is particularly used to detect file renames.

Let's break down the components of this command:

- `git log`: Displays the commit logs.
- `--stat`: Includes additional statistics at the end of each commit entry, providing a summary of changes.
- `-M`: Enables Git's rename detection, which identifies file renames between commits.

```bash
git log --stat -M
```

This command will output a detailed log that includes commit information and statistics. The statistics section shows the number of lines added and removed for each file affected by the commit. Additionally, if a file has been renamed, Git will provide information about the rename.

Here is a simplified example of what the output might look like:

```
commit abcd1234 (HEAD)
Author: John Doe <john.doe@example.com>
Date:   Tue Jan 18 12:00:00 2024 +0000

    Updated README.md

 README.md | 10 +++++-----
 1 file changed, 5 insertions(+), 5 deletions(-)
```

In this example:

- `commit abcd1234`: The unique identifier of the commit.
- `Author`: The author of the commit.
- `Date`: The date and time of the commit.
- `Updated README.md`: The commit message.

The statistics section (`README.md | 10 +++++-----`) indicates that in the file `README.md`, 10 lines were added, and 5 lines were removed. This section provides a quick overview of the changes made in each commit.

The `-M` option enhances this by detecting file renames. If a file has been renamed, Git will track the rename and display the old and new filenames.

- The `-M` option can take an optional value to set a similarity index for rename detection. For example, `-M90%` would consider files as renamed if 90% of their content is similar.
- Using `-M` can be especially useful when you want to track the history of a file that has been renamed over time.
