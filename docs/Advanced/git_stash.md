In Git, temporary commits, also known as "stashing," allow you to save your work temporarily without committing it to the repository. Stashing is useful when you need to switch to another task or branch but want to keep your current changes for later use. 

## **Stashing Changes**

```bash
git stash
```
This command saves your current changes in a "stash" without committing them to the repository. Git removes the changes from the working directory and staging area, allowing you to switch branches or perform other operations.

The `git stash` command can be used to save the changes you have made to the working directory. It can also be used to save the changes you have made to the staging area. Stashing is useful when you need to switch to another task or branch but want to keep your current changes for later use. 

### **--keep-index**

To save the changes in the staging area, use the --keep-index option to the git stash command. This tells Git to not only include all staged content in the stash being created, but simultaneously leave it in the index.

### **--include-untracked**

If you specify --include-untracked or -u, Git will include the untracked files as well as the tracked ones. By default, git stash will stash only modified and staged **tracked** files. If you specify --include-untracked or -u, Git will include untracked files in the stash being created. However, including untracked files in the stash will still not include explicitly ignored files; to additionally include ignored files, use --all (or just -a).

### **--patch**

 If you specify the --patch flag, Git will not stash everything that is modified but will instead prompt you interactively which of the changes you would like to stash and which you would like to keep in your working directory.

### **--all**
Use  git stash --all to remove everything but save it in a stash.



## **Listing Stashes**

```bash
git stash list
```

This command lists all stashes you have saved. Each stash is identified by a unique identifier, typically a number.

## **Applying Stashed Changes**

```bash
git stash apply [<stash>]
```

```bash
git stash apply stash@{0}
```

- This command applies the changes from the specified stash to the current working directory.
- If no stash is specified, Git applies the most recent stash by default.

You can save a stash on one branch, switch to another branch later, and try to reapply the changes. You can also have modified and uncommitted files in your working directory when you apply a stash — Git gives you merge conflicts if anything no longer applies cleanly.

The --index option with git stash apply applies both the changes from the stash and the staged changes (changes that were added to the index) that were present when the changes were stashed. This option is particularly useful when you have staged changes that you also want to apply along with the unstaged changes from the stash.

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

```bash
git stash drop stash@{0}
```

This command removes the specified stash from the stash list. If no stash is specified, Git drops the most recent stash by default.
Once dropped, the changes stored in the stash are permanently deleted and cannot be recovered.

## **Clearing Stashes**

```bash
git stash clear
```

This command removes all stashes from the stash list. Use with caution, as it permanently deletes all stashed changes.

## Creating a Branch from a Stash

If you stash some work, leave it there for a while, and continue on the branch from which you stashed the work, you may have a problem reapplying the work. If the apply tries to modify a file that you’ve since modified, you’ll get a merge conflict and will have to try to resolve it. If you want an easier way to test the stashed changes again, you can run git stash branch <new branchname>, which creates a new branch for you with your selected branch name, checks out the commit you were on when you stashed your work, reapplies your work there, and then drops the stash if it applies successfully.

``` bash
$ git stash branch testchanges
M	index.html
M	lib/simplegit.rb
Switched to a new branch 'testchanges'
On branch testchanges
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   index.html

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   lib/simplegit.rb

Dropped refs/stash@{0} (29d385a81d163dfd45a452a2ce816487a6b8b014)
```
This is a nice shortcut to recover stashed work easily and work on it in a new branch.
