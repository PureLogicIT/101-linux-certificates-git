# Git Fundamentals

## Prerequisites

Before using Git, check that it is installed and learn about commands available for troubleshooting. This section covers verifying the installation and accessing Git’s built-in help.

### Checking Git Version

To verify that Git is installed and to see which version is on your system, use the following command:

```bash
git --version
```

Example output: `git version 2.34.1`

This command outputs the currently installed Git version, such as 2.34.1. If Git is not installed, it will prompt you to install it.

### Help and Troubleshooting

For an overview of top level git commands. 

```bash
git -h
```

To view a summary of a specific command, such as `git commit`. 

```bash
git commit -h
```

Or for a more in-depth view

```bash
git commit --help
```

## Git Config

Before starting to work with repositories, it’s important to configure your Git settings. The `git config` command customizes Git’s behaviour, such as setting your username and email, which are used to identify the author of each commit.

To start, check that there are no config settings already set. 

```bash
git config --list
```

### Setting up Git Configurations

Below are the most common options for configuring Git. Run these commands from any directory.

1. Global or Local Configuration

    Git configurations can be set globally (for all repositories) or locally (for the current repository only).

    Global: Use the `--global` flag to set a configuration that applies to all repositories on your machine.

    ```bash
    git config --global <option> <value>
    ```

    Local: Omit the `--global` flag to set the configuration for the current repository only.

    ```bash
    git config <option> <value>
    ```

2. Setting Username and Email

    To identify yourself in commits, set your username and email:

    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "yourname@domain.com"
    ```

    - `user.name`: Specifies your name, which will appear in the commit history.
    - `user.email`: Sets your email address, which is attached to each commit.

    These do not need to be your real name or email, they are only set to help as documentation. 

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

    - `core.editor`: Sets your preferred text editor for commit messages and other Git operations.

## Essential Git Commands

| Git Command | Explanation |
| :--------- | ------- |
| `git init` | Initializes a new Git Repository in the current directory |
| `git status` | Shows the current status of the repository, including staged, unstaged, and untracked files. |
| `git clone <repository_url>` | Clones a repository from a remote source to your local machine. |
| `git add <file_or_directory>` | Adds specified files or directories to the staging area, preparing them for a commit. Use `.`  to add all changes in the directory. |
| `git commit -m "commit message"` | Commits staged changes to the repository with a descriptive message. |
| `git push <remote> <branch>` | Pushes local changes to a remote repository. Typically, the remote is `origin`, and the branch is `main` or `master`. |
| `git pull <remote> <branch>` | Fetches changes from a remote repository and merges them into the current branch. |

## Creating a new repository

Start by creating a new folder called `git-repo`.

```bash
mkdir git-repo
cd git-repo
```

Create a file called `README.md` with the following header.

```bash
echo "# Git Repo " > README.md
```

The first thing you need to start tracking any project in git is initialize it. This can be done at anytime and changes will be tracked from that point on.

```bash
git init
```

The following message appears. 

```text
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
```

The git CLI uses `master` as the default name for the initial branch. Sites like Github or Gitlab have `main` as their default branch name. Outside of this course, you should set your default branch name to match what your organization uses, following the instructions above. 

Now that the folder is tracked with git you can look at the `status`

```bash
git status
```

```bash
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

In the above output, you can see that there's a file that isn't yet tracked by git, because it's new.

You can `add` that file to your commit

```bash
git add README.md
```

Running `status` again will show that it is now staged and will be included in your next commit

```bash
git status
```

