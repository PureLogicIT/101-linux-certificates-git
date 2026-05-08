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

Now, we'll create a new branch locally, and publish our changes to Gitea. 



In gitea:
 Create repo
 Add SSH Keys

In terminal:
Add remote
Push
cd ..
rm -rf git-repo
git clone git://git.dev....
cd git-repo
git branch new-branch
git add
git commit 
git pull
git push

In gitea:
See that only master and new branch exist
Create pull request
Approve Pull request 
Talk about merge type

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