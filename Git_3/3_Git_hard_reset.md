# Instructions

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/apps` present on `Storage server` in `Stratos DC`.
This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the `HEAD` and the branch itself to a commit with message `add data.txt file`.  Find below more details:

1. In `/usr/src/kodekloudrepos/apps` git repository, reset the git commit history so that there are only two commits in the commit history i.e `initial commit` and `add data.txt file`.
2. Also make sure to push your changes.

# Solution

`ssh natasha@ststor01`

`sudo su -`

`git log`

```bash
commit 1afaadffa1081b56853e7cd9aeb2997a57cd20b2
Author: Admin <admin@kodekloud.com>
Date:   Tue Apr 30 17:38:40 2024 +0000

    add data.txt file

commit ceb0fa925453287b6ee9b881c637a63268560f5e
Author: Admin <admin@kodekloud.com>
Date:   Tue Apr 30 17:38:40 2024 +0000

    initial commit
```

Reset the commit history: `git reset --hard <commit-hash>`

Replace `<commit-hash>` with the commit hash obtained from step 2. This command will reset the commit history, moving the HEAD and branch pointer to the specified commit and discarding all subsequent commits.

git reset --hard ceb0fa925453287b6ee9b881c637a63268560f5e

git reset --hard 1afaadffa1081b56853e7cd9aeb2997a57cd20b2

```bash
[root@ststor01 apps]# git reset --hard ceb0fa925453287b6ee9b881c637a63268560f5e
HEAD is now at ceb0fa9 initial commit
[root@ststor01 apps]# git reset --hard 1afaadffa1081b56853e7cd9aeb2997a57cd20b2
HEAD is now at 1afaadf add data.txt file
[root@ststor01 apps]#
```

Verify the commit history: `git log`

Push the changes to the remote repository:  `git push -f origin master`
