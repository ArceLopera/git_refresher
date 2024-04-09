## In Git

In Git, aliases are shortcuts or custom commands that you can create to simplify and streamline your workflow. They can be particularly useful to reduce typing and remember complex commands. Here's a step-by-step guide on how to add useful aliases, along with some suggestions for commonly used and helpful aliases:

### 1. **Adding Git Aliases**

Open your terminal and run the following command to open the global Git configuration file in your default text editor:

```bash
git config --global --edit
```

This command opens the global configuration file in your preferred text editor. If you haven't configured a text editor, Git will prompt you to choose one.

### 2. **Adding Aliases to the Configuration File**

Inside the configuration file, add the following section to set up aliases:

```bash
[alias]
  alias_name = actual_git_command
```

Replace `alias_name` with the desired alias and `actual_git_command` with the Git command or series of commands you want to associate with the alias.

### 3. **Example Aliases**

Here are some commonly used and helpful aliases:

```bash
[alias]
  # Show a compact and colorful log
  lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
  
  # Show the current branch
  current-branch = symbolic-ref --short HEAD

  # Show a concise status
  st = status -s
  
  # Commit all changes with a message
  ca = !git add -A && git commit -m
  
  # Show the diff of the last commit
  last-commit-diff = diff HEAD^ HEAD
```

### 4. **Explanation of Some Aliases**

- `lg`: Displays a compact and colorful log.
- `current-branch`: Shows the current branch.
- `st`: Provides a concise status of the repository.
- `ca`: Commits all changes with a message. Usage: `git ca "Commit message"`.
- `last-commit-diff`: Shows the diff of the last commit.

### 5. **Save and Exit**

Save the configuration file and exit your text editor.

### 6. **Using Aliases**

After adding aliases, you can use them in the terminal like regular Git commands. For example:

```bash
git lg
```

This will execute the log alias and display a compact and colorful log.

### 7. **Additional Tips**

- Choose aliases that align with your workflow and make common tasks more efficient.
- Keep aliases short and memorable.
- Test your aliases in a safe environment before using them extensively.

Creating and using Git aliases can significantly improve your productivity and make Git commands more convenient. Feel free to customize these aliases to suit your preferences and workflow.

## In Bash
Setting up aliases in Bash involves editing your Bash profile file. The Bash profile file is a script that runs whenever you start a new terminal session. Here's a step-by-step guide on how to set up aliases in Bash:

### 1. **Open Your Bash Profile**

Open your Bash profile file in a text editor. The Bash profile is usually one of the following files, depending on your operating system:

- For Linux/macOS: `~/.bashrc` or `~/.bash_profile`
- For Windows using Git Bash: `~/.bashrc`

If the file doesn't exist, you can create it.

```bash
# For Linux/macOS
nano ~/.bashrc   # or use your preferred text editor

# For Windows using Git Bash
nano ~/.bashrc   # or use your preferred text editor
```
or using VScode

```bash
# For Linux/macOS
code ~/.bashrc   # or use your preferred text editor

# For Windows using Git Bash
code ~/.bashrc   # or use your preferred text editor
```

### 2. **Add Your Aliases**

In your Bash profile, add lines for each alias you want to create. The syntax is as follows:

```bash
alias alias_name='actual_command'
```

Replace `alias_name` with the desired alias and `actual_command` with the actual command or series of commands you want to associate with the alias.

### 3. **Example Aliases**

Here are some example aliases:

