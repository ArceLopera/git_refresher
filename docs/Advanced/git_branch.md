Branching in Git is a powerful feature that allows developers to diverge from the main line of development (usually referred to as the `master` branch) and work on separate, isolated lines of development. Each branch represents an independent line of development, enabling multiple features, fixes, or experiments to be worked on simultaneously without interfering with each other. Branching can be a crucial part of software development, as it allows teams to work collaboratively on different features or fixes simultaneously while maintaining a clear and organized codebase. Isolating work in branches, changing context, and integrating changes into the main line of development can help streamline the development process and improve collaboration.

## **What are Branches?**

A branch in Git is simply a lightweight movable pointer to one commit. The default branch name in Git is master. As you start making commits, you’re given a master branch that points to the last commit you made. Every time you commit, the master branch pointer moves forward automatically. The pointer HEAD tells you where you are in the repository. When you switch branches, HEAD moves to the tip of the new branch.

![](../Images/branchistory.png)

## **Main Branches**

- **Master Branch:** By convention, the `master` branch is often used to represent the main line of development. It typically contains stable, production-ready code.

- **Main Branch (Post-Git 2.28):** Some projects have adopted renaming the default branch from `master` to `main`. The `main` branch serves the same purpose as the `master` branch, but the naming convention aims to promote more inclusive language.

## **Creating Branches**

- **Creating a New Branch:** To create a new branch, use the `git branch <branch-name>` command. Optionally, you can switch to the new branch immediately with `git checkout -b <branch-name>` or `git switch -c <branch-name>`.

![](../Images/branch1.png)

  ```bash
  git branch feature-branch
  ```

![](../Images/branchistory.png)


- **Switching Branches:** To switch between branches, use the `git checkout <branch-name>` command or `git switch <branch-name>`.

### 1. **`git checkout`**

  ```bash
  git checkout feature-branch
  ```
![](../Images/branch2.png)

### 2. **`git switch`**
 Introduced in Git version 2.23, `git switch` is specifically designed for branch switching. It offers a more intuitive and safer way to switch branches compared to `git checkout`.
  
  ```bash
  git switch <branch-name>
  ```

- Both `git switch` and `git checkout` are used for switching branches in Git.
- `git switch` is specifically designed for branch switching and offers a safer and more intuitive experience compared to `git checkout`.
- While `git checkout` remains a versatile command for various Git operations, `git switch` is recommended for branch switching to promote consistency and safety in your workflow.

 git checkout is a multi-purpose tool, capable of switching branches, discarding changes, and even creating new branches. Conversely, git switch is more specialized, focusing solely on switching and creating branches.

## **Viewing Branches**

- **Listing Branches:** To list all branches in the repository, use the `git branch` command.

  ```bash
  git branch
  ```
  An asterisk (`*`) in the output indicates that the branch you are currently on is the currently active branch.

The `git branch -a` command is used to list all branches in a Git repository, including both local branches and remote branches.

```bash
git branch -a
```

- **`git branch`:** This is the Git command used to manage branches in a repository.

- **`-a` (or `--all`):** This option tells Git to list both local branches and remote branches.

When you run `git branch -a`, Git will list all branches in the repository, including:

- **Local Branches:** Branches that exist only in your local repository.
- **Remote Branches:** Branches that exist on the remote repository (e.g., on GitHub, GitLab).
- **Remote Tracking Branches:** Local representations of remote branches, used for tracking changes from the remote repository.

The output typically looks like this:

```plaintext
* main
  feature-branch
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/feature-branch
```

- The branches listed without the `remotes/` prefix are local branches.
- The branches listed with the `remotes/` prefix are remote branches.
- `origin` is the default name for the remote repository, but you may see other names if you have multiple remotes configured.

### Use Cases

- Checking the status of local and remote branches.
- Identifying available branches for merging, rebasing, or switching.
- Tracking changes from remote branches and syncing local branches accordingly.

By using `git branch -a`, you can get a comprehensive overview of all branches in your Git repository, allowing you to manage and navigate through branches effectively.

## **Merging Branches**

Let's do another commit on the `feature-branch` branch. 

![](../Images/branch3.png)

We will want eventually to merge this commit into the `main` branch. However, other collaborators are working on the other features (hotfixes) and create another branch for a hotfix.

### Fast-forward merges

To merge changes from one branch into another, use the `git merge <branch-name>` command.

However you have to first checkout to the branch you want to merge into.

```bash
git checkout master
```
![](../Images/branch4.png)

```bash
git merge hotfix-branch
```

![](../Images/branch5.png)

You’ll notice the phrase “fast-forward” in that merge. Because the commit pointed to by the branch hotfix (d45a6) you merged in was directly ahead of the commit (fe456) you’re on, Git simply moves the pointer forward.  Git simplifies things by moving the pointer forward because there is no divergent work to merge together — this is called a “fast-forward”.



### **Deleting Branches**

