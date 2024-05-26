Git submodules allow you to include and manage external repositories within your own repository. This is particularly useful when you want to incorporate third-party libraries or other dependencies while maintaining control over their specific versions.

### Adding a Submodule

To add a submodule to your repository, use the `git submodule add` command.

```bash
# Syntax
git submodule add <repository-url> [<path>]

# Example
git submodule add https://github.com/example/library.git lib/library
```

This command clones the submodule repository into the specified path and adds a reference to it in your `.gitmodules` file. By default, submodules will add the subproject into a directory named the same as the repository. You can add a different path at the end of the command if you want it to go elsewhere. You can also specify a branch to checkout with the `-b` flag.

```bash
git submodule add -b master git@github.com:emeneo/moodle-enrol_apply.git enrol/apply
```
The new `.gitmodules` file is a configuration file that stores the mapping between the project’s URL and the local subdirectory you’ve pulled it into.

```bash
[submodule "enrol/apply"]
    path = enrol/apply
    url = git@github.com:emeneo/moodle-enrol_apply.git
    branch = master
```
If you have multiple submodules, you’ll have multiple entries in this file. It’s important to note that this file is version-controlled with your other files, like your .gitignore file. It’s pushed and pulled with the rest of your project. This is how other people who clone this project know where to get the submodule projects from.


### Initializing and Updating Submodules

After cloning a repository that contains submodules, you need to initialize and update them.

```bash
# Initialize submodules
git submodule init

# Update submodules
git submodule update

git submodule sync; git submodule update --init --recursive 
```

The `init` command initializes the submodules recorded in the `.gitmodules` file, while the `update` command fetches and checks out the correct commit specified by the superproject.

### Cloning a Repository with Submodules

When cloning a repository that includes submodules, use the `--recurse-submodules` flag to automatically initialize and update submodules. When you clone such a project, by default you get the directories that contain submodules, but none of the files within them yet.  You must run two commands: git submodule init to initialize your local configuration file, and git submodule update to fetch all the data from that project and check out the appropriate commit listed in your superproject. If you pass `--recurse-submodules` to the git clone command, it will automatically initialize and update each submodule in the repository, including nested submodules if any of the submodules in the repository have submodules themselves.

```bash
# Clone repository and initialize submodules
git clone --recurse-submodules <repository-url>

# Example
git clone --recurse-submodules https://github.com/example/main-project.git
```
If you already cloned the project and forgot --recurse-submodules, you can combine the git submodule init and git submodule update steps by running git submodule update --init. To also initialize, fetch and checkout any nested submodules, you can use the foolproof git submodule update --init --recursive.

```bash
git submodule update --init --recursive 
```
There is a special situation that can happen when pulling superproject updates: it could be that the upstream repository has changed the URL of the submodule in the `.gitmodules` file in one of the commits you pull. This can happen for example if the submodule project changes its hosting platform. In that case, it is possible for git pull --recurse-submodules, or git submodule update, to fail if the superproject references a submodule commit that is not found in the submodule remote locally configured in your repository. In order to remedy this situation, the git submodule sync command is required:

```bash
# copy the new URL to your local config
$ git submodule sync --recursive
# update the submodule from the new URL
$ git submodule update --init --recursive
```


### Updating Submodules to the Latest Commit

If you want to update your submodules to their latest commit, use:

```bash
# Update all submodules to the latest commit
git submodule update --remote

# Example
git submodule update --remote lib/library
```

This command updates the submodule to the latest commit on the default branch.

### Removing a Submodule

To remove a submodule, you need to follow several steps:

```bash
git rm <path-to-submodule>
git commit
```
This removes the filetree at 'path-to-submodule', and the submodule's entry in the .gitmodules file. i.e. all traces of the submodule in your repository proper are removed.

####  Remove the submodule directory from the index

The `.git/config` file keeps track of all submodules, their paths, and URLs. Here’s an example entry.

