## Working with snapshots and the Git staging area.

For the basic workflow of staging content and committing it to your history, there are only a few basic commands.

|  Command |  Example |
|:-------------:|:----------------:|
| [git status](#git-status)       |  ``` bash git status```  |
| [git add](#git-add)        |  ``` bash git add myfile.txt```  |
| [git reset](#git-reset)        |  ``` bash git reset myfile.txt```  |
| [git diff](#git-diff)        |  ``` bash git diff```  |
| [git commit](#git-commit)        |  ``` bash git commit -m "Add new feature"```  |



### git status

Shows the status of your working directory, indicating which files are modified, untracked, and staged for the next commit.
```bash
git status
```

This command provides a detailed summary of the changes in your working directory. Files listed under "Changes not staged for commit" are modified but not staged, and files under "Changes to be committed" are staged for the next commit.

### git add

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

Unstages changes in the specified file, reverting it to the state in the last commit, while retaining the changes in the working directory.

```bash
# Unstage the changes in myfile.txt
git reset myfile.txt
```

This command removes `myfile.txt` from the staging area, but the changes made to the file remain in the working directory.

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

Commits the changes that are staged in the snapshot, with a descriptive commit message.

```bash
# Commit the changes with a message
git commit -m "Add new feature"
```

This command creates a new commit with the staged changes and the specified commit message.

- Combine `git add` and `git commit` in one step using `git commit -am "[descriptive message]"` to stage and commit changes.

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

#### Interactive Commit
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
| [git rm](#git-rm)        |  ``` bash git rm file.txt```  |
| [git mv](#git-mv)        |  ``` bash git mv oldfile.txt newfile.txt```  |
| [git clean](#git-clean)        |  ``` bash git clean -ffd```  |
| [git log](#git-log---stat--m)        |  ``` bash git log --stat -M```  |


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