In Git, temporary commits, also known as "stashing," allow you to save your work temporarily without committing it to the repository. Stashing is useful when you need to switch to another task or branch but want to keep your current changes for later use. 

## **Stashing Changes**

```bash
git stash
```
- This command saves your current changes in a "stash" without committing them to the repository.
- Git removes the changes from the working directory and staging area, allowing you to switch branches or perform other operations.

## **Listing Stashes**

```bash
git stash list
```

- This command lists all stashes you have saved.
- Each stash is identified by a unique identifier, typically a number.

## **Applying Stashed Changes**

```bash
git stash apply [<stash>]
```

```bash
git stash apply stash@{0}
```

- This command applies the changes from the specified stash to the current working directory.
- If no stash is specified, Git applies the most recent stash by default.

## **Popping Stashed Changes**

```bash
git stash pop [<stash>]
```

```bash
git stash pop stash@{0}
```

- This command applies the changes from the specified stash and removes it from the stash list.
- If no stash is specified, Git pops the most recent stash by default.

## **Droping Stashed Changes**

The `git stash drop` command is used to remove stashed changes from the stash list.

```bash
git stash drop [<stash>]
```
- `<stash>`: Optional. Specifies the stash to drop. If not provided, Git drops the most recent stash by default.

```bash
git stash drop stash@{0}
```

- This command removes the specified stash from the stash list.
- If no stash is specified, Git drops the most recent stash by default.
- Once dropped, the changes stored in the stash are permanently deleted and cannot be recovered.

- **Cleanup:** Use `git stash drop` to remove unwanted or obsolete stashes from the stash list, freeing up space and decluttering your repository.

- **Permanent Deletion:** Be cautious when using `git stash drop`, as it permanently deletes the specified stash and its associated changes. Ensure that you no longer need the changes before dropping the stash.

## **Clearing Stashes**

```bash
git stash clear
```

- This command removes all stashes from the stash list.
- Use with caution, as it permanently deletes all stashed changes.



### Use Cases
- **Switching Tasks:** Stash changes when you need to switch tasks or work on a different branch temporarily.
- **Experimentation:** Stash changes before experimenting with code changes, allowing you to revert easily if needed.
- **Conflict Resolution:** Stash changes when resolving conflicts during a merge or rebase operation.


Stashing changes in Git provides a convenient way to save your work temporarily without committing it to the repository. By stashing changes, you can switch tasks, experiment with code changes, or resolve conflicts with confidence, knowing that your work is safely stored and can be easily reapplied when needed. Stashing is a valuable tool for managing your workflow effectively in Git.