As the docs note however, the .git dir of the submodule is kept around (in the modules/ directory of the main project's .git dir), "to make it possible to checkout past commits without requiring fetching from another repository".
If you nonetheless want to remove this info, manually delete the submodule's directory in .git/modules/, and remove the submodule's entry in the file .git/config. These steps can be automated using the commands

```bash
rm -rf .git/modules/<path-to-submodule>
git config --remove-section submodule.<path-to-submodule>.
```

After performing these steps, commit the changes to complete the removal of the submodule.


### Managing Nested Submodules

Submodules can themselves contain submodules. To initialize and update nested submodules, you need to pass the `--recursive` flag:

```bash
# Initialize nested submodules
git submodule update --init --recursive

# Update nested submodules to the latest commit
git submodule update --remote --recursive
```

### Submodule Configuration in `.gitmodules`

The `.gitmodules` file keeps track of all submodules, their paths, and URLs. Here’s an example entry:

```plaintext
[submodule "lib/library"]
    path = lib/library
    url = https://github.com/example/library.git
```

### Submodule Foreach

The `git submodule foreach` command allows you to run a command on each submodule in your project. It can be used to execute arbitrary commands on each submodule.
For example, let’s say we want to start a new feature or do a bugfix and we have work going on in several submodules. We can easily stash all the work in all our submodules.

```bash
git submodule foreach 'git stash'
```
Then we can create a new branch and switch to it in all our submodules.

```bash
git submodule foreach 'git checkout -b new-branch'
```

One really useful thing you can do is produce a nice unified diff of what is changed in your main project and all your subprojects as well.

```bash
git diff; git submodule foreach 'git diff'
```

### The `--recurse-submodules` flag 


| Command                     | Example Usage                                          |
|-----------------------------|--------------------------------------------------------|
| `git clone`                 | `git clone --recurse-submodules <repository-url>`      |
| `git pull`                  | `git pull --recurse-submodules`                        |
| `git fetch`                 | `git fetch --recurse-submodules`                       |
| `git checkout`              | `git checkout --recurse-submodules <branch-name>`      |
| `git submodule update`      | `git submodule update --recurse-submodules`            |
| `git push`                  | `git push --recurse-submodules=check`                  |
| `git reset`                 | `git reset --recurse-submodules <commit>`              |

Using the `--recurse-submodules` flag ensures that operations affecting the main repository also appropriately handle submodules, keeping everything in sync and properly updated. You can tell Git (>=2.14) to always use the `--recurse-submodules` flag by setting the configuration option submodule.recurse: `git config submodule.recurse true`. This will also make Git recurse into submodules for every command that has a --recurse-submodules option (except git clone).


##### `git clone`
When cloning a repository, this flag ensures that all submodules are also cloned and initialized.

```bash
git clone --recurse-submodules <repository-url>
```
```bash
git clone --recurse-submodules https://github.com/example/repo.git
```

##### `git pull` or `git fetch`
Pulls or Fetches changes from the remote repository and updates submodules as well.

```bash
git pull --recurse-submodules
git fetch --recurse-submodules
```
##### `git checkout`
Checks out a branch or commit and updates the submodules to the correct commit.

```bash
git checkout --recurse-submodules <branch-name>
```
```bash
git checkout --recurse-submodules main
```

##### `git submodule update`
Updates the submodules to the commit specified in the superproject. This flag is used to ensure that nested submodules are also updated.

```bash
git submodule update --recurse-submodules
```

##### `git push`
When pushing to a remote repository, this flag ensures that submodule commits are also pushed.

```bash
git push --recurse-submodules=check
```
Here, the `check` option checks that all submodule commits that the superproject references are present on the remote repository before pushing. The `check` option will make push simply fail if any of the committed submodule changes haven’t been pushed. If you want the “check” behavior to happen for all pushes, you can make this behavior the default by doing `git config push.recurseSubmodules check`.

The `on-demand` option ensures that the submodule commits are pushed even if the superproject commit is not present on the remote repository. Git will go into each submodule and push to the remotes to make sure they’re externally available.  You can make this behavior the default by doing `git config push.recurseSubmodules on-demand`.

##### `git reset`
Resets the current HEAD to a specified state and updates submodules accordingly.

```bash
git reset --recurse-submodules <commit>
```
```bash
git reset --recurse-submodules HEAD~1
```

### `git config` and submodules

#### `git diff --submodule`

`git diff --submodule` shows the changes in the submodules. You can see that the submodule was updated and get a list of commits that were added to it. If you don’t want to type `--submodule` every time you run `git diff`, you can set it as the default format by setting the diff.submodule config value to “log”.

```bash
git config --global diff.submodule log
```

#### Modify .gitmodules

If you want to have a submodule track that repository’s “stable” branch, you can set it in either your .gitmodules file (so everyone else also tracks it), or just in your local .git/config file. Let’s set it in the .gitmodules file:

```bash
git config -f .gitmodules submodule.<name-submodule>.branch stable
```

If you leave off the -f .gitmodules it will only make the change for you, but it probably makes more sense to track that information with the repository so everyone else does as well.

#### Status of submodules

```bash
git submodule status
```
If you set the configuration setting status.submodulesummary, Git will also show you a short summary of changes to your submodules.

```bash
git config status.submodulesummary 1
```

#### git log

We can actually see the log of commits that we’re about to commit to in our submodule. Once committed, you can see this information after the fact as well when you run git log -p.

```bash
git log -p --submodule
```

#### Useful aliases

```bash
git config alias.sdiff '!'"git diff && git submodule foreach 'git diff'"
git config alias.spush 'push --recurse-submodules=on-demand'
git config alias.supdate 'submodule update --remote --merge'
```

