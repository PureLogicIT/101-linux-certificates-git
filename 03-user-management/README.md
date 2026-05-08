# User Management

## Creating a user

Let's create a user named `ansibleuser` with the default home directory.

### Adding a user with useradd

On `webserver`, we'll use the `useradd` cli. While many modern distributions provide interactive wrappers to this command, `useradd` is found in virtually every version of linux. For cases where `useradd` is the only option available, we'll go over how to use it. 

Run the following command on `webserver` to add the new user. 

```bash
sudo useradd -m ansibleuser
```

- `-m` creates a home directory for the user in the default location. 

The `-m` option is standard because it ensures the environment for the user is ready to be used when created. For our purposes, we would not be able to SSH to the user on the host if they didn't have a home directory, since SSH requires a place to put the `.ssh/authorized_keys` file.

To check if the user has been created, run the following command. 

```bash
sudo cat /etc/passwd
```

You will see the following line at the bottom of the output, confirming that the user was created. 

```text
ansibleuser:x:1002:1002::/home/ansibleuser:/bin/sh
```

Each line is structured as follows:

```text
<username>:<placeholder for password>:<UID>:<GID>:<Full Name,Room Number,Work Phone,Home Phone>:<home directory>:<shell>
```

Looking at our output, we can see that the user has a home directory of `/home/ansibleuser`, and shell `/bin/sh`. 

### Set a password a user

Change the password for `ansibleuser`

```bash
sudo passwd ansibleuser
```

### Add a user using adduser

On `pki-server`, we can use the more user friendly `adduser` cli. It will automatically create a home directory and group memberships and prompt for a password to be set. `adduser` acts as a wrapper for `useradd`, making it much easier to use if it is available. 

```bash
sudo adduser ansibleuser

Adding user `ansibleuser' ...
Adding new group `ansibleuser' (1004) ...
Adding new user `ansibleuser' (1004) with group `ansibleuser' ...
Creating home directory `/home/ansibleuser' ...
Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for ansibleuser
Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:

        Other []:
Is the information correct? [Y/n] y
```

Now, check again if the user was created.

```bash
sudo cat /etc/passwd
```

This time, you will see an output like the one below:

```text
ansibleuser:x:1002:1002::/home/ansibleuser:/bin/bash
```

Without having to specify it, a home directory was created. The shell was also set to `/bin/bash`, which is what we want. 

Overall, the `adduser` command is easier to use, and has less to remember when creating a user. 

## Create a group

Permissions can be assigned to individual users or to groups.

Create a group that will be used in a later lesson.

On **both** `webserver` and `pki-server`, create the group

```bash
sudo groupadd courseadmin
```

### Add user to group with usermod

On `webserver`, add the `pluser` user to the new `courseadmin` group

```bash
sudo usermod -aG courseadmin pluser
```

- `-a` to append the user to the groups mentioned after -G without removing the user from other groups
- `-G` specifies the group to add, and that the group is not the user's primary group

To view info related to `pluser`

```bash
id pluser
```

Output:

```text
uid=1001(pluser) gid=1001(pluser) groups=1001(pluser),27(sudo),100(users),1003(courseadmin)
```

From this output, we can see that `pluser` was added to the `courseadmin` group. 

### Add user to group with adduser

You can also use the adduser command to add a user to a group

On the `pki-server` run the following command

```bash
sudo adduser pluser courseadmin
```

Using `adduser`, we no longer need to specify any extra option, making the process much simpler. 

```bash
id pluser
```

The output matches the same as when we used the `usermod` command. 

Another place to view group members is in `/etc/group`. 

```bash
cat /etc/group
```

In the output, you'll see the following line. 

```text
courseadmin:x:1003:pluser
```

This also shows that `pluser` was successfully added to the `courseadmin` group. 

## Move the home directory for a user

This is not something we need to do for the course but might be needed eventually.

A user's home directory can be changed at any time.

```bash
sudo usermod -m -d /usr/testuser testuser
```

- `-m` will move the existing content, if any, from the current home folder to the new home folder, /usr/testuser, for the user 'testuser'

You see every user's current home directory in the `passwd` file

```bash
cat /etc/passwd
```

`/etc/passwd`:

```text
...
pluser:x:1001:1001:,,,:/home/pluser:/bin/bash
...
```

Each line is structured as follows:

```text
<username>:<placeholder for password>:<UID>:<GID>:<Full Name,Room Number,Work Phone,Home Phone>:<home directory>:<shell>
```

## Change a user's shell

The default shell on Ubuntu is `bash` but users can use many different shells. You may encounter different environments where your preferred shell isn't installed, and if so, you'll need to check what shells are available and change your user's shell. 

To set a user's default shell, we can use the `chsh` command. We first need to know which shells are installed on the server.

```bash
cat /etc/shells
```

This will show a list of installed shells. 

Copy the full path of the shell you want to set. 

On `webserver`, change the shell for `ansibleuser`. 

Set the shell to `/bin/bash`.

```bash
sudo chsh ansibleuser

Changing the login shell for ansibleuser
Enter the new value, or press ENTER for the default
        Login Shell [/bin/sh]: /bin/bash
```

Every user's current shell is also listed in the `passwd` file. 

```bash
cat /etc/passwd
```

## Add SSH access to ansibleuser

Earlier, when you created the `ansibleuser` user, a `.ssh` directory for `ansibleuser` was not created. We will need this directory to exist and contain our `workstation` public key to be able to ssh to `ansibleuser`. 

On `webserver`, create the `.ssh` directory for `ansibleuser`. 

```bash
sudo mkdir -p ~ansibleuser/.ssh/
```

Then, copy the authorized keys file from the `pluser` .ssh directory to `ansibleuser`. 

```bash
sudo cp .ssh/authorized_keys ~ansibleuser/.ssh/
```

Finally, make sure that `.ssh` directory is owned by `ansibleuser`. 

```bash
sudo chown -R ansibleuser: ~ansibleuser/.ssh/
```

Repeat the same steps on `pki-server`. 

This will allow us to ssh directory to the `ansibleuser` on both hosts. 

Make sure you are able to ssh to both hosts from `workstation`. 

```bash
ssh ansibleuser@webserver
```

```bash
ssh ansibleuser@pki-server
```
