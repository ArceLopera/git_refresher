In Git, tagging is the process of assigning a meaningful label or marker to a specific commit in the repository's history. Tags are often used to mark significant points in the project's development, such as releases, versions, or milestones. 

## **Types of Tags**

There are two types of tags in Git:

- **Lightweight Tags** These are simple pointers to specific commits, similar to branches but immutable. They are created with the `-l` option.
  
- **Annotated Tags** These are stored as full objects in the Git database, containing details like the tagger's name, email, date, and a tagging message. They are created with the `-a` option.

## **Creating Tags**

### Lightweight Tags
```bash
git tag <tag-name> <commit-hash>
```
```bash
git tag v1.0.0 7c52d53
```
### Annotated Tags
```bash
git tag -a <tag-name> <commit-hash> -m "Tagging message"
```
```bash
git tag -a v1.0.0 7c52d53 -m "Version 1.0.0 release"
```
The -m specifies a tagging message, which is stored with the tag. If you don’t specify a message for an annotated tag, Git launches your editor so you can type it in.

Tagging Later : You can also tag commits after you’ve moved past them.

## **Viewing Tags**

To view all tags in the repository:
```bash
git tag
```
In Git, listing tags allows you to view all tags present in the repository. You can use wildcards to filter tags based on specific patterns. Here's how you can list tags in Git with examples and wildcards:

### **Listing Tags Matching a Pattern**

You can use wildcards to list tags matching a specific pattern. Listing tag wildcards requires -l or --list option. The two common wildcards used are:

- `*`: Matches any sequence of characters.
- `?`: Matches any single character.

#### Listing Tags Starting with a Prefix

To list tags starting with a specific prefix (e.g., `v1`):
```bash
git tag -l 'v1*'
```

#### **Listing Tags with Annotated Tags Only**

To list only annotated tags (excluding lightweight tags):
```bash
git tag -l -n
```

Using wildcards with `git tag -l` allows you to filter and list tags based on specific patterns, making it easier to manage and navigate through tags in your Git repository. Listing tag wildcards requires -l or --list option.

#### **Comparing local and remote tags**

To compare local and remote tags:
```bash
# Get local repository tags
git tag -l > repo1_tags.txt

# Get tags from the other repository, filtering out ^{} entries
git ls-remote --tags other-repo | awk '{print $2}' | grep -v '\^{}' | sed 's|refs/tags/||' > repo2_tags.txt

# Compare local and remote tags
diff repo1_tags.txt repo2_tags.txt
```
For more details refer to [git ls-remote](../More/plumbing.md#git-ls-remote)

## **Pushing Tags to Remote**

By default, the git push command doesn’t transfer tags to remote servers.
To push a specific tag to the remote repository:
```bash
git push origin <tag-name>
```

To push all tags to the remote repository:
```bash
git push origin --tags
```

```bash
git push origin v1.0.0
```

git push pushes both types of tags.

git push <remote> --tags will push both lightweight and annotated tags. There is currently no option to push only lightweight tags, but if you use git push <remote> --follow-tags only annotated tags will be pushed to the remote.

## **Deleting Tags**

To delete a specific tag:
```bash
git tag -d <tag-name>
```

```bash
git tag -d v1.0.0
```

To delete a tag on the remote repository:
```bash
git push origin --delete <tag-name>
```

```bash
git push origin --delete v1.0.0
```

## **Checking out Tags**

To checkout a specific tag and create a detached HEAD state:
```bash
git checkout <tag-name>
```

```bash
git checkout v1.0.0
```

In “detached HEAD” state, if you make changes and then create a commit, the tag will stay the same, but your new commit won’t belong to any branch and will be unreachable, except by the exact commit hash. Thus, if you need to make changes — say you’re fixing a bug on an older version, for instance — you will generally want to create a branch:

```bash
$ git checkout -b version2 v2.0.0
#Switched to a new branch 'version2'
```
If you do this and make a commit, your version2 branch will be slightly different than your v2.0.0 tag since it will move forward with your new changes, so do be careful.

## **Tagging Best Practices**

- Use semantic versioning (e.g., v1.0.0, v1.1.0) for version tags to maintain consistency.
- Annotate tags with meaningful messages describing the purpose or significance of the tagged commit.
- Push tags to the remote repository to share them with collaborators and facilitate release management.

Git tagging is a powerful feature for marking important points in your project's history, making it easier to navigate and manage releases. 