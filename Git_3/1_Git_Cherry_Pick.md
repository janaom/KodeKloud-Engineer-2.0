# Instructions

The Nautilus application development team has been working on a project repository `/opt/cluster.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`.  They recently shared the following requirements with the DevOps team:

There are two branches in this repository, `master` and `feature`. One of the developers is working on the `feature` branch and their work is still in progress, however they want to merge one of the commits from the `feature` branch to the `master` branch, the message for the commit that needs to be merged into `master` is `Update info.txt`. Accomplish this task for them, also remember to push your changes eventually.

# Solution

`ssh natasha@ststor01`

`sudo su -`

`cd /usr/src/kodekloudrepos/cluster`

`git branch`

```bash
[root@ststor01 cluster]# git branch
* feature
  master
```

 `git log --oneline | grep "Update info.txt”`

 ```bash
[root@ststor01 cluster]# git log --oneline | grep "Update info.txt"
18ca75c Update info.txt
```

`git log --oneline`

```bash
[root@ststor01 cluster]# git log --oneline
a112dae (HEAD -> feature, origin/feature) Update welcome.txt
18ca75c Update info.txt
cc8169b (origin/master, master) Add welcome.txt
95fb52e initial commit
[root@ststor01 cluster]#
```

`git log --oneline`

```bash
[root@ststor01 cluster]# git cherry-pick 18ca75c
[master 78d6617] Update info.txt
 Date: Wed Apr 24 15:59:55 2024 +0000
 1 file changed, 1 insertion(+), 1 deletion(-)
[root@ststor01 cluster]#
```

 `git push origin master`

 ```bash
[root@ststor01 cluster]# git push origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 36 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 315 bytes | 315.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/cluster.git
   cc8169b..78d6617  master -> master
[root@ststor01 cluster]#
```
