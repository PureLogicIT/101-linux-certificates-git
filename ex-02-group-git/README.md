# Git group conflict

person A and person B

One person creates a git repo on gitea, adds the other as a collaborator. 

both people follow "create a new repo on cmd line" instructions that show up in the new repo in gitea

Both people now go to edit the readme file, commit it and push it

someone will get their push blocked, have them need to merge it, then they get to push too

have the other person pull, then both of them look at `git log --graph --oneline --all`

---

## merge in gitea instead

whoever got to do the merge, go into gitea and create a branch protection rule on `main`

Other person, on terminal, create new branch, make change to readme, commit and push

then someone, idk who, goes to gitea creates an PR, adds the other as a reviewer

Review the PR, merge it. both go back to terminals and pull again
