# SSH Without A Password 

Before we can do any work we'll need to be able to connect to the servers. With Linux the standard way to access them is via SSH.

```bash
ssh UserName@SSHserver.example.com
```

SSH using a password works well but isn't as secure. Your password is sent to the server over the network. The better option is SSH with a private key. The private key never leaves your workstation even when connecting to a host.

## Generate public/private rsa key pair

Run the command 
```bash
ssh-keygen
```

Enter file in which to save the key
```bash
(/home/{user}/.ssh/id_rsa)
```

Enter passphrase
```bash
Press Enter if you don't want a passphrase (leave empty)
```

Validate passphrase
```bash
Press Enter if you don't want a passphrase (leave empty)
```

Result
```bash
Your identification has been saved in /home/{user}/.ssh/id_rsa.
Your public key has been saved in /home/{user}/.ssh/id_rsa.pub.
The key fingerprint is:
b2:ad:a0:80:85:ad:6c:16:bd:1c:e7:63:4f:a0:00:15 user@host
The key's randomart image is:
+---[RSA 2048]----+
|           . o + |
|         = X * . |
|      . O @ = . .|
|     + o X O . + |
|   . = S * = o   |
| . ..o . . o .   |
|     + .o. E     |
|      + ...o.    |
|     . . ooo+.   |
+----[SHA256]-----+
```

Setting a passphrase to the private key is optional but recommanded. This protects the connection with multiple factors (Something you know, the passphrase. Something you have, the key)

This created two file `id_rsa` and `id_rsa.pub`. `id_rsa` is your private key and should never be shared or leave your workstation. `id_rsa.pub` is the public key, it can and will be shared.

```bash
cat ~/.ssh/id_rsa.pub
```

## Copy the SSH Public Key to the Remote Host

In order for the host to allow your private key to connect you need to give it your public key. The public key needs to go in the `authorized_keys` file of your user's home directory (ex: /home/{user}/.ssh/authorized_keys). The file then needs to be read/writable for the user only (600)

The `ssh-copy-id` cli will automatically do all of this for you.

Copy using the command ssh-copy-id
```bash
ssh-copy-id {user}@{serverip}
```
- EXAMPLE: ssh-copy-id testuser@10.45.2.1

Enter your user passowrd
```bash
{user}@{serverip}'s password: 
```
- EXAMPLE: testuser@10.45.2.1's password:

## Login to the Remote Host Without a Passwor

Now that your public key has been added to the host we can login.

Login
```bash
ssh {user}@{serverip}
```
- EXAMPLE: ssh testuser@10.45.2.1

 Now you can connect to the server without a password.

You can very the `authorized_keys` file

```bash
ls -l ~/.ssh/authorized_keys
cat ~/.ssh/authorized_keys
```

## Disable Password Login

We can force the use of SSH Keys by disabling Password Authentication. This is recommanded by the NIST standard


Edit the sshd config file
```bash
sudo vi /etc/ssh/sshd_config
```

Find the line for `PasswordAuthentication` and set it to `no`

```
PasswordAuthentication no
```

## Aditional Information

SSH (Secure Shell or Secure Socket Shell) is a network protocol that gives users -- particularly systems administrators -- a secure way to access a computer over an unsecured network.

In addition to providing strong encryption, SSH is widely used by network administrators to manage systems and applications remotely, enabling them to log in to another computer over a network, execute commands and move files from one computer to another.

SSH runs on the TCP/IP protocol suite in a client-server architecture. SSH provides encryption and authentication using public key cryptography. The most basic use of SSH is to connect to a remote host for a terminal session.
