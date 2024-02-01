Undoing changes in Git involves various commands and strategies, depending on the nature of the changes and the stage at which they are. Here is a comprehensive guide on common Git undo commands and strategies:

Summary table of common Git undo commands and strategies:

| Action                                    | Command                                                     | Description                                                                                                                                                               |
|-------------------------------------------|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**Undoing Uncommitted Changes**](#1-undoing-uncommitted-changes-in-the-working-directory)            | `git checkout -- file.txt`                                  | Discard changes in a specific file, reverting it to the state in the last commit.                                                                                          |
|                                           | `git reset --hard`                                          | Discard all uncommitted changes, reverting the working directory to the state of the last commit.                                                                          |
| [**Undoing Staged Changes**](#2-undoing-staged-changes)                 | `git reset file.txt`                                        | Unstage changes in a specific file, moving them back to the working directory.                                                                                             |
| [**Undoing Commits**](#3-undoing-commits)                        | `git reset --soft HEAD^`                                    | Undo the last commit, keeping changes staged in the working directory.                                                                                                     |
|                                           | `git reset --mixed HEAD^`                                   | Undo the last commit, unstaging changes but keeping them in the working directory.                                                                                         |
|                                           | `git reset --hard HEAD^`                                    | Undo the last commit, discarding changes both in the commit and the working directory.                                                                                     |
|                                           | `git commit --amend`                                        | Amend the last commit by adding new changes or modifying the commit message.                                                                                                |
| [**Reverting Commits**](#4-reverting-commits)                      | `git revert <commit-hash>`                                  | Create a new commit that undoes changes introduced by a specific commit.                                                                                                   |
| [**Undoing Remote Changes**](#5-undoing-remote-changes)                 | `git push --force origin <branch-name>`                     | Force push local changes to a remote branch (use with caution, as it rewrites remote history).                                                                             |
| [**Resetting to a Specific Commit**](#6-resetting-to-a-specific-commit)         | `git reset --hard <commit-hash>`                            | Reset the branch to a specific commit, discarding all changes after that commit.                                                                                            |
| [**Cleaning Untracked Files**](#7-cleaning-untracked-files)               | `git clean -fd`                                             | Remove untracked files and directories from the working directory.                                                                                                         |
| [**Interactively Reverting or Editing**](#8-interactively-reverting-or-editing-commits)    | `git rebase -i <commit-hash>`                               | Open an interactive rebase session, allowing you to edit, squash, or drop commits.                                                                                           |
| [**Restoring Files**](#9-restoring-files)                       | `git restore file.txt`                                      | Restore the specified file to the state in the last commit (introduced in Git 2.23).                                                                                        |

Please note that some commands, like force push and hard reset, should be used with caution, especially when collaborating with others, as they can alter Git history. Always be mindful of the consequences and potential impacts on collaborators before executing these commands.

### 1. **Undoing Uncommitted Changes in the Working Directory**

- **Discard Changes in a File**
  ```bash
  git checkout -- file.txt
  ```

  This command discards changes in the specified file, reverting it to the state in the last commit. It’s important to understand that `git checkout -- <file>` is a dangerous command. Any local changes you made to that file are gone — Git just replaced that file with the last staged or committed version. Don’t ever use this command unless you absolutely know that you don’t want those unsaved local changes.

- **Discard All Changes**
  ```bash
  git reset --hard
  ```

  This command discards all uncommitted changes in the working directory, reverting it to the state of the last commit.

### 2. **Undoing Staged Changes**

- **Unstage Changes**
  ```bash
  git reset file.txt
  ```

  This command unstages changes in the specified file, moving them back to the working directory.

### 3. **Undoing Commits**

- **Undo the Last Commit (soft reset)**
  ```bash
  git reset --soft HEAD^
  ```

  This command undoes the last commit, keeping the changes staged in the working directory.

- **Undo the Last Commit (mixed reset, default)**
  ```bash
  git reset --mixed HEAD^
  ```

  This command undoes the last commit, unstaging the changes but keeping them in the working directory.

- **Undo the Last Commit (hard reset)**
  ```bash
  git reset --hard HEAD^
  ```

  This command undoes the last commit, discarding the changes both in the commit and the working directory.

- **Amend the Last Commit**
  ```bash
  git commit --amend
  ```

  This command allows you to amend the last commit by adding new changes or modifying the commit message.

### 4. **Reverting Commits**

- **Revert a Commit**
  ```bash
  git revert <commit-hash>
  ```

  This command creates a new commit that undoes the changes introduced by a specific commit.

### 5. **Undoing Remote Changes**

- **Force Push to Remote (use with caution)**
  ```bash
  git push --force origin <branch-name>
  ```

  Use this command to force push local changes to a remote branch. **Be cautious** as it rewrites the remote history.

### 6. **Resetting to a Specific Commit**

- **Reset to a Specific Commit**
  ```bash
  git reset --hard <commit-hash>
  ```

  This command resets the branch to a specific commit, discarding all changes after that commit.

### 7. **Cleaning Untracked Files**

- **Remove Untracked Files and Directories**
  ```bash
  git clean -fd
  ```

  This command removes untracked files and directories from the working directory.

### 8. **Interactively Reverting or Editing Commits**

- **Interactive Rebase**
  ```bash
  git rebase -i <commit-hash>
  ```

This command opens an interactive rebase session, allowing you to edit, squash, or drop commits.
The `git rebase -i HEAD~6` command initiates an interactive rebase for the last six commits starting from the current `HEAD`. This allows you to modify, reorder, squash, or drop commits interactively before applying them onto a new base commit.

Let's break down the components of this command:

- `git rebase`: The main command for reorganizing or combining commits.
- `-i`: Stands for "interactive," which opens an interactive rebase session.
- `HEAD~6`: Specifies the commit range to rebase. In this case, it's the last six commits from the current `HEAD`.

```bash
git rebase -i HEAD~6
```

This command opens a text editor with a list of the last six commits in your default branch. The list might look something like this:

```bash
pick abc123 Commit message 1
pick def456 Commit message 2
pick 789ghi Commit message 3
pick jkl012 Commit message 4
pick mno345 Commit message 5
pick pqr678 Commit message 6
```

In the interactive rebase editor, you can choose actions for each commit:

- `pick`: Keep the commit as is.
- `reword`: Change the commit message.
- `edit`: Pause for amending the commit.
- `squash` or `fixup`: Combine the commit with the previous one.
- `drop`: Remove the commit.

For example, to squash the last three commits into a single commit, you can modify the file to look like this:

```bash
pick abc123 Commit message 1
pick def456 Commit message 2
squash 789ghi Commit message 3
squash jkl012 Commit message 4
squash mno345 Commit message 5
pick pqr678 Commit message 6
```

After saving and closing the file, Git will prompt you to modify the commit message for the new squashed commit.

- Interactive rebasing allows you to create a cleaner and more organized commit history.
- Use `edit` to pause at a specific commit and make changes (e.g., amend, add files, or reword the commit message).
- Always make sure to review and understand the changes you're making during an interactive rebase, as it rewrites commit history.

Interactive rebasing is a powerful but potentially risky operation. Be cautious and ensure you have a backup or a way to recover your changes if needed.

### 9. **Restoring Files**
The `git restore` command is a versatile command introduced in Git version 2.23. It is designed to restore parts of the working directory or discard changes in a way that is more explicit and flexible than some of the previous commands.

1. **Restore Uncommitted Changes in a File:**

    To discard uncommitted changes in a specific file and restore it to the state in the last commit:

    ```bash
    git restore file.txt
    ```

    This command reverts the changes made to `file.txt` in the working directory, making it identical to the state of the file in the last commit.

2. **Restore Staged Changes in a File:**

    To unstage changes in a file and move them back to the working directory:

    ```bash
    git restore --staged file.txt
    ```

    This command effectively undoes the staging of changes in `file.txt` and puts them back into the working directory. It is similar to `git reset file.txt`, but `git restore` is more explicit in this context.

3. **Restore Specific Commit's State for a File:**

    To restore a specific file to the state it had in a particular commit:

    ```bash
    git restore --source=<commit-hash> --staged --worktree file.txt
    ```

    - `--source=<commit-hash>`: Specifies the commit from which to take the file state.
    - `--staged`: Restores the file to the specified commit's state in the staging area.
    - `--worktree`: Restores the file to the specified commit's state in the working directory.

4. **Restore Entire Working Directory:**

    To discard all uncommitted changes and restore the entire working directory to the state of the last commit:

    ```bash
    git restore --source=HEAD --staged --worktree --source=HEAD --recursive
    ```

    - `--source=HEAD`: Specifies the commit from which to take the file states.
    - `--staged`: Restores all changes to the staging area.
    - `--worktree`: Restores all changes to the working directory.
    - `--recursive`: Applies the operation recursively to all files and directories.

    The `git restore` command provides a clearer and more explicit syntax for specific use cases, making it a powerful tool for managing changes in the working directory and staging area. Always use it with care, especially when dealing with commands that modify or discard changes.

5. **Comparing `git reset` and `git restore`**

    `git reset` and `git restore` are both Git commands that deal with modifying the working directory, staging area, and commit history, but they serve slightly different purposes. Let's compare them in terms of functionality and use cases:

    + `git reset` 
        - Purpose
            - Primarily used for resetting the branch pointer, staging area, and working directory to a specific commit.
            - Can be used to undo commits, unstage changes, and discard changes in the working directory.

        + Key Options

            - `--soft`: Keeps changes in the working directory and staging area.
            - `--mixed` (default): Unstages changes but keeps them in the working directory.
            - `--hard`: Discards changes in the working directory, staging area, and the commit itself.

        + Examples

            - Undo the last commit and keep changes in the working directory:
            ```bash
            git reset --soft HEAD^
            ```

            - Unstage changes and keep them in the working directory:
            ```bash
            git reset file.txt
            ```

    + `git restore`

        - Purpose

            - Introduced in Git 2.23 as a more explicit command for restoring parts of the working directory.
            - Designed for restoring files, either discarding changes or moving them between the working directory and staging area.

        - Key Options

            - `--source=<commit>`: Specifies the commit from which to take the file state.
            - `--staged`: Restores the file to the specified commit's state in the staging area.
            - `--worktree`: Restores the file to the specified commit's state in the working directory.

        - Examples

            - Discard uncommitted changes in a file:
            ```bash
            git restore file.txt
            ```

            - Unstage changes in a file and move them back to the working directory:
            ```bash
            git restore --staged file.txt
            ```

    - **Use Cases:**
        - `git reset` is often used for branch-related operations and has more options for handling staged and unstaged changes.
        - `git restore` is tailored specifically for restoring parts of the working directory and is focused on clarity.
        - `git reset` can be used to amend or undo commits, while `git restore` focuses more on file-level restoration.

    Ultimately, the choice between `git reset` and `git restore` depends on the specific use case and the level of clarity you seek in your Git commands.