```bash
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

`git status` now shows that the file has been added, but has yet to be committed. 

We can now `git commit` this as our first commit. 

```bash
git commit -m "Initial commit"
```

- `-m` is for the commit message. If it is not specified, it will launch your default editor  for you to write a commit message.

Running `status` again will show that there are no untracked changes

```bash
git status
```

```bash
On branch master
nothing to commit, working tree clean
```

After committing our changes, we may notice something broke and want to check what changes just got made. To do this, view the commit log. 

```bash
git log
```

This log shows a commit SHA (or hash) for each commit, as well as the author information that you set earlier, the commit date, and message. 

Copy the commit SHA and use it to check more details about the commit. 

```bash
git show {commit SHA}
```

- Note that only the first few (usually 7) characters are used instead of the entire SHA. 

This allows us to see some more information related to the commit, included at least some of the changes that were made. 

Edit the `README.md` file and add a second line. 

```bash
vim README.md
```

```text
# Git Repo
A new change
```

Checking git status shows that `README.md` has been modified, but not committed, which is what we expect. 

```bash
git status
```

You see the file tagged as being modified. We can see exactly what has been modified by running a `diff`

```bash
git diff
```

```text
diff --git a/README.md b/README.md
index 0b86574..f5b70f7 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,2 @@
-# Git Example
+# Git Example 2
+Something
```

Add the file and commit the changes

```bash
git add .
git commit
```

- `git add .` adds all files in the current directory and all subdirectories. 

We ran `git commit` without specifying a commit message. When this happens, git opens your configured default text editor, and lets you write a commit message there instead. 

Enter the commit message `Updated README file` and save and close the file. 

Now we can look at the history of changes and see both commits. 

```bash
git log
```

```text
commit 4f77676cc0d52de6bebb17a6e39d4194886b59c3 (HEAD -> master)
Author: Your Name <you@domain.com>
Date:   Thu Jun 19 18:08:18 2025 +0000

    Updated README file

commit 8f9e6608b62d47e0c0d96b134473869895a3523a
Author: Your Name <you@domain.com>
Date:   Thu Jun 19 17:43:02 2025 +0000

    Initial commit
```

The changes are in chronological order, with the latest commit at the top.

If you want to undo the change you just did because it broke something. Take the commit number of the latest change and `revert` it. 

```bash
git revert 4f77676cc0d52de6bebb17a6e39d4194886b59c3
```

Save and close the text editor that is opened. 

Running `git log` again you'll see it created a third commit saying it was reverting the second one. `git revert` doesn't actually delete the commit from history.

Check the contents of the `README.md` file. 

```bash
cat README.md
```

The file should now only contain `# Git Repo`. 

## Git Branches

Working directly on the `master` branch is considered bad practice in professional environments. The `master` branch should always represent a stable, deployable version of the project. Whenever a new feature or bug fix needs to be worked on, you should do so on a dedicated branch. This allows you to make your changes in isolation without breaking the main project. 

Branches should be named descriptively so they can be tracked back to a specific task or issue number (feature/issue-101-update-readme or fix/bug-245-login-page-error)

Create a new branch

```bash
git branch feature-update-readme
```

View the list of branches. 

```bash
git branch
```

Change to the new branch. 

```bash
git checkout feature-update-readme
```

To view the branch that you are currently on. 

```bash
git status
```

Make another edit to the `README.md` file. 

```text
# Git Repo on branch feature-update-readme
```

`git status` to see what has changed. 

`git diff` to see the text added to the `README.md` file

Add the change, commit it, then check the log. 

```bash
git add -A
git commit -m "Changes on feature-update-readme"
git log
```

- `git add -A`: Adds all files and directories within the git directory, not only within your current directory. `git add .` would have had the same effect in this case, but not always. 

Now that we've made our change on the feature branch, we want to bring that change back to the `master` branch. 

To do this, we will **merge** our feature branch into the `master` branch. To start, change to the `master` branch. 

```bash
git checkout master
git log
```

When you run `git log` on the master branch, you will notice that the commit you just made on your feature branch is missing. The changes exist, but they haven't been integrated into the `master` history yet. 

Now, merge the changes made on the feature branch into the `master` branch. 

```bash
git merge feature-update-readme
```

Accept the suggested commit message. 

View the git log to see your merge in the commit history. 

```bash
git log
git status
```

## Merge Conflicts

A merge conflict is what occurs when Git is not able to automatically merge the differences between two branches. Instead of trying to guess at which change is most important, Git allows you to step in and make the decision on which changes to keep, and which to discard. 

Edit the README.md file on `master` to the following:

```text
# Git Repo
A second line
A third line
```

Add and commit the file. 

```bash
git add README.md
git commit -m "Updated README file on master"
```

Next, create and checkout a new branch called `feature-readme-conflict`. 

```bash
git checkout -b feature-readme-conflict
```

- `-b` (Branch): Adding this option checks out the branch after it's created. 

Edit the README.md file on `feature-readme-conflict` to match the following. 

