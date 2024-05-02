# Instructions

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/games` present on `Storage server` in `Stratos DC`.
 One of the developers mistakenly created a couple of files under this repository, but now they want to clean this repository without adding/pushing any new files. Find below more details:

Clean the `/usr/src/kodekloudrepos/games` git repository without adding/pushing any new files, make sure `git status` is clean.


# Solution

`ssh natasha@ststor01`

`sudo su -`

`cd /usr/src/kodekloudrepos/games`

Check the status of the repository:`git status`

Discard all changes and untracked files:

```bash
[root@ststor01 games]# git reset --hard HEAD
HEAD is now at d832509 add data.txt file
```

Discard all untracked files:`git clean -f`

Verify the repository status: `git status`

```bash
[root@ststor01 games]# git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```
