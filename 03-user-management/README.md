# User Management

## Creating a user

For this course let's create a user named ansibleuser with the default home directory

### Adding a user with useradd

On the ***server1*** we'll use the more manual `useradd` cli 

```bash
sudo useradd -m ansibleuser
```

`-m` creates a home directory for the user in the default location

### Set a password a user

Change the password for ansibleuser

```bash
sudo passwd ansibleuser

New password: 
Retype new password:
passwd: password updated successfully
```

### Add a user using adduser

On ***server2***, we can use the more user friendly `adduser` cli. It will automatically create a home directory and group memberships and prompt for a password to be set.


```bash
sudo adduser anisbleuser

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
        Full Name []: Ansible User
        Room Number []:
        Work Phone []:
        Home Phone []:

        Other []:
Is the information correct? [Y/n] y
```

## Create a group

Permissions can be assigned to individual users or to groups.

Create a group that will be used in a later lesson

On ***both*** servers, create the group
```bash
sudo groupadd courseadmin
```

### Add user to group with usermod

On ***server1***, add the `pluser` user to the new `courseadmin` group
```bash
sudo usermod -aG cousadmin pluser
```

`-a` to append the user to supplemental GROUPS mentioned by the -G option without removing the user from other groups
`-G` specifies the group to add, and that the group is not the user's primary group

### Add user to group with usermod

You can also use the adduser command to add a user to a group

On the ***server2*** run the following command

```bash
sudo adduser pluser courseadmin
```

## Move the home directory for a user

This is not something we need to do for the course but might be needed eventually.

A user's home directory can be changed at any time.

```bash
sudo usermod -m -d /usr/testuser testuser
```

`-m` will move the existing content, if any, from the current home folder to the new home folder, /usr/testuser, for the user 'testuser'


You see every user's current home directory in the `passwd` file

```bash
cat /etc/passwd
```

*passwd*
```
...
pluser:x:1001:1001:,,,:/home/pluser:/bin/bash
...
```

```
<username>:<placeholder for password>:<UID>:<GID>:<Full Name,Room Number,Work Phone,Home Phone>:<home directory>:<shell>
```

## Change a user's shell

The default shell on Ubuntu is `bash` but user's can use many different shells.

To set a user's default shell, we can use the `chsh` commnand but first we need to know which shells are installed on the server

```bash
cat /etc/shells
```

Copy the full path of the shell you want to set

```bash
sudo chsh ansibleuser

Changing the login shell for ansibleuser
Enter the new value, or press ENTER for the default
        Login Shell [/bin/bash]: /bin/sh
```

Every user's current shell is also listed in the `passwd` file

```bash
cat /etc/passwd
```
