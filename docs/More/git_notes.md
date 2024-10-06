`git notes` is a command that allows you to add, inspect, and manage additional information (referred to as *notes*) toobjects without altering the objects themselves. This is particularly useful for attaching metadata like comments, code reviews, or documentation to objectss without changing their hash (which would occur if you amend or edit them directly). It is a lesser-known yet powerful feature of git, often overshadowed by its challenging usability. Git notes allow users to append metadata to commits, blobs, and trees in git without altering the original objects. This feature is highly versatile, enabling a range of applications from tracking time per commit, adding review and testing information, to facilitating fully distributed code reviews. Despite their potential, git notes suffer from limited adoption and usability issues, as seen in GitHub discontinuing their display in 2014.

### Use Cases for Git Notes

- **Adding documentation** to commits (e.g., explaining why a specific change was made).
- **Code reviews** and feedback without altering the commit.
- **Tracking external information** (e.g., bug tracker IDs or task numbers) linked to a commit.
- **Tracking time per commit** without altering the commit.

### Adding Notes to Commits

You can add notes to any commit using the `git notes add` command. By default, notes are added to the most recent commit.

#### Example 1: Adding a Note to the Latest Commit

First, create a new commit:
```bash
git commit -m "Initial commit"
```

Now, add a note to this commit:
```bash
git notes add -m "This is an extra explanation for the initial commit."
```

You can inspect the note with:
```bash
git log --show-notes
```

Output:
```
commit <hash>
Author: Your Name <you@example.com>
Date:   <date>

    Initial commit

Notes:
    This is an extra explanation for the initial commit.
```

#### Example 2: Adding Notes to a Specific Commit

To add a note to a specific commit, you need to specify the commit hash:
```bash
git notes add -m "Fixes bug #1234." <commit_hash>
```

Now if you check the logs:
```bash
git log --show-notes
```

You’ll see:
```
commit <commit_hash>
Author: Your Name <you@example.com>
Date:   <date>

    Some previous commit message

Notes:
    Fixes bug #1234.
```

### Viewing Notes

To view notes attached to commits, use:
```bash
git log --show-notes
```

Alternatively, to view the note for a specific commit:
```bash
git notes show <commit_hash>
```

### Editing Notes

If you need to modify a note after it’s been created, use:
```bash
git notes edit <commit_hash>
```

This will open your default editor where you can modify the note.

### Removing Notes

To delete a note from a commit:
```bash
git notes remove <commit_hash>
```

### Listing All Notes

To list all notes in your repository:
```bash
git notes list
```

This will display all the commit hashes along with their associated notes.

### Merging Notes

When collaborating on a project, different contributors might add notes to the same commit. Git provides a way to merge these notes.

#### Example: Merging Notes

First, fetch the notes from the remote repository:
```bash
git fetch origin refs/notes/*:refs/notes/*
```

Then, merge the notes from another branch:
```bash
git notes merge
```

### Splitting Notes by Ref Namespace

You can have multiple sets of notes for the same commit by specifying different namespaces. By default, notes are saved under the `refs/notes/commits` reference, but you can store them elsewhere using the `--ref` option.

#### Example: Using a Custom Namespace for Notes

Add notes under a different reference (e.g., `refs/notes/review`):
```bash
git notes --ref=review add -m "This commit has passed the review." <commit_hash>
```

To view these notes:
```bash
git log --show-notes=review
```

This allows you to have multiple categories of notes, such as *code reviews* or *documentation*, without conflicting with one another.

### Notes with Multiple Lines

If you want to add a note with multiple lines, use:
```bash
git notes add
```

This will open the default editor, where you can write your multi-line note.

Alternatively, you can use:
```bash
git notes add -m "First line" -m "Second line"
```

### Storing Notes in Remote Repositories

By default, notes are not pushed to remotes. You need to explicitly push and fetch notes.

#### Example: Pushing Notes to a Remote

To push notes to a remote repository:
```bash
git push origin refs/notes/commits
```

#### Example: Fetching Notes from a Remote

To fetch notes from a remote:
```bash
git fetch origin refs/notes/commits:refs/notes/commits
```
### Why Git Notes Face Usability Challenges
Despite their potential, Git notes have limited adoption due to usability challenges and limited toolchain support. For example, GitHub supported displaying notes for a time, but discontinued this feature in 2014 due to low usage and technical limitations.

+ **Lack of visibility:** Notes are not shown in a default git log output unless explicitly requested with git log --show-notes. This can make them "invisible" to most users unless they are aware of the feature.

+ **Not synchronized by default:** Notes are not pushed or fetched by default when collaborating with remote repositories. Developers must manually handle pushing and fetching notes using commands like git push origin refs/notes/commits and git fetch origin refs/notes/commits:refs/notes/commits.

+ **Toolchain integration:** Many popular Git hosting services (e.g., GitHub, GitLab) and GUIs do not natively support Git notes in their interfaces. This limits their visibility and usefulness in typical workflows, which focus more on commit messages and branch structures.




### Conclusion

`git notes` provides a flexible way to attach additional metadata to commits without modifying them. This feature is particularly useful for attaching comments, reviews, or documentation to commits, and can be customized by namespaces for different purposes. It’s also a great way to keep a clean history while still having the context you need.