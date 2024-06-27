# Instructions

The Nautilus application development team has been working on a project repository `/opt/media.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`. They recently shared the following requirements with DevOps team:

One of the developers is working on `feature` branch and their work is still in progress, however there are some changes which have been pushed into the `master` branch, the developer now wants to `rebase` the `feature` branch with the `master` branch without loosing any data from the `feature` branch, also they don't want to add any `merge commit` by simply merging the `master` branch into the `feature` branch. Accomplish this task as per requirements mentioned.

Also remember to push your changes once done.

# Solution

`ssh natasha@ststor01`

`cd /usr/src/kodekloudrepos/media`

`git status`

- This command displays the current status of the Git repository, including the active branch, any modified files, and any untracked files.

`sudo git checkout feature-branch`

- This command switches the active branch to the "feature-branch" branch.

`sudo git rebase master feature`

- This command performs a rebase operation of the "feature-branch" branch with the "master" branch.

`sudo git push origin feature`

- This command pushes the "feature-branch" branch to the remote repository (origin).

`sudo git pull origin feature`

- This command pulls the latest changes from the "feature-branch" branch in the remote repository (origin).

`sudo git config pull.rebase true`

- This command sets the pull.rebase configuration option to true for the Git repository.

`sudo git pull origin feature`

- This command pulls the latest changes from the "feature-branch" branch in the remote repository (origin), and automatically rebases your local branch on top of the remote branch.

`sudo git push origin feature`

- This command pushes the "feature-branch" branch, with the rebase changes, to the remote repository (origin).
