### Merge Conflicts
Merge conflicts occur when Git is unable to automatically resolve differences between branches. You can resolve conflicts manually by editing the conflicting files and then committing the changes. Git’s philosophy is to be smart about determining when a merge resolution is unambiguous, but if there is a conflict, it does not try to be clever about automatically resolving it. Therefore, if you wait too long to merge two branches that diverge quickly, you can run into some issues.

```bash
git merge feature-branch
# Resolve conflicts in conflicted files
git add <resolved-file>
git commit
```

First of all, if at all possible, try to make sure your working directory is clean before doing a merge that may have conflicts. If you have work in progress, either commit it to a temporary branch or stash it. This makes it so that you can undo anything you try here. If you have unsaved changes in your working directory when you try a merge, some of these tips may help you preserve that work.


### Aborting a Merge
If you encounter issues during a merge and want to abort the process, you can use:

```bash
git merge --abort
```
The `--abort` option tries to revert back to your state before you ran the merge. The only cases where it may not be able to do this perfectly would be if you had unstashed, uncommitted changes in your working directory when you ran it, otherwise it should work fine.

If for some reason you just want to start over, you can also run `git reset --hard HEAD`, and your repository will be back to the last committed state. Remember that any uncommitted work will be lost, so make sure you don’t want any of your changes.

### Ignoring Whitespace
To ignore whitespace changes during a merge, you can use the `-Xignore-space-change` or `-Xignore-all-space` option. If you see that you have a lot of whitespace issues in a merge, you can simply abort it and do it again, this time with -Xignore-all-space or -Xignore-space-change. The first option ignores whitespace completely when comparing lines, the second treats sequences of one or more whitespace characters as equivalent.

```bash
git merge -Xignore-space-change feature-branch
```

This can be identified when looking at the conflict because every line is removed on one side and added again on the other. By default, Git sees all of these lines as being changed, so it can’t merge the files.

This is a lifesaver if you have someone on your team who likes to occasionally reformat everything from spaces to tabs or vice-versa.

### Manual File Re-merging


Accessing the three versions of a file in a conflict can be useful for debugging or troubleshooting. Suppose we have three versions of the hello.rb file: hello.common.rb (common ancestor), hello.ours.rb (current version), and hello.theirs.rb (version to be merged).

```bash
git show :1:hello.rb > hello.common.rb
git show :2:hello.rb > hello.ours.rb
git show :3:hello.rb > hello.theirs.rb
```
Note: The :1:hello.rb is just a shorthand for looking up that blob SHA-1.

The `git merge-file` command is used to merge changes from three different versions of a file: a common ancestor, the current version, and the version to be merged. This command is particularly useful when resolving merge conflicts manually or when automating the merge process in scripts. 

```bash
git merge-file [-p | --stdout] <current-file> <base-file> <other-file>
```

- **`-p` or `--stdout`**: Optional flag to output the merged content to standard output instead of modifying files directly.
- **`<current-file>`**: The file as it exists in the current branch.
- **`<base-file>`**: The common ancestor file before any changes were made.
- **`<other-file>`**: The file to be merged into the current branch.

**Merging Changes from Three Versions**
   ```bash
   git merge-file -p hello.ours.rb hello.common.rb hello.theirs.rb > hello.rb
   ```
   This command merges the changes from `hello.common.rb` (common ancestor), `hello.ours.rb` (current version), and `hello.theirs.rb` (version to be merged) and outputs the merged content to `hello.rb`.

- **Manual Conflict Resolution**: Use `git merge-file` to resolve merge conflicts manually by inspecting and editing the merged content before committing.
- **Automated Merge Process**: Integrate `git merge-file` into scripts or automated processes to perform merges without manual intervention, such as during continuous integration workflows.

To compare your result to what you had in your branch before the merge, in other words, to see what the merge introduced, you can run `git diff --ours`. If we want to see how the result of the merge differed from what was on their side, you can run `git diff --theirs`. Finally, you can see how the file has changed from both sides with `git diff --base`.

### Checking Out Conflicts
To view the conflicting changes in a file, you can use `git checkout --conflict`.

```bash
git checkout --conflict <file>
```

### Merge Log
To view the history of merges in your repository, you can use `git log --merges`.

```bash
git log --merges
```

### Combined Diff Format
The combined diff format shows changes from multiple branches in a single diff output.

```bash
git diff --combined
```

### Undoing Merges
To undo a merge commit and reset the branch to its state before the merge, you can use `git reset --hard`.

```bash
git reset --hard HEAD^
```

### Fix the References
If you need to fix the references after a failed merge, you can manually update the references in the `.git/refs` directory.

### Reverse the Commit
To reverse a merge commit, you can use `git revert`.

```bash
git revert -m 1 <merge-commit>
```

### Our or Theirs Preference
During a merge, you can specify whether to prefer changes from the current branch (`--ours`) or the merged branch (`--theirs`).

```bash
git merge -Xours feature-branch
git merge -Xtheirs feature-branch
```

### Subtree Merging
Subtree merging involves merging in changes from one branch into a subdirectory of another branch.

```bash
git merge --squash --no-commit feature-branch
git read-tree --prefix=subdir/ -u feature-branch
git commit -m "Merge feature-branch into subdir"
```

These advanced merging techniques provide you with greater control and flexibility when managing merges in your Git repository, allowing you to handle complex merging scenarios efficiently.