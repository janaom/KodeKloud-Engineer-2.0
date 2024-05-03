# Instructions

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/media` present on `Storage server` in `Stratos DC`.
 One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. 
Find below more details to accomplish this task:

Look for the stashed changes under `/usr/src/kodekloudrepos/media` git repository, and restore the stash with `stash@{1}` identifier. Further, commit and push your changes to the origin.

# Solution

`ssh natasha@ststor01`

`sudo su -`

`cd /usr/src/kodekloudrepos/media`

List the stashed changes: `git stash list`

This command will display a list of stashed changes, each with a unique identifier. Identify the stash that you want to restore based on the description or timestamp.

```bash
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/media
[root@ststor01 media]# git stash list
stash@{0}: WIP on master: be0e04c initial commit
stash@{1}: WIP on master: be0e04c initial commit
[root@ststor01 media]#
```

Restore the desired stash: `git stash apply stash@{1}`

```bash
[root@ststor01 media]# git stash apply stash@{1}
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   welcome.txt

[root@ststor01 media]#
```

Verify the changes: `git status`

Commit the changes: `git commit -m "Restored stashed changes”`

Push the changes to the origin: `git push origin master`