Now, that the hotfix branch is no longer needed, we can delete it.
To delete a branch, use the `git branch -d <branch-name>` command.

```bash
git branch -d hotfix-branch
```

![](../Images/branch6.png)

### Recursive merge

Now we can switch back to our work-in-progress branch in order to continue working on our project.

```bash
git checkout feature-branch
```
And do more commits on the feature branch.

![](../Images/branch7.png)

As you can see, now there is a divergent work to merge. Because the commit on the branch you’re on isn’t a direct ancestor of the branch you’re merging in. To merge merge changes into the master branch, we must first switch to the master branch. then we can merge the feature branch into the master branch.


```bash
git checkout master
```

then 

```bash
git merge feature-branch
```
You will notice the info: Merge made by the 'recursive' strategy.  Git does a simple three-way merge, using the two snapshots pointed to by the branch tips and the common ancestor of the two. Instead of just moving the branch pointer forward, Git creates a new snapshot that results from this three-way merge and automatically creates a new commit that points to it. This is referred to as a merge commit, and is special in that it has more than one parent.

![](../Images/branch8.png)

Now that our work is merged in, we have no further need for the feature branch. We can close the issue in the issue-tracking system, and delete the branch:

```bash
git branch -d feature-branch
```

### Merging conflicts

 If you changed the same part of the same file differently in the two branches you’re merging, Git won’t be able to merge them cleanly. In this case, you’ll see a merge conflict in the output, and Git will tell you which files you need to resolve the conflict with. Git hasn’t automatically created a new merge commit. It has paused the process while you resolve the conflict. If you want to see which files are unmerged at any point after a merge conflict, you can run git status.

 This resolution has a little of each section, and the <<<<<<<, =======, and >>>>>>> lines have been completely removed. After you’ve resolved each of these sections in each conflicted file, run git add on each file to mark it as resolved. Staging the file marks it as resolved in Git.

 If you want to use a graphical tool to resolve these issues, you can run git mergetool, which fires up an appropriate visual merge tool and walks you through the conflicts. After you exit the merge tool, Git asks you if the merge was successful. If you tell the script that it was, it stages the file to mark it as resolved for you. You can run git status again to verify that all conflicts have been resolved.
If you’re happy with that, and you verify that everything that had conflicts has been staged, you can type git commit to finalize the merge commit. 



```bash
git status
```
Anything that has merge conflicts and hasn’t been resolved is listed as unmerged. Git adds standard conflict-resolution markers to the files that have conflicts, so you can open them manually and resolve those conflicts.

## **Branching Strategies**

Git encourages workflows that branch and merge often, even multiple times in a day. Understanding and mastering this feature gives you a powerful and unique tool and can entirely change the way that you develop. The way Git branches is incredibly lightweight, making branching operations nearly instantaneous, and switching back and forth between branches generally just as fast.

- **Feature Branching:** Create a separate branch for each new feature or task. This isolates changes related to specific features, making them easier to manage and review.

- **Release Branching:** Create branches for each release to stabilize the codebase before deployment.

- **Hotfix Branching:** Create branches to fix critical issues in production code quickly.

## **Remote Branches**

- **Pushing a Branch:** To push a local branch to a remote repository, use the `git push <remote-name> <branch-name>` command.

  ```bash
  git push origin feature-branch
  ```

- **Tracking Remote Branches:** After pushing a branch to the remote repository, it can be tracked. This means Git remembers the relationship between the local branch and its corresponding remote branch.

  ```bash
  git checkout -b feature-branch origin/feature-branch
  ```

  The `git branch --set-upstream-to` command is used to set up the tracking relationship between a local branch and a remote branch. This command tells Git which remote branch the local branch should track, allowing you to push and pull changes to and from the correct remote branch without specifying it each time. 

```bash
git branch --set-upstream-to=<remote>/<branch>
```

- `<remote>`: The name of the remote repository.
- `<branch>`: The name of the remote branch.


```bash
git branch --set-upstream-to=origin/main
```

This command sets the tracking relationship for the current branch to the specified remote branch (`main` in this example) in the `origin` remote repository.
After running this command, Git knows that when you push or pull changes from the current branch, it should interact with the `main` branch in the `origin` remote repository.

**Syncing Changes** After setting up tracking, you can simply use `git push` and `git pull` without specifying the remote branch, as Git already knows where to push and pull changes from.

- This command can also be used in combination with `git push -u` or `git push --set-upstream` to set up tracking and push changes to the remote branch in one step.

## **Branching Best Practices**

- **Use Descriptive Names:** Choose meaningful names for branches to indicate their purpose or associated feature.

- **Keep Branches Short-Lived:** Merge branches into the main line of development once their purpose is served to avoid branch clutter.

- **Regularly Update Branches:** Keep branches up-to-date with changes in the main line of development by frequently merging or rebasing.

- **Review Branches:** Encourage code review and collaboration by reviewing changes made in feature branches before merging them.