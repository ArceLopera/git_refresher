After installing Git, the next step is to configure it with your user information. This ensures that your commits are associated with the correct identity. 
Git uses a series of configuration files to determine non-default behavior that you may want. The first place Git looks for these values is in the system-wide `[path]/etc/gitconfig` file, which contains settings that are applied to every user on the system and all of their repositories. If you pass the option `--system` to `git config`, it reads and writes from this file specifically.

The next place Git looks is the `~/.gitconfig` (or `~/.config/git/config`) file, which is specific to each user. You can make Git read and write to this file by passing the `--global` option.
Finally, Git looks for configuration values in the configuration file in the Git directory (`.git/config`) of whatever repository you’re currently using. These values are specific to that single repository, and represent passing the `--local` option to `git config`. If you don’t specify which level you want to work with, this is the default.

Each of these “levels” (system, global, local) overwrites values in the previous level, so values in `.git/config` trump those in `[path]/etc/gitconfig`.
Git’s configuration files are plain-text, so you can also set these values by manually editing the file and inserting the correct syntax. It’s generally easier to run the git config command, though.

Follow these steps to configure Git with your user information used across all local repositories:

## 1. **Open a Terminal/Command Prompt**
   - **Windows:** Git Bash, Command Prompt, or PowerShell.
   - **macOS/Linux:** Terminal.

## 2. **Set Your Username**
   Run the following command, replacing "Your Name" with your actual name:
   ```bash
   git config --global user.name "firstname Lastname"
   ```

## 3. **Set Your Email Address**
   Run the following command, replacing "your.email@example.com" with your actual email address:
   ```bash
   git config --global user.email "your.email@example.com"
   ```

## 4. **Configure Line Endings**
   To prevent line ending issues when collaborating across different platforms, set your preferred line ending configuration. For Unix-style line endings (LF), run:
   ```bash
   git config --global core.autocrlf input
   ```
   For Windows-style line endings (CRLF), run:
   ```bash
   git config --global core.autocrlf true
   ```

## 5. **Configure Text Editor (Optional)**
   
   When you launch VS Code from the command line, you can pass the --wait argument to make the launch command wait until you have closed the new VS Code instance. This can be useful when you configure VS Code as your Git external editor so Git will wait until you close the launched VS Code instance.

   Set your preferred text editor for Git. For example, to use VSCode:
   ```bash
   git config --global core.editor "code --wait"
   ```
   use VS Code's diff and merge capabilities even when using Git from command-line. Add the following to your Git configurations to use VS Code as the diff and merge tool:
   ```bash
   git config --global -e
   ```
and change the following:
   ```bash
   [diff]
      tool = default-difftool
   [difftool "default-difftool"]
      cmd = code --wait --diff $LOCAL $REMOTE
   [merge]
      tool = code
   [mergetool "code"]
      cmd = code --wait --merge $REMOTE $LOCAL $BASE $MERGED
   ```
This uses the --diff option that can be passed to VS Code to compare two files side by side. The merge tool will be used the next time Git discovers a merge conflict.

To summarize, here are some examples of where you can use VS Code as the editor:

- `git rebase HEAD~3 -i` do interactive rebase using VS Code
- `git commit` use VS Code for the commit message
- `git add -p` followed by `e` for interactive add
- `git difftool <commit>^ <commit>` use VS Code as the diff editor for changes

### Git output window
You can always peek under the hood to see the Git commands that are being run. This is helpful if something strange is happening or if you are just curious.

To open the Git output window, run View > Output and select Log (Git) from the dropdown list.

## 6. **Enable Color Output**
Improve readability by enabling color output in the Git command line:

   ```bash
   git config --global color.ui auto
   ```
This setting adds color to various Git outputs, making it easier to distinguish between different types of information.To view your Git configuration, run:

## 7. **Check Configuration**
   To view your Git configuration, run:
   ```bash
   git config --list
   ```

   Ensure that your name, email, and other settings are correctly displayed.

## 8. **Verify Configuration**
   Run the following commands to verify your configuration:
   ```bash
   git config user.name
   git config user.email
   ```

   These commands should display the values you've just set.

## Additional Tips

- **Credential Caching**

  To avoid entering your credentials repeatedly, you can set up credential caching. On Windows, Git Credential Manager is often used. On macOS/Linux, you can use the credential helper provided by Git:
  ```bash
  git config --global credential.helper cache
  ```

- **SSH Key**

  If you prefer using SSH for authentication, [generate an SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) and [associate it with your GitHub account](../Github/git_ssh.md).

- **Commit templates**

   Consider a template file at ~/.gitmessage.txt to use as a commit message template.
   ```bash
   [Ticket: X] Subject line (try to keep under 50 characters)

   Multi-line description of commit,
   feel free to be detailed.
   ```
   then set the commit.template configuration option to the path to this file.

   ```bash
   git config --global commit.template ~/.gitmessage.txt
   ```

- **core.pager**

This setting determines which pager is used when Git pages output such as log and diff. You can set it to more or to your favorite pager (by default, it’s less), or you can turn it off by setting it to a blank string:

```bash
git config --global core.pager ''
```
If you run that, Git will print the entire output of all commands, no matter how long they are.

- **user.signingkey**

If you’re making signed annotated tags (as discussed in Signing Your Work), setting your GPG signing key as a configuration setting makes things easier. Set your key ID like so:

```bash
git config --global user.signingkey <gpg-key-id>
```

Now, you can sign tags without having to specify your key every time with the git tag command:

```bash
git tag -s <tag-name>
```

- **core.excludesfile**

You can put patterns in your project’s `.gitignore` file to have Git not see them as untracked files or try to stage them when you run `git add` on them.

But sometimes you want to ignore certain files for all repositories that you work with. In that case, you can set your global `.gitignore_global` file as follows:

This setting lets you write a kind of global .gitignore file. If you create a ~/.gitignore_global file with these contents:

```
*~
.*.swp
.DS_Store
```
Then you run 
```bash 
git config --global core.excludesfile ~/.gitignore_global
``` 

Git will never again bother you about those files.

