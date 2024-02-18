Setting up SSH keys with GitHub allows you to securely authenticate with GitHub without needing to enter your username and password every time you interact with a repository. Here's a step-by-step guide to setting up SSH keys with GitHub:

## **Check for Existing SSH Keys**
Before generating new SSH keys, check if you already have SSH keys on your system.

```bash
ls -al ~/.ssh
```

## **Generate a New SSH Key**
If you don't have an SSH key pair, generate one using the `ssh-keygen` command. Press Enter to accept the default file location and optionally set a passphrase for added security.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

## **Add SSH Key to SSH Agent**
Start the SSH agent and add your SSH private key to the agent.

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

## **Copy SSH Public Key**
Copy the SSH public key to your clipboard using the `pbcopy` command (macOS) or display it in the terminal and manually copy it.

```bash
pbcopy < ~/.ssh/id_rsa.pub  # macOS
cat ~/.ssh/id_rsa.pub       # Linux
```

## **Add SSH Key to GitHub**
- Go to your GitHub account settings.
- Click on "SSH and GPG keys" in the left sidebar.
- Click on "New SSH key" or "Add SSH key."
- Paste your SSH public key into the "Key" field.
- Provide a descriptive title for the SSH key.
- Click on "Add SSH key" or "Save SSH key."

## **Test SSH Connection**
To verify that your SSH key is correctly set up, try connecting to GitHub.

```bash
ssh -T git@github.com
```

## **Configure SSH for Multiple Accounts (Optional)**
If you have multiple GitHub accounts, you may need to configure SSH to use different keys for different accounts. This involves creating a `config` file in your `~/.ssh` directory.

```bash
touch ~/.ssh/config
```

Edit the `config` file and specify which SSH key to use for each GitHub hostname:

```
# Default GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

# Second GitHub account
Host github-other
  HostName github.com
  User git
  IdentityFile ~/.ssh/other_id_rsa
```


Setting up SSH keys with GitHub provides a secure and convenient way to authenticate with GitHub repositories. By generating SSH keys, adding them to your GitHub account, and configuring your SSH client, you can securely interact with GitHub repositories without needing to enter your username and password for each operation.