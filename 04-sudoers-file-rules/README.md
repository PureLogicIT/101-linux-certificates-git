# Sudoers File Config

The sudoers file is used to configure the `sudo` privileges for users and groups on a Unix-like system (Linux, macOS, etc.). It defines which users or groups can run which commands as which other users (usually root) and under which conditions.

## Modifying sudoers file

On `webserver`, complete the following changes. 

When modifying the sudoers file, it's best practice to use the `visudo` cli. It will validate the syntax of the sudoers file before saving to prevent breaking sudo access to the server. If the sudoers file is saved with a typo, this may cause no users to be able to have sudo access. 

```bash
sudo visudo
```

- `visudo` uses nano by default on this version of Ubuntu. CTRL+X, then Y, then ENTER to save the changes

Add the following line to the bottom of the file.

```ini
%courseadmin  ALL=(ALL) NOPASSWD: /usr/bin/apt-get
```

- `%courseadmin`: The group that will be granted sudo privileges. The `%` represents a group.
- `ALL`: Applies to all hosts (can be restricted to specific hosts).
- `(ALL)`: The user can execute commands as any user, including root.
- `NOPASSWD:`: Will not prompt for a password (optional)
- `/usr/bin/apt-get`: The commands that will be allowed to run as root (comma separated list)

Running `visudo` without any flags will modify the global sudoers file.

To view the sudoers file, run `cat /etc/sudoers`. This will show the same content as we saw using `visudo`. Do not edit the `/etc/sudoers` file directly. 

## Sudoers file rule priority

The `sudoers` file is parsed from top to bottom. If a user matches multiple lines, the last rule in the file is the one that will be applied. 

```text
# Give pluser full root access
pluser ALL=(ALL) ALL

...

# Restrict everyone in the courseadmin group (which pluser belongs to) to only have sudo access to apt-get
%courseadmin ALL=(ALL) /usr/bin/apt-get
```

In the example above, even though we added a rule giving `pluser` full access, the later rule for the `courseadmin` group overrides this, and gives `pluser` sudo access to only `apt-get`. 

## Visudo safety net

If while editing the sudoers file using `visudo` the file contains an error that makes it invalid, `visudo` will prompt you, allow you to fix or discard your change. 

Use `visudo` to edit the `sudoers` file and add a mistake to one of the rules. 

```bash
sudo visudo
```

After you have added a mistake to a rule, CTRL + X, Y, then ENTER to save your changes. 

```text
/etc/sudoers:59:18: syntax error
%courseadmin  ALL(ALL) NOPASSWD: /usr/bin/apt-get
                 ^
What now?
```

You have the following options available (this can be seen be pressing enter to the `What now?` prompt)

```text
Options are:
  (e)dit sudoers file again
  e(x)it without saving changes to sudoers file
  (Q)uit and save changes to sudoers file (DANGER!)
```

Press `x` to discard your changes. 

## Sudo rule update effect

The rule we added to the `sudoers` file specified that the `apt-get` command can be run without the user's password. 

First, logout then login to `webserver`. 

Then, run the following command, you will not be prompted for your passsword. 

```bash
sudo apt-get update
```

But running the following, you will be prompted to enter your password, since the `apt` command was not covered by the rule we added. 

```bash
sudo apt update
```

When adding rules to the `sudoers` file, it is important to always add full absolute paths (like `/user/bin/apt-get`, and not `apt-get`). This prevents a user from placing a different script at `apt-get` and tricking sudo into running it, which can't happen if the full path (`/user/bin/apt-get`) is specified. 

## Dedicated sudo policy file

You can also create individual sudoers files for users or groups, you can modify those with the `-f` option with the cli. This is better practice than modifying the global sudoers file like we did before. 

Note that the `ansible` filename must not contains a `.` (period). 

```bash
sudo visudo -f /etc/sudoers.d/ansible
```

The choice of `ansible` here does not have any effect, we choose it to make it easier for to remember what user the file is for. 

Add the following rule to the file. 

```ini
ansibleuser  ALL=(ALL) ALL
```

- `ansibleuser`: The user who is being granted sudo privileges.
- `ALL`: Applies to all hosts (can be restricted to specific hosts).
- `(ALL)`: The user can execute commands as any user, including root.
- `ALL`: The user can run all commands.

When finished, repeat the **Modifying sudoers file** and **Dedicated sudo policy file** sections on `pki-server`. 
