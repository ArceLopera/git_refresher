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

Perhaps we’re not happy with the resolution at this point for some reason, or maybe manually editing one or both sides still didn’t work well and we need more context. For example, imagine we have two longer lived branches that each have a few commits in them but create a legitimate content conflict when merged.

```bash
$ git log --graph --oneline --decorate --all
* f1270f7 (HEAD, master) Update README
* 9af9d3b Create README
* 694971d Update phrase to 'hola world'
| * e3eb223 (mundo) Add more tests
| * 7cff591 Create initial testing script
| * c3ffff1 Change text to 'hello mundo'
|/
* b7dcc89 Initial hello world code
```

We now have three unique commits that live only on the master branch and three others that live on the mundo branch. If we try to merge the mundo branch in, we get a conflict.

```bash
$ git merge mundo
Auto-merging hello.rb
CONFLICT (content): Merge conflict in hello.rb
Automatic merge failed; fix conflicts and then commit the result.
```
We would like to see what the merge conflict is. If we open up the file, we’ll see something like this:

```bash
#! /usr/bin/env ruby

def hello
<<<<<<< HEAD
  puts 'hola world'
=======
  puts 'hello mundo'
>>>>>>> mundo
end

hello()
```
Both sides of the merge added content to this file, but some of the commits modified the file in the same place that caused this conflict.

One helpful tool is git checkout with the --conflict option. This will re-checkout the file again and replace the merge conflict markers. This can be useful if you want to reset the markers and try to resolve them again.

```bash
git checkout --conflict <file>
```

You can pass --conflict either diff3 or merge (which is the default). If you pass it diff3, Git will use a slightly different version of conflict markers, not only giving you the “ours” and “theirs” versions, but also the “base” version inline to give you more context.

```bash
git checkout --conflict=diff3 hello.rb
```
Once we run that, the file will look like this instead:

```bash
#! /usr/bin/env ruby

def hello
<<<<<<< ours
  puts 'hola world'
||||||| base
  puts 'hello world'
=======
  puts 'hello mundo'
>>>>>>> theirs
end

hello()
```
If you like this format, you can set it as the default for future merge conflicts by setting the merge.conflictstyle setting to diff3.

```bash
git config --global merge.conflictstyle diff3
```

The git checkout command can also take --ours and --theirs options, which can be a really fast way of just choosing either one side or the other without merging things at all.

This can be particularly useful for conflicts of binary files where you can simply choose one side, or where you only want to merge certain files in from another branch — you can do the merge and then checkout certain files from one side or the other before committing.

### Merge Log
AQnother useful tool when resolving merge conflicts is git log. This can help you get context on what may have contributed to the conflicts. Reviewing a little bit of history to remember why two lines of development were touching the same area of code can be really helpful sometimes.

To get a full list of all of the unique commits that were included in either branch involved in this merge, we can use the “triple dot”.

```bash
git log --oneline --left-right HEAD...MERGE_HEAD
```
We can further simplify this though to give us much more specific context. If we add the --merge option to git log, it will only show the commits in either side of the merge that touch a file that’s currently conflicted.

```bash
git log --oneline --left-right --merge
```
If you run that with the -p option instead, you get just the diffs to the file that ended up in conflict. This can be really helpful in quickly giving you the context you need to help understand why something conflicts and how to more intelligently resolve it.


### Combined Diff Format
Since Git stages any merge results that are successful, when you run git diff while in a conflicted merge state, you only get what is currently still in conflict. This can be helpful to see what you still have to resolve.

When you run git diff directly after a merge conflict, it will give you information in a rather unique diff output format.

```bash
git diff
```

```
diff --cc hello.rb
index 0399cd5,59727f0..0000000
--- a/hello.rb
+++ b/hello.rb
@@@ -1,7 -1,7 +1,11 @@@
  #! /usr/bin/env ruby

  def hello
++<<<<<<< HEAD
 +  puts 'hola world'
++=======
+   puts 'hello mundo'
++>>>>>>> mundo
  end

  hello()
```
The format is called “Combined Diff” and gives you two columns of data next to each line. The first column shows you if that line is different (added or removed) between the “ours” branch and the file in your working directory and the second column does the same between the “theirs” branch and your working directory copy.

So in that example you can see that the <<<<<<< and >>>>>>> lines are in the working copy but were not in either side of the merge. This makes sense because the merge tool stuck them in there for our context, but we’re expected to remove them.

If we resolve the conflict and run git diff again, we’ll see the same thing, but it’s a little more useful.

```bash
vim hello.rb
git diff
```
```
diff --cc hello.rb
index 0399cd5,59727f0..0000000
--- a/hello.rb
+++ b/hello.rb
@@@ -1,7 -1,7 +1,7 @@@
  #! /usr/bin/env ruby

  def hello
-   puts 'hola world'
 -  puts 'hello mundo'
++  puts 'hola mundo'
  end

  hello()
```

This shows us that “hola world” was in our side but not in the working copy, that “hello mundo” was in their side but not in the working copy and finally that “hola mundo” was not in either side but is now in the working copy. This can be useful to review before committing the resolution.

You can also get this from the git log for any merge to see how something was resolved after the fact. Git will output this format if you run git show on a merge commit, or if you add a --cc option to a git log -p (which by default only shows patches for non-merge commits).

```bash
git log --cc -p -1
```


### Undoing Merges

Now that you know how to create a merge commit, you’ll probably make some by mistake. One of the great things about working with Git is that it’s okay to make mistakes, because it’s possible (and in many cases easy) to fix them.

There are two ways to approach this problem, depending on what your desired outcome is.

#### Fix the References