```bash
# Git aliases
alias gst='git status'
alias gl='git log'
alias gll='git log --oneline'
alias glog='git log --oneline --decorate --graph'
alias gamend='git commit --amend'
alias gr='git remote -v'
alias gb='git branch'
alias gba='git branch -a'
alias gd='git diff'
alias gds='git diff --staged'
alias gsub='git submodule sync; git submodule update --init --recursive'
alias gsreset='git submodule foreach --recursive git reset --hard'
alias gf='git fetch --all --prune --tags --prune-tags --progress'
alias gc='git commit'

# Navigation aliases
alias cdproj='cd ~/projects'
alias cddownloads='cd ~/Downloads'
alias cddesktop='cd ~/Desktop'
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'

# Shortcuts for common commands
alias ll='ls -alF'
alias la='ls -A'
alias cp='cp -i'
alias mv='mv -i'
alias rm='rm -i'

# 🤘
alias yolo='git push --force'

# useful for daily stand-up
# See https://dev.to/maxpou/git-cheat-sheet-advanced-3a17
git-standup() {
    AUTHOR=${AUTHOR:="`git config user.name`"}

    since=yesterday
    if [[ $(date +%u) == 1 ]] ; then
        since="2 days ago"
    fi

    git log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --since "$since" --author="$AUTHOR"
}
```

### 4. **Save and Exit**

Save your changes and exit the text editor.

- For Nano, press `Ctrl` + `X`, then `Y` to confirm saving, and `Enter` to exit.
- For other editors, follow their respective save and exit commands.

### 5. **Reload Your Bash Profile**

To apply the changes without restarting your terminal, either close and reopen the terminal or use the `source` command:

```bash
source ~/.bashrc
```

This command reloads the Bash profile, making your new aliases immediately available.

### 6. **Verify Your Aliases**

To verify that your aliases are set up correctly, you can use the `alias` command:

```bash
alias
```

This will display a list of all defined aliases in your current session.

Now you can use your aliases in the terminal for quick and convenient access to frequently used commands. Customize the aliases to match your workflow and make your command-line experience more efficient.

## In Powersell

In PowerShell, you can add aliases using your PowerShell profile script. The profile script is a PowerShell script that is automatically executed when you start a PowerShell session. Here's a step-by-step guide on how to add aliases in PowerShell:

### 1. **Open Your PowerShell Profile**

Open your PowerShell profile script in a text editor. If you don't have a profile, you can create one. The profile script is usually located at one of the following paths:

- Current User, Current Host (for all users): `$PROFILE.AllUsersCurrentHost`
- Current User, Current Host (for the current user): `$PROFILE.CurrentUserCurrentHost`
- All Users, All Hosts (for all users): `$PROFILE.AllUsersAllHosts`
- Current User, All Hosts (for the current user): `$PROFILE.CurrentUserAllHosts`

To open your profile script, you can use the following command:

```powershell
notepad $PROFILE
```

This command opens the profile script in Notepad. If the file doesn't exist, PowerShell will prompt you to create it.

or use VSCode to open your profile script:

```powershell
code $PROFILE
```

### 2. **Add Your Aliases**

In your PowerShell profile, add lines for each alias you want to create. The syntax is as follows:

```powershell
function alias_name {
    actual_command
}
```

Replace `alias_name` with the desired alias and `actual_command` with the actual command or series of commands you want to associate with the alias.

### 3. **Example Aliases**

Here are some example aliases:

```powershell
# Git aliases
function gs { git status }
function ga { git add }
function gc { git commit }
function gp { git push }
function gl { git log --oneline }

# Navigation aliases
function cdproj { Set-Location C:\Projects }
function cddownloads { Set-Location C:\Users\YourUsername\Downloads }
function cddesktop { Set-Location C:\Users\YourUsername\Desktop }

# Shortcuts for common commands
function ll { Get-ChildItem -Force }
```

### 4. **Save and Exit**

Save your changes and exit the text editor.

### 5. **Reload Your PowerShell Profile**

To apply the changes without restarting PowerShell, you can use the `Import-Module` cmdlet:

```powershell
Import-Module $PROFILE
```

### 6. **Verify Your Aliases**

To verify that your aliases are set up correctly, you can use the `Get-Alias` cmdlet:

```powershell
Get-Alias
```

This will display a list of all defined aliases in your current PowerShell session.

Now you can use your aliases in the PowerShell console for quick and convenient access to frequently used commands. Customize the aliases to match your workflow and make your PowerShell experience more efficient.