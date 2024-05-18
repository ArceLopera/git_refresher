`git rerere` (reuse recorded resolution) is a useful tool for handling repeated merge conflicts in Git. When enabled, Git records how you resolve a merge conflict and automatically applies the same resolution if the same conflict occurs again in the future. This can be a significant time saver when working on long-lived branches or frequently rebasing/merging. This same tactic can be used if you want to keep a branch rebased so you don’t have to deal with the same rebasing conflicts each time you do it. Or if you want to take a branch that you merged and fixed a bunch of conflicts and then decide to rebase it instead — you likely won’t have to do all the same conflicts again.

### Enabling `git rerere`

Before you can use `git rerere`, you need to enable it. You can do this globally or per repository:

```bash
# Enable rerere globally
git config --global rerere.enabled true

# Enable rerere for a specific repository
git config rerere.enabled true
```
You can also turn it on by creating the .git/rr-cache directory in a specific repository, but the config setting is clearer and enables that feature globally for you.

### Basic Usage

#### **Setup a Scenario for `git rerere`**

Let's set up a scenario where `git rerere` can be useful. Consider two branches that both modify the same line in a file:

```bash
# Create a new repository
mkdir rerere-example
cd rerere-example
git init

# Create a file and commit it
echo "Line 1" > file.txt
git add file.txt
git commit -m "Initial commit"

# Create a new branch and modify the file
git checkout -b branch1
echo "Line 1 - branch1" > file.txt
git commit -am "Change in branch1"

# Switch back to master and create another branch with a conflicting change
git checkout master
git checkout -b branch2
echo "Line 1 - branch2" > file.txt
git commit -am "Change in branch2"
```

#### **Merge with Conflict**

Merge `branch1` into `branch2`, causing a conflict:

```bash
git checkout branch2
git merge branch1
```
When rerere is enabled, the merge will output an additional line in the log:

```bash
Recorded preimage for FILE
``` 
 Otherwise it should look exactly like a normal merge conflict. At this point, rerere can tell us a few things. Normally, you might run git status at this point to see what all conflicted. However, git rerere will also tell you what it has recorded the pre-merge state for with git rerere status.

```bash
git rerere status
```
And git rerere diff will show the current state of the resolution — what you started with to resolve and what you’ve resolved it to.

```bash
git rerere diff
```

Also (and this isn’t really related to rerere), you can use git ls-files -u to see the conflicted files and the before, left and right versions of the file.

```bash
git ls-files -u
```

Resolve the conflict manually:

```bash
# Edit file.txt to resolve the conflict
# For example, make the file content: "Line 1 - resolved"
echo "Line 1 - resolved" > file.txt
git add file.txt
git commit -m "Resolve conflict between branch1 and branch2"
```
You can see that it "Recorded resolution for FILE". You can run git rerere diff again to see what rerere will remember.

#### **Reuse Recorded Resolution**

Now, let's create another conflict scenario to see `git rerere` in action:

```bash
# Create a new conflicting branch
git checkout master
git checkout -b branch3
echo "Line 1 - branch3" > file.txt
git commit -am "Change in branch3"

# Merge branch1 into branch3, causing the same conflict
git merge branch1
```

At this point, `git rerere` automatically recognizes the conflict and reuses the previously recorded resolution:

```bash
# Git should have automatically resolved the conflict
# Check the resolution
cat file.txt

# The content should be: "Line 1 - resolved"
```
There should be in the output a line, Resolved FILE using previous resolution. Also, git diff will show you how it was automatically re-resolved. You can also recreate the conflicted file state with git checkout.

```bash
git checkout --conflict=merge file.txt
```

#### **Finalize the Merge**

Finalize the merge with the reused resolution:

```bash
git add file.txt
git commit -m 
# Automatically resolve conflict between branch1 and branch3 using rerere.
```

### Viewing and Managing `git rerere` Data

You can view recorded resolutions and manage them:

```bash
# List conflicts that have been recorded by rerere
ls .git/rr-cache

# Clean up old recorded resolutions
git rerere gc
```

### Summary

`git rerere` is an advanced Git tool that records conflict resolutions and reuses them in future conflicts, saving time and effort when dealing with repeated conflicts. It is particularly useful in workflows involving frequent rebasing or merging, where the same conflicts might appear multiple times. By enabling `rerere` and leveraging its capabilities, you can streamline your conflict resolution process significantly.