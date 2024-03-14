# Instructions

The Nautilus application development team has been working on a project repository `/opt/news.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`.  They recently shared the following requirements with DevOps team:

Create a new branch `xfusion` in `/usr/src/kodekloudrepos/news` repo from `master` and copy the `/tmp/index.html` file (present on `storage server` itself) into the repo. Further, `add/commit` this file in the new branch and merge back that branch into `master` branch. Finally, push the changes to the origin for both of the branches.

# Solution

`ssh natasha@ststor01`

`sudo su -`

`cd /usr/src/kodekloudrepos/news`

`git status`

`git checkout -b xfusion`

`git status`

`git branch`

`cp /tmp/index.html /usr/src/kodekloudrepos/news/`

`git add index.html`

`git commit -m "add index.html”`

`git checkout master`

This command merges the changes from the xfusion branch into the current branch (which is assumed to be master in this case): `git merge xfusion`

This command pushes the local xfusion branch to the remote repository specified by origin. The -u flag is used to set the upstream branch, so that in the future, you can simply run git push without specifying the branch name. This command sends the commits made on the local xfusion branch to the remote repository, making them available to other collaborators: `git push -u origin xfusion`

This command pushes the local master branch to the remote repository specified by origin. Similar to the previous command, the -u flag sets the upstream branch, allowing you to use git push without specifying the branch name in the future. By running this command, the commits made on the local master branch are sent to the remote repository, making them accessible to other team members: `git push -u origin master`

`git status`
