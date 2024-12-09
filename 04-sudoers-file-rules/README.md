# Sudoers File Config

The sudoers file is used to configure the `sudo` privileges for users and groups on a Unix-like system (Linux, macOS, etc.). It defines which users or groups can run which commands as which other users (usually root) and under which conditions.


## Modifying sudoers file

### visudo

When modifying the sudoers file it's always best practice to use the `visudo` cli. It will validate the syntax of the sudoers file before saving to prevent breaking sudo access to the server.


```bash
visudo
```

Add the following line to the bottom of the file
```ini
%courseadmin  ALL=(ALL) NOPASSWD: /usr/bin/apt-get
```
- %courseadmin: The group that will be granted sudo privileges. The `%` represents a group
- ALL: Applies to all hosts (can be restricted to specific hosts).
- (ALL): The user can execute commands as any user, including root.
- NOPASSWD: Will not prompt for a password (optional)
- /usr/bin/apt-get: The commands that will be allowed to run as root (comma seperated list)

Running `visudo` without any flags will modify the global sudoers file.

You can also create idividual sudoers files for users or groups, you can modify those with the `-f` option with the cli

```bash
visudo -f /etc/sudoers.d/ansible
```

For ansible we will set the following config
```ini
ansibleuser  ALL=(ALL) ALL
```
- ansibleuser: The user who is being granted sudo privileges.
- ALL: Applies to all hosts (can be restricted to specific hosts).
- (ALL): The user can execute commands as any user, including root.
- ALL: The user can run all commands.


