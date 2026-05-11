# Gitea - Remote Repositories

Git hosting platforms, such as github and gitlab, provide ... This allows you to have a central place to store your code, off your computer, and be able to share and collaborate with others. 

Gitea is a lightweight alternative to other git providers, giving an easy and fast way to self-host a git instance.

## Initial setup

Before we can move our code off the local workstation and into the web, we need to set up a destination. 

Login to gitea at [git.dev.purelogicit.ca](git.dev.purelogicit.ca)

Navigate to your account settings, and open the `SSH / GPG Keys` section. 

Under `Manage SSH Keys`, add a key. 

On `workstation`, copy the contents of your public key. 

```bash
cat ~/.ssh/id_ed25519.pub
```

Paste this into the content section on gitea. Save the key. 

To test if you are able to connect to the gitea instance through SSH, run the following command. 

```bash
ssh -T git@git.dev.purelogicit.ca
```

The output should confirm that you were able to authenticate with the same key that you just added to Gitea. 

Now, create a new repository in Gitea. 

On `workstation`, make sure you are in the `~/git-repo` directory. 

Add your Gitea repository as a remote. 

```bash
git remote add origin git@git.dev.purelogicit.ca:{your username}/{your repository name}.git
```

- Note that this line can also be copied from the gitea page where 

Now, git push will publish your changes to the remote repository that was set above. 

```bash
git push -u origin master
```

- `-u`: This option tells git to match the local master branch with the master branch (origin) on the gitea server. 

Next time you `git push`, you'll no longer need to specify any extra options. 

Now that our code is saved in Gitea, we'll delete and restore our local copy of the repository. 

```bash
cd ..
rm -rf git-repo
```

The git repo is now deleted, so we'll `clone` it from gitea to restore our local copy. 

```bash
git clone git.dev.purelogicit.ca:{your username}/{your repository name}.git git-repo
cd git-repo
```

- The `git-repo` at the end of the `git clone` command specifies what the folder that will contain your git repository will be named. By default, this is the same name as that of the repository in Gitea. 

Now, we'll create a new branch locally, and publish that branch to Gitea. 

```bash
git checkout -b feature-branch
```

- `-b` will checkout the branch after it is created. 

Now, make a change on the new branch. Edit the `README.md` file to match the following. 

```text
# Git Repo on feature-branch

A second line

This line will not conflict

A third line

This will not conflict
```

Add and commit the file once your changes are saved. 

```bash
git add .
git commit -m "change on feature-branch for pull request"
```

After the change is committed, pull and push to Gitea. 

```bash
git push -u origin feature-branch
```

Both the `master` and `feature-branch` branches are now pushed to Gitea. View both branches on the gitea site. 

Navigate to the `Pull Requests` tab, and create a new pull request. 

Both the branches being merged into and being pulled from are set to master by default. Change the branch that is being pulled from (the one on the right) to your `feature-branch`. 

The diff view should now show our change that we made on `feature-branch`. 

Create the pull request. 

Now that the pull request is created, we want to approve it. Click the dropdown beside `Create merge commit` to view the different merge options. 

- `Merge commit`: Combines all branch work and adds a dedicated "merge commit," creating a visible "bubble" in your project history.
- `Rebase, then fast-forward`: Moves your commits to the very end of the master branch, resulting in a perfectly straight, linear timeline.
- `Rebase, then create merge commit`: Updates your work to the end of the line first, then adds a merge commit to mark exactly when it was integrated.
- `Create squash commit`: Adds every individual commit from your feature branch into one single, clean commit to keep the master history tidy.
- `Fast-forward only`: Only allows the merge if master hasn't changed since you started, ensuring a strictly linear path with zero extra merge commits.

Select and click `Create merge commit`. 

Delete `feature-branch`. It can still be restored by viewing the list of all branches in Gitea. 

## Rebase and squash merges



In terminal:
git branch another-new-branch
git add
git commit
git add
git commit
git add
git commit
git squash (2 commits total)
git checkout master
git rebase/merge another-new-branch
git pull
git push -A (--all-branches)
