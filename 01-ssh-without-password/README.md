# SSH Without A Password

Before we can do any work we'll need to be able to connect to the servers. With Linux the standard way to access them is via SSH.

```bash
ssh UserName@SSHserver.server1.com
```

SSH using a password works well but isn't as secure. Your password is sent to the server over the network. The more secure option is SSH with a private key. The private key never leaves your workstation even when connecting to a host.

## The Case for Key-Based Authentication

While SSH with a password is functional, it is not considered as secure for several reasons:

- **Brute Force Vulnerability:** Passwords can be guessed and are subject to brute-force attacks.
- **Credential Stuffing:** If the password to SSH to a server is reused with other services, the server is now compromised as soon as any of those services suffer a data breach.
- **Network Exposure:** Even though passwords are encrypted when sent over the network, this is still less safe then if they were never sent at all.
- **User Fatigue:** Strong passwords are hard to remember, while SSH keys are more secure without requiring a strong password.

## Padlock and Key Analogy

Think of the `Public Key` as a padlock. You give a copy of this padlock to a server that you want to be able to connect to. The server is now "locked" with your padlock.  

The `Private Key` is the key that can open the padlock. You keep this key on your workstation at all times. You are now able to connect to the server by proving that your key unlocks the padlock, which is done without your `Private Key` ever leaving your workstation.  

## Generate public/private ssh key pair

To generate a pair of public and private keys, run the following command

```bash
ssh-keygen
```

Enter the file in which to save the key. 

```bash
(/home/{user}/.ssh/id_ed25519)
```

Enter a passphrase (you can leave this as empty)

```bash
Press Enter if you don't want a passphrase (leave empty)
```

The results

```bash
Your identification has been saved in /home/{user}/.ssh/id_ed25519.
Your public key has been saved in /home/{user}/.ssh/id_ed25519.pub.
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

Setting a passphrase to the private key is optional but recommended. This protects the connection with multiple factors (Something you know: the passphrase. Something you have: the key)

This created two files `id_ed25519` and `id_ed25519.pub`. `id_ed25519` is your private key and should never be shared or leave your workstation. `id_ed25519.pub` is the public key, which can and will be shared.

To view the contents of the public key

```bash
cat ~/.ssh/id_ed25519.pub
```

```text
ssh-ed25519 AAAAC3Nza...4jgu8 pluser@ip-172-16-0-129
```

- `ssh-ed25519`: This is the key type. It identifies the algorithm that was used to create the key (ed25519). 
- `AAAAC3Nza...6qXRE/4jgu8`: This is the key itself, the "padlock" from the analogy before. 
- `pluser@ip-172-16-0-129`: This is only a comment helping to identify the key, usually telling you the username and hostname of the key's owner. 

## SSH Key File Permissions

SSH is extremely strict about file permissions. Keys must only be accessible by your user on your workstation, and SSH will refuse to use them if this is not the case. 

Run the following to view the permissions on the `.ssh` directory

```bash
ls -ld ~/.ssh
```

The output

```text
drwx------ 2 pluser pluser 4096 May  5 18:44 /home/pluser/.ssh
```

The `rwx` in the output means that the `.ssh` directory can only be accessed by `pluser`, which is your user. This means that no other user can access any files within your `.ssh` directory. 

To view the permissions of the ssh keys we created

```bash
ls -l ~/.ssh
```

The output

```text
-rw-rw-r-- 1 pluser pluser  181 May  5 18:41 config
-rw------- 1 pluser pluser  419 May  5 18:20 id_ed25519
-rw-r--r-- 1 pluser pluser  104 May  5 18:20 id_ed25519.pub
-rw------- 1 pluser pluser 2934 May  5 18:41 known_hosts
-rw------- 1 pluser pluser 2098 May  5 18:41 known_hosts.old
```

We care about the `id_ed25519` and `id_ed25519.pub` files. 

- `-rw-------` means that the private key can only be read and edited by `pluser`. 
- `-rw-r--r--` means that the public key can only be edited by `pluser`, and read by everyone. 

These permissions are strict to ensure that no other user is able to change either key file, or to read your private key. 

More details on file permissions will be covered later in the course. 

## Copy the SSH Public Key to the Remote Host

In order for the host to allow your private key to connect you need to give it your public key. The public key needs to go in the `authorized_keys` file of your user's home directory (ex: /home/{user}/.ssh/authorized_keys). The file then needs to be read/writable for the user only (600). 

The `ssh-copy-id` cli will automatically do all of this for you.

We will start with the `webserver` host, followed by repeating the same steps on `pki-server`. 

Copy the key using the `ssh-copy-id` command

```bash
ssh-copy-id pluser@webserver
```

- Note that your `~/.ssh/config` file already specifies that `pluser` should be used when connecting to the `webserver` host. Therefore, it is not necessary to specify `pluser` when running ssh-copy-id.

You will then be prompted to enter `pluser`'s password. 

## Login to the remote Host without a password

Now that your public key has been added to the host we can login.

```bash
ssh webserver
```

Now you can connect to the server without a password.

You can verify the `authorized_keys` file

```bash
ls -l ~/.ssh/authorized_keys
cat ~/.ssh/authorized_keys
```

The most recently added key at the bottom of this file will match the contents of your public key in your `~/.ssh` directory. 

## Disable Password Login

We can enforce the use of SSH Keys by disabling password authentication, as recommended by the NIST standard.

Edit the sshd config file

```bash
sudo vi /etc/ssh/sshd_config
```

Find the line for `PasswordAuthentication` and set it to `no`

```text
PasswordAuthentication no
```

## Repeat on pki-server

On `pki-server`, repeat the same steps as completed on `webserver`: 

- Copy the ssh key to the host
- Test that you are able to login
- Disable password login

## Attempt password login

On your `workstation` host, move your private ssh key out of your `.ssh` folder

```bash
mv ~/.ssh/id_ed25519 ~/
```

Now, try to SSH to either `webserver` or `pki-server`. You will get the following error:

```text
pluser@172.16.1.128: Permission denied (publickey).
```

You are no longer given the option to connect using a password. Since the only option now is key-based authentication, you are not able to connect to the server. 

Move your private key back to the `.ssh` folder to be able to connect to the VMs again. 

```bash
mv ~/id_ed25519 ~/.ssh/
```

SSH to `webserver` or `pki-server`. You will not be able to connect again. 

## Additional Information

SSH (Secure Shell or Secure Socket Shell) is a network protocol that gives users - particularly systems administrators - a secure way to access a computer over an unsecured network.

In addition to providing strong encryption, SSH is widely used by network administrators to manage systems and applications remotely, enabling them to log in to another computer over a network, execute commands and move files from one computer to another.

SSH runs on the TCP/IP protocol suite in a client-server architecture. SSH provides encryption and authentication using public key cryptography. The most basic use of SSH is to connect to a remote host for a terminal session.
