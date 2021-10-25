---
title: SSH
category: cli
tags:
updated:
---

## ssh how to disable password and allow only key authentication

Key authentication is more secure over password authentication. Just because
users might have weak passwords, an attacker can easily brute force them. On
the other hand, it's impossible to brute force ssh key. That's why I encourage
you to use ssh key authentication and below I will show you how to configure
it.

### Client part

Generate key on client machine.

```bash
ssh-keygen
```

You will be asked to enter path to your key, default path would be OK. Also
you may enter passphrase or leave it empty.

Copy public key to ssh server and remove it from client machine.

```bash
scp yourkey.pub username@yourserver.com:~/
rm yourkey.pub
```

### Server part

ssh to your server using password.

```bash
ssh username@yourserver.com
```

Create .ssh directory, set permissions

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Append public key to authorized keys, set permissions:

```bash
cat ~/yourkey.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Edit `/etc/ssh/sshd_config` using any text editor and set the following
settings:

```config
RSAAuthentication yes
PubkeyAuthentication yes
PermitEmptyPasswords no
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```

Reload configuration for your ssh service (or restart):

```bash
sudo service ssh reload
```

Now ssh to your server using password and make sure you get permission denied
error.

```bash
ssh username@yourserver.com
```

Finally, ssh to your server using private key. You should successfully connect
without any password prompt.

```bash
ssh -i yourkey username@yourserver.com
```

---

THE END