```text
# Git Repo this will conflict
A second line
A third line
This will not conflict
```

Stage the `README.md` file. 

```bash
git add README.md
```

Commit the file

```bash
git commit -m "Updated README file on feature-readme-conflict branch"
```

Change back to the `master branch`. This time, we will also make changes to the README file on the master branch to cause a merge conflict. 

```bash
git checkout master
```

Open the `README.md` file and update it to the following. 

```text
# Git Repo on branch master
A second line
This line will not conflict
A third line
```

Stage and commit the changes. 

```bash
git add README.md
git commit -m "Updated README file on master with second change"
```

Before merging the feature branch into `master`, check how the two branches have diverged. 

```bash
git log --oneline
```

Look on the feature branch. 

```bash
git checkout feature-readme-conflict
git log --oneline
```

Return back to master and attempt to merge the feature branch. 

```bash
git checkout master
git merge feature-readme-conflict
```

Git will try to automatically merge the `README.md` files from both branches, but will not be able to. 

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

You now have to manually fix the merge conflicts. To do so, open the `README.md` file. 

```text
<<<<<<< HEAD
# Git Repo on branch master
=======
# Git Repo this will conflict
>>>>>>> feature-readme-conflict
A second line
This line will not conflict
A third line
This will not conflict
```

- `<<<<<<< HEAD`: Everything below this is what currently exists on master.
- `=======`: This is the divider between the two conflicting versions.
- `>>>>>>> feature-readme-conflict`: Everything above this is what is coming from the feature branch.
- The content outside of those marks was present on one of the two branches, and was merged together automatically. 

To solve the merge conflict, you will have to manually edit the file. 

Delete the markers (<<<, ===, >>>), and decide which text to keep. 

Edit the `README.md` file to match the following

```text
# Git Repo on branch master
A second line
This line will not conflict
A third line
This will not conflict
```

```bash
git add .
git status
```

Now that you have solve all merge conflicts, commit your changes on `master`. 

```bash
git commit
```

Even though we didn't specify a commit message, git knows that we are committing our merge, and suggests a simple commit message. 

Save and quit the text editor to accept this commit message. 

On master, we can look at the git log to see the updated history. 

```bash
git log
```

This now shows the commit that was made on the feature branch, followed by our merge commit on the `master` branch. 

Take a look at the git log on the feature branch as well. 

```bash
git checkout feature-readme-conflict
git log
```

As we'd expect, there is nothing more past the commit we made on the feature branch, as the merge was done into the master branch, and did not affect the feature branch. 

We are now done with the feature branch, as all of it's changes have been merged into master. 

Tagging the branch creates a pointer to a commit that doesn't move. After tagging the branch, we can then delete the branch itself. 

```bash
git tag archive/feature-readme-conflict feature-readme-conflict
git branch -d feature-readme-conflict
```

# REMOVE THIS eventually, but needs to happen somewhere else

## Cloning a Repository

To start, on `webserver`, clone this repository

```bash
git clone https://github.com/PureLogicIT/101-linux-certificates-git
```

It will create a directory called `101-linux-certificates-git`. Change to the new directory

```bash
cd 101-linux-certificates-git
```

Checkout the branch called `git-example`

```bash
git checkout git-example
```

There should be two new files, `index.html` and `tutorial.conf`.

In `index.html`, set a welcome message in place of `WELCOME MESSAGE HERE` to use for your website later

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

You can inspect the specific changes that were made to the file.

```bash
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

Add a useful message detailing the changes of the commit.

```bash
git commit -m "Set a welcome message"
```

Again you can check the status of the change.

```bash
git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
    (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Then simply push your local changes which have been committed to the remote repo.

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
| `git reset <file>` | Unstages a file without discarding the changes. |
| `git checkout -- <file>` | Discards changes in a file by restoring it to the last committed version. |
| `git revert a1b2c3d4` | Creates a new commit that undoes the changes from a specified commit. |
| `git reset --hard a1b2c3d4` | Resets the current branch to a specified commit and discards all changes since that commit. |

## Viewing History and Logs

| Git Command | Explanation |
| :--------- | ------- |
| `git log` | Shows the commit history for the repository. Use `--oneline` for a simplified view. |
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
