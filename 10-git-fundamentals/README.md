# Git Fundamentals

## Prerequisites

Before using Git, check that it is installed and learn about commands available for troubleshooting. This section covers verifying the installation and accessing Git’s built-in help.

### Checking Git Version

To verify that Git is installed and to see which version is on your system, use the following command:

```bash
git --version
```

**Example Output:** `git version 2.34.1`
This command outputs the currently installed Git version, such as 2.34.1. If Git is not installed, it will prompt you to install it.

## Git Config

Before starting to work with repositories, it’s important to configure your Git settings. The `git config` command customizes Git’s behavior, such as setting your username and email, which are used to identify the author of each commit.

### Setting up Git Configurations

Below are the most common options for configuring Git. Run these commands from any directory.

1. Global or Local Configuration

    Git configurations can be set globally (for all repositories) or locally (for the current repository only).

        - Global: Use the `--global` flag to set a configuration that applies to all repositories on your machine.
        - Local: Omitting the `--global` flag sets the configuration for the current repository only.

    ```bash
    git config --global <option> <value>
    git config <option> <value>   # Sets for the current repository only
    ```

2. Setting Username and Email

    To identify yourself in commits, set your username and email:

    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "yourname@example.com"
    ```

    user.name
    : Specifies your name, which will appear in the commit history.

    user.email
    : Sets your email address, which is attached to each commit.

3. Viewing Configuration Settings

    To check your current Git configuration, use:

    ```bash
    git config --list
    ```

    This command lists all settings, including username, email, and other configurations.

4. Core Editor

    Git uses a default text editor for writing commit messages. By default, this is often vim on Linux, but you can change it with the `core.editor` option. For example, to set vim as the editor:

    ```bash
    git config --global core.editor "vim"
    ```

    core.editor
    : Sets your preferred text editor for commit messages and other Git operations.

## Essential Git Commands

| Git Command | Explanation |
| :--------- | ------- |
| `git init` | Initializes a new Git Repository in the current directory |
| `git status` | Shows the current status of the repository, including staged, unstaged, and untracked files. |
| `git clone <respository_url>` | Clonse a repository from a remote source to your local machine. |
| `git add <file_or_directory>` | Adds specified files or directories to the staging area, preparing them for a commit. Use `.`  to add all changes in the directory. |
| `git commit -m "commit message"` | Commits staged changes to the repository with a descriptive message. |
| `git push <remote> <branch>` | Pushes local changes to a remote repository. Typically, the remote is `origin`, and the branch is `main` or `master`. |
| `git pull <remote> <branch>` | Fetches changes from a remote repository and merges them into the current branch. |


## Cloning a Repository

To start, on server1, clone this repository

```bash
git clone https://github.com/PureLogicIT/101-linux-certificates
```

It will create a directory called `101-linux-certificates-git`. Change to that directory

```bash
cd 101-linux-certificates
```

Checkout the branch called, `git-example`

```bash
git checkout git-example
```

There should be two mew files, `index.html` and `default.conf`.

In `index.html` Set a welcome message to use for your website later

```html
<!doctype html>
  <html>
    <head>
      <meta charset="utf-8">
      <title>Tutorial</title>
    </head>
  <body>
    <h1>WELCOME MESSAGE HERE</h1>
    <p>We have just configured our Nginx web server on Ubuntu Server!</p>
  </body>
</html>
```

After making that edit, you can use git to see changes that happened.

```bash
git status

On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
        modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

You can inspect the specific changes of that file.

```
git diff index.html
```

Next we'll include that file in our commit

```bash
git add index.html
```

You can see the staged file.

```bash
git status

On branch master
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html
```

We'll need to set a description message to this commit. You should add a useful message detailing the changes of the commit. 

```bash
git commit -m "Set a welcome message"
```

Again you can check status of the change.

```bash
git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
    (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Then simply push your local changes which have been commited to the remote repo.

```bash
git push
Username for remoterepo: <username>
Password for remoterepo:
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 923 bytes | 923.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0), pack-reused 0
remote: . Processing 1 references
remote: Processed 1 references in total
To remoterepo/ILTrainingCourses/101-linux-certificates-git.git
   c2f983d..801fee5  master -> master
```

## Branching and Merging

| Git Command | Explanation |
| :--------- | ------- |
| `git branch` | Lists all the branches in the repository. |
| `git branch <new-feature>` | Add a branch name to create a new branch. |
| `git checkout <new-feature>` | Switches to the specified branch. Added a `-b` creates and switches to the new branch. |
| `git merge <branch>` | Merges the specified branch into the current branch. |
| `git branch -d <branch>` | Deletes the specified branch. Will fail if there are unmerged changes. Use `-D` to force deletion. | 

## Undoing Changes

| Git Command | Explanation |
| :--------- | ------- |
| `git reset <file>` | Unstages a file withou discarding the changes. |
| `git checkout --` <file> | Discards changes in a file by restoring it to the last committed version. |
| `git revert a1b2c3d4` | Creates a new commit that undoes the changes from a specified commit. |
| `git reset --hard a1b2c3d4` | Resets the current branch to a specified commit and discards all changes since that commit. |

## Viewing History and Logs

| Git Command | Explanation |
| :--------- | ------- |
| `git log` | Shows the commit history for the repository. Use `--oneline` for a simplfied view. |
| `git diff` | Shows differences between files in the working directory and the staging area. Add a file name to see changes for a specific file. |
| `git show <commit>` | Displays information and changes for a specified commit. |


## Working with Remote Repositories

| Git Command | Explanation |
| :--------- | ------- |
| `git remote -v` | Lists all remote connections for the repository. Each remote connection has a name and a URL. |
| `git remote add <name> <url>` | Adds a new remote repository with a specified name. |
| `git fetch <remote>` | Retrieves changes from a remote repository without merging them. |


## Additional Resources

For more on git config and other Git commands, consult the [Git documentation](https://git-scm.com/docs) or use `git --help` for further assistance.
