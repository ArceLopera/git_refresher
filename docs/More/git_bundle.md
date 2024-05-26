Bundling in Git is a way to package multiple Git objects (such as commits, trees, and blobs) into a single file, which can be transferred and applied to another repository. This is particularly useful for scenarios where direct access to a remote repository is not available, such as offline transfers or sneakernet situations.

- **Offline Transfers:** When network access is limited or unavailable, you can bundle changes, transfer them via physical media (like USB drives), and apply them to another repository.
- **Archiving:** Bundle can be used to archive a specific state of a repository, which can be stored and applied later.
- **Distribution:** Distribute a repository snapshot without requiring direct network access to the Git server.

By understanding and utilizing Git bundling, you can efficiently manage and transfer repository data in scenarios where traditional network operations are impractical.


|                              Command                              |                         Description                         |
| ----------------------------------------------------------------- | ----------------------------------------------------------- |
| [`git bundle create <file> <reference>`](#creating-a-bundle)      | Creates a bundle file containing the specified reference(s) |
| [`git bundle list-heads <file>`](#listing-references-in-a-bundle) | Lists the references stored in a bundle file                |
| [`git clone <bundle-file> <directory>`](#cloning-from-a-bundle)   | Clones a new repository from the bundle                     |
| [`git fetch <bundle-file> <reference>`](#fetching-from-a-bundle)  | Fetches updates from a bundle into an existing repository   |
| [`git pull <bundle-file> <reference>`](#applying-a-bundle)        | Applies changes from a bundle to the current branch         |

### Creating a Bundle

To create a bundle, you use the `git bundle create` command, specifying the name of the bundle file and the range of commits or references you want to include.

Suppose you have a repository with the following commit history:

```
A -- B -- C -- D (master)
```

You want to create a bundle that includes all commits in the master branch.

```bash
git bundle create repo.bundle master
```

This command creates a file named `repo.bundle` containing all the commits from the master branch.  If you intend for this to be cloned somewhere else, you should add HEAD as a reference.

```bash
git bundle create repo.bundle HEAD master
```

### Listing References in a Bundle

You can list the references stored in a bundle file using the `git bundle list-heads` command. This shows the branches and commits included in the bundle.

```bash
git bundle list-heads repo.bundle
```

Output:
```
b1a2d3c4e5f6g7h8i9j0 - refs/heads/master
```

### Cloning from a Bundle

If you want to create a new repository from a bundle, you can use the `git clone` command with the bundle file.

```bash
git clone repo.bundle new-repo
```

This creates a new repository named `new-repo` initialized with the contents of the bundle. If you don’t include HEAD in the references, you have to also specify -b master or whatever branch is included because otherwise it won’t know what branch to check out.

### Fetching from a Bundle

If you already have a repository and want to fetch updates from a bundle, you can use the `git fetch` command with the bundle file.

Suppose you have a repository that already contains commits up to `B`, and you want to fetch commits `C` and `D` from the bundle.

```bash
git fetch repo.bundle master
```

After this command, your repository will include the commits from the bundle.

### Bundling Specific Revisions

You can also bundle specific revisions or ranges of commits. For example, to bundle commits from a specific tag to the current commit:

```bash
git bundle create updates.bundle v1.0..HEAD
```

This bundles all commits from the `v1.0` tag to the current `HEAD`.

To get the differences between my local changes to the bundle and the remote repository, you can use the `git log` command with the bundle file. 
```bash
git log --oneline master ^origin/master
```

Then only bundle the differences.

```bash
git bundle create updates.bundle master ^origin/master
```

### Applying a Bundle

To apply a bundle to an existing repository, you can use the `git pull` command with the bundle file.

```bash
git pull repo.bundle master
```

This applies the changes from the `master` branch in the bundle to your current repository.

### Verifying Bundles

The bundle verify command will make sure the file is actually a valid Git bundle and that you have all the necessary ancestors to reconstitute it properly.

```bash
git bundle verify repo.bundle
```

The verify sub-command will tell you the heads as well. The point is to see what can be pulled in, so you can use the fetch or pull commands to import commits from this bundle. Here we’ll fetch the master branch of the bundle to a branch named other-master in our repository.

```bash
git fetch repo.bundle master:other-master
```
