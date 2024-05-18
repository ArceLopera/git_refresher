Debugging with Git involves using various commands to trace the history of changes, identify when and where bugs were introduced, and understand the context of those changes. Here are some essential commands for debugging:

- **`git blame`**: Shows the last modification for each line of a file.
- **`git blame -L`**: Limits the blame output to specified line ranges.
- **`git blame -C -L`**: Tracks lines that have been copied or moved.
- **`git bisect`**: Performs a binary search to find the commit that introduced a bug.


###  File Annotation
The `git blame` command is used to show what revision and author last modified each line of a file. This is particularly useful for finding out who introduced a bug in a specific line of code.


```bash
git blame file.txt
```

In the output, the first column is the commit hash, the second column is the author, and the third column is the line number. The ^ prefix designates lines that were introduced in the repository’s initial commit and have remained unchanged ever since.

####  `git blame -L`
The `-L` option allows you to specify line ranges to limit the output to specific parts of the file.


```bash
# Show blame for lines 10 to 20 in file.txt
git blame -L 10,20 file.txt
```

#### `git blame -C -L`
The `-C` option detects lines that have been copied or moved. Combined with `-L`, it helps find the origin of specific lines even if they have been moved from another file.


```bash
# Show blame for lines 10 to 20 in file.txt, detecting moved lines
git blame -C -L 10,20 file.txt
```

### `git bisect`
The `git bisect` command is a powerful tool to find the commit that introduced a bug by performing a binary search between known good and bad commits.


1. **Start the Bisecting Process**:
   ```bash
   git bisect start
   ```
2. **Mark the Current Commit as Bad**:
   ```bash
   git bisect bad
   ```
3. **Mark an Older Commit as Good**:
   ```bash
   git bisect good <commit-hash>
   ```
4. **Git will checkout an intermediate commit**. Test your code to see if it has the bug:
   ```bash
   # Test the code and if the bug is present, mark the commit as bad
   git bisect bad

   # If the bug is not present, mark the commit as good
   git bisect good
   ```
5. **Repeat** the process until Git identifies the first bad commit.

6. **Reset Bisecting** once you've found the bad commit:
   ```bash
   git bisect reset
   ```


### Example Scenario

Suppose you have a bug in your code and you suspect it was introduced recently. Here’s how you can debug it:

1. **Use `git blame` to identify the commit that modified a specific line**:
   ```bash
   git blame -L 45,45 src/main.py
   ```

   This command will show who last modified line 45 in `src/main.py`.

2. **If the change was copied or moved from another file, use `git blame -C -L`**:
   ```bash
   git blame -C -L 45,45 src/main.py
   ```

3. **If you need to find the commit that introduced a bug, use `git bisect`**:
   ```bash
   git bisect start
   git bisect bad
   git bisect good <commit-hash>
   ```

   Test each commit Git checks out to determine if the bug is present and mark it as good or bad until Git identifies the problematic commit.

By using these commands, you can effectively trace the history of changes, identify the introduction of bugs, and understand the context behind those changes, making debugging with Git a powerful technique in your development workflow.