In Git, you can ignore files using patterns by creating or modifying a special file called `.gitignore`. This file contains a list of file patterns that Git should ignore when tracking changes in your repository.

## **Creating or Modifying `.gitignore`**

 Specify files or directories that Git should ignore.
 The `.gitignore` file should be located at the root of your Git repository.

## **Common File Patterns**

- **Wildcard (`*`):** Matches zero or more characters.
- **Directory (`/`):** Denotes a directory.
- **Negation (`!`):** Excludes files or directories that match the pattern.

The rules for the patterns you can put in the .gitignore file are as follows:

+ Blank lines or lines starting with # are ignored.

+ Standard glob patterns work, and will be applied recursively throughout the entire working tree.

+ You can start patterns with a forward slash (/) to avoid recursivity.

+ You can end patterns with a forward slash (/) to specify a directory.

+ You can negate a pattern by starting it with an exclamation point (!).

Glob patterns are like simplified regular expressions that shells use. An asterisk (*) matches zero or more characters; [abc] matches any character inside the brackets (in this case a, b, or c); a question mark (?) matches a single character; and brackets enclosing characters separated by a hyphen ([0-9]) matches any character between them (in this case 0 through 9). You can also use two asterisks to match nested directories; a/**/z would match a/z, a/b/z, a/b/c/z, and so on.

### a. Ignore Specific Files
```
# Ignore log files
*.log

# Ignore build artifacts
/build/
```

### b. Ignore Files in a Directory
```
# Ignore all files in the temp directory
temp/*

# But track one specific file in temp
!temp/important.txt
```

### c. Ignore Files with a Specific Extension
```
# Ignore all .pdf files
*.pdf
```

### d. Ignore Files in Subdirectories
```
# Ignore all .class files in any directory
**/*.class
```

### e. More examples
```
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```
More examples from [GitHub]( https://github.com/github/gitignore).

## **Applying `.gitignore` Changes**

After modifying the `.gitignore` file, you need to commit the changes to apply them to the repository.

```bash
git add .gitignore
git commit -m "Add .gitignore to ignore specific files"
```

## **Global `.gitignore`**

You can create a global `.gitignore` file that applies to all repositories on your system by configuring Git to use it.

The command `git config --global core.excludesfile [file]` is used to specify a system-wide ignore pattern file that Git will use for all local repositories on your system. 

```bash
git config --global core.excludesfile [file]
```

+ Parameters:

    - **`--global`:** This option specifies that the configuration should be applied globally, affecting all Git repositories on your system.
    - **`core.excludesfile`:** This is the Git configuration key for specifying the location of the system-wide ignore file.
    - **`[file]`:** This parameter specifies the path to the file containing the ignore patterns.

```bash
git config --global core.excludesfile ~/.gitignore_global
```

- This command tells Git to use the specified file (`~/.gitignore_global` in this example) as the system-wide ignore pattern file.
- Any patterns defined in this file will be applied to all local repositories on your system, regardless of their location.

## **Ignoring Changes in Tracked Files**

If you want to ignore changes in files that are already being tracked by Git, you can use the `git update-index --assume-unchanged` command.

The `git update-index --assume-unchanged` command is used to tell Git to temporarily ignore changes to a tracked file. When you run this command, Git updates its index to mark the specified file (`config.ini` in this example) as "assume unchanged".

The `--assume-unchanged` option allows you to tell Git to treat a file as if it hasn't been changed, even if it has been modified since the last commit.

```bash
git update-index --assume-unchanged <file>
```

- `<file>`: The path to the file you want to mark as unchanged.

```bash
git update-index --assume-unchanged config.ini
```

- When you run this command, Git updates its index to mark the specified file (`config.ini` in this example) as "assume unchanged".
- After marking a file as "assume unchanged", Git will not track changes to that file in future commits, treating it as if it hasn't been modified.
- This command is useful when you have local changes to a tracked file that you don't want to commit or stage for commit, such as configuration files with local settings that should not be shared with others.

##### Use Cases
1. **Ignoring Local Changes:** You can use this command to prevent accidentally committing local changes to certain files, such as configuration files with sensitive information or local overrides.
  
2. **Performance Optimization:** By marking files as "assume unchanged", Git can improve performance by not having to check these files for changes when performing various operations, such as `git status` or `git add`.

##### Caveats
- **Manual Updates:** It's important to note that this command only affects how Git treats the file in future operations. If the file is modified again in the future and you want to commit those changes, you will need to remove the "assume unchanged" status using `git update-index --no-assume-unchanged <file>`.

##### Note
- This command only affects your local repository and does not affect the repository's history or other collaborators.

Using `git update-index --assume-unchanged` can be a helpful tool for managing local changes to tracked files in your Git repository, allowing you to prevent accidental commits of sensitive or local-specific changes.

Ignoring files using patterns in Git is essential for preventing certain files or directories from being tracked in your repository. By creating or modifying the `.gitignore` file and specifying file patterns, you can effectively manage which files Git should ignore. Understanding the common file patterns and how to apply them allows you to tailor Git's behavior to suit your project's needs while keeping the repository clean and manageable.