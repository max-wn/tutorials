---
title: Mutt
category: cli
layout:
updated:
---

## Basis

Generate ssh keys used for authentication, password-less logins, and other things.

```bash
ssh-keygen                            # Generate a key interactively
ssh-keygen -f ~/.ssh/filename         # Specify file in which to save the key
ssh-keygen -t ed25519 -a 100          # Generate an ed25519 key with 100 key derivation function rounds
ssh-keygen -t rsa -b 4096 -C "email"  # Generate an RSA 4096 bit key with email as a comment
```

Remove and retrieve keys

```bash
ssh-keygen -l -F remote_host             # Retrieve the key fingerprint
                                         # from a host (useful for confirming
                                         # the authenticity of the host
                                         # when first connecting to it via SSH)
ssh-keygen -R remote_host                # Remove the keys of a host from the known\_hosts file (useful when a known host has a new key)
ssh-keygen -l -E md5 -f ~/.ssh/filename  # Retrieve the fingerprint of a key in MD5 Hex
```

Change the password of a key:

```bash
ssh-keygen -p -f ~/.ssh/filename
```
To transfer a file to a remote computer in the terminal run:

```bash
scp /path/to/local/file remoteuser@remote-ip-address:/path/to/remote/file
```

And to transfer a file from a remote computer to your local computer simply
type first the remote address and then local.

```bash
scp remoteuser@remote-ip-address:/path/to/remote/file  /path/to/local/file
```

Debian/Ubuntu you install on local machine `sudo apt install openssh-client`
and on remote `sudo apt install openssh-server`

on Archlinux you install on local and remote `sudo pacman -S openssh`

## Github SSH workflow

Generate key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"  # generate key
eval "$(ssh-agent -s)"  # Start the ssh-agent in the background
vim ~/.ssh/config       # check to see if your ~/.ssh/config file exists
                        # in the default location
touch ~/.ssh/config     # If the file doesn't exist, create the file
```

Open your `~/.ssh/config` file, then modify the file to contain the following
lines

```
Host *
  AddKeysToAgent yes UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Add your SSH ***private*** key to the ssh-agent and store your passphrase in the
keychain

```bash
ssh-add -K ~/.ssh/id_ed25519
```

Adding a new  ***public*** SSH key to your GitHub account and test connection

```bash
pbcopy < ~/.ssh/id_ed25519.pu  # Copy the SSH public key to your clipboard
                               # and paste it to SSH settings on github site
ssh -T git@github.com          # testing your SSH connection
```

Verify that the fingerprint in the message you see matches [GitHub's RSA
public key fingerprint][001]. If it does, then type `yes`

---

THE END

[001]: https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints "github fingerprint"