If the unwanted merge commit only exists on your local repository, the easiest and best solution is to move the branches so that they point where you want them to. In most cases, if you follow the errant git merge with git reset --hard HEAD~, this will reset the branch pointers to what they were before the merge. To undo a merge commit and reset the branch to its state before the merge, you can use `git reset --hard`.

```bash
git reset --hard HEAD^
```

`reset --hard` usually goes through three steps:

+ Move the branch HEAD points to. In this case, we want to move master to where it was before the merge commit (C6).

+ Make the index look like HEAD.

+ Make the working directory look like the index.

The downside of this approach is that it’s rewriting history, which can be problematic with a shared repository. This approach also won’t work if any other commits have been created since the merge; moving the refs would effectively lose those changes.

#### Reverse the Commit
If moving the branch pointers around isn’t going to work for you, Git gives you the option of making a new commit which undoes all the changes from an existing one. Git calls this operation a “revert”. To reverse a merge commit, you can use `git revert`.

```bash
git revert -m 1 <merge-commit>
```

```bash
git revert -m 1 HEAD
```

### Other types of Merges
So far we’ve covered the normal merge of two branches, normally handled with what is called the “recursive” strategy of merging. There are other ways to merge branches together however.

#### Our or Theirs Preference

During a merge, you can specify whether to prefer changes from the current branch (`--ours`) or the merged branch (`--theirs`). By default, when Git sees a conflict between two branches being merged, it will add merge conflict markers into your code and mark the file as conflicted and let you resolve it. If you would prefer for Git to simply choose a specific side and ignore the other side instead of letting you manually resolve the conflict, you can pass the merge command either a -Xours or -Xtheirs.

If Git sees this, it will not add conflict markers. Any differences that are mergeable, it will merge. Any differences that conflict, it will simply choose the side you specify in whole, including binary files.

```bash
git merge -Xours feature-branch
git merge -Xtheirs feature-branch
```

This option can also be passed to the git merge-file command we saw earlier by running something like git merge-file --ours for individual file merges.

If you want to do something like this but not have Git even try to merge changes from the other side in, there is a more draconian option, which is the “ours” merge strategy. This is different from the “ours” recursive merge option.

This will basically do a fake merge. It will record a new merge commit with both branches as parents, but it will not even look at the branch you’re merging in. It will simply record as the result of the merge the exact code in your current branch.
```	bash
git merge -s ours mundo
Merge made by the 'ours' strategy.
git diff HEAD HEAD~
```

You can see that there is no difference between the branch we were on and the result of the merge.

This can often be useful to basically trick Git into thinking that a branch is already merged when doing a merge later on. For example, say you branched off a release branch and have done some work on it that you will want to merge back into your master branch at some point. In the meantime some bugfix on master needs to be backported into your release branch. You can merge the bugfix branch into the release branch and also merge -s ours the same branch into your master branch (even though the fix is already there) so when you later merge the release branch again, there are no conflicts from the bugfix.

#### Subtree Merging

The idea of the subtree merge is that you have two projects, and one of the projects maps to a subdirectory of the other one. When you specify a subtree merge, Git is often smart enough to figure out that one is a subtree of the other and merge appropriately.

An example of this will be adding a separate project into an existing project and then merging the code of the second into a subdirectory of the first.

So, we’ll add the Rack application to our project. We’ll add the Rack project as a remote reference in our own project and then check it out into its own branch:

```bash
git remote add rack_remote https://github.com/rack/rack
git fetch rack_remote --no-tags
git checkout -b rack_branch rack_remote/master
```
Now we have the root of the Rack project in our rack_branch branch and our own project in the master branch. If you check out one and then the other, you can see that they have different project roots.

In this case, we want to pull the Rack project into our master project as a subdirectory. We can do that in Git with git read-tree. `read-tree` reads the root tree of one branch into your current staging area and working directory. We just switched back to your master branch, and we pull the rack_branch branch into the rack subdirectory of our master branch of our main project.

```bash
git read-tree --prefix=rack/ -u rack_branch
```

When we commit, it looks like we have all the Rack files under that subdirectory — as though we copied them in from a tarball. What gets interesting is that we can fairly easily merge changes from one of the branches to the other. So, if the Rack project updates, we can pull in upstream changes by switching to that branch and pulling it in.

```bash
git checkout rack_branch
git pull
```

Then, we can merge those changes back into our master branch. To pull in the changes and prepopulate the commit message, use the `--squash` option, as well as the recursive merge strategy’s `-Xsubtree` option. The recursive strategy is the default here, but we include it for clarity.

```bash
git checkout master
git merge --squash -s recursive -Xsubtree=rack rack_branch
```

All the changes from the Rack project are merged in and ready to be committed locally. You can also do the opposite — make changes in the rack subdirectory of your master branch and then merge them into your rack_branch branch later to submit them to the maintainers or push them upstream.

This gives us a way to have a workflow somewhat similar to the submodule workflow without using submodules. We can keep branches with other related projects in our repository and subtree merge them into our project occasionally. It is nice in some ways, for example all the code is committed to a single place. However, it has other drawbacks in that it’s a bit more complex and easier to make mistakes in reintegrating changes or accidentally pushing a branch into an unrelated repository.

Another slightly weird thing is that to get a diff between what you have in your rack subdirectory and the code in your rack_branch branch — to see if you need to merge them — you can’t use the normal diff command. Instead, you must run git diff-tree with the branch you want to compare to.

```bash
git diff-tree -p rack_branch
```

Or, to compare what is in your rack subdirectory with what the master branch on the server was the last time you fetched, you can run:

```bash
git diff-tree -p rack_remote/master
```