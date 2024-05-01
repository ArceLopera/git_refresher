## GPG Introduction

GPG (GNU Privacy Guard) is a free and open-source software for secure communication and data encryption. In Git, GPG is used for cryptographic signing to verify the authenticity of commits and tags. When you sign a commit or tag with your GPG key, it proves that the changes were indeed made by you and haven't been tampered with.

First of all, if you want to sign anything you need to get GPG configured and your personal key installed.
```bash
gpg --list-keys
```
If you don’t have a key installed, you can generate one with gpg --gen-key.

```bash
gpg --gen-key
```
Once you have a private key to sign with, you can configure Git to use it for signing things by setting the user.signingkey config setting.

```bash
git config --global user.signingkey 0A46826A!
```

Now Git will use your key by default to sign tags and commits if you want.

## Signing Tags

To sign a tag with GPG, you can use the `-s` or `--sign` option with the `git tag` command. For example:

```bash
git tag -s v1.0 -m "Release version 1.0"
```

This creates a signed tag `v1.0` with the specified message. When someone verifies this tag, they can see that it's been signed by your GPG key.

## Verifying Tags

To verify the authenticity of a signed tag, you can use the `git tag -v` command followed by the tag name. For example:

```bash
git tag -v v1.0
```

Git will check the signature of the tag against the associated GPG public key and display whether the tag is valid or not.

## Signing Commits

To sign commits with GPG, you can use the `-S` or `--gpg-sign` option with the `git commit` command. For example:

```bash
git commit -S -m "Commit message"
```

This signs the commit with your GPG key. When others view the commit, they can verify its authenticity using your GPG public key.

## Summary

- GPG (GNU Privacy Guard) is used for cryptographic signing in Git to verify the authenticity of commits and tags.
- You can sign tags using the `git tag -s` command and verify them with `git tag -v`.
- Commits can be signed using the `git commit -S` command.
- GPG signing ensures the integrity and authenticity of your Git history, providing a layer of trust and security for your repository.