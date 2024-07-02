# Instructions

Some new developers have joined `xFusionCorp Industries` and have been assigned `Nautilus` project. They are going to start development on a new application, and some pre-requisites have been shared with the DevOps team to proceed with. Please note that all tasks need to be performed on `storage server` in `Stratos` DC.

a. Install git, set up any values for user.email and user.name globally and create a bare repository `/opt/media.git`.

b. There is an `update` hook (to block direct pushes to the `master` branch) under `/tmp` on `storage server` itself; use the same to block direct pushes to the `master` branch in `/opt/media.git repo`.

c. Clone `/opt/media.git` repo in `/usr/src/kodekloudrepos/media` directory.

d. Create a new branch named `xfusioncorp_media` in repo that you cloned under `/usr/src/kodekloudrepos` directory.

e. There is a `readme.md` file in `/tmp` directory on `storage server` itself; copy that to the repo, `add/commit` in the new branch you just created, and finally push your branch to the origin.

f. Also create `master` branch from your branch and remember you should not be able to push to the `master` directly as per the hook you have set up.

# Solution

`ssh natasha@ststor01`

`sudo su -`

`yum install -y git`

```bash
[root@ststor01 ~]# git config --global --add user.name natasha
[root@ststor01 ~]# git config --global --add user.email natasha@stratos.xfusioncorp.com
```

`git init --bare /opt/media.git`

```bash
[root@ststor01 ~]# git init --bare /opt/media.git
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
Initialized empty Git repository in /opt/media.git/
[root@ststor01 ~]#
```

`cat /tmp/update`

```bash
[root@ststor01 ~]# cat /tmp/update 
#!/bin/sh
if [ "$1" == refs/heads/master ];
then
  echo "Manual pushes to the master branch is restricted!!"
  exit 1
fi
```

`cp /tmp/update  /opt/media.git/hooks/`

`cd /usr/src/kodekloudrepos/`

`git clone /opt/media.git`

```bash
[root@ststor01 ~]cp /tmp/update  /opt/media.git/hooks/
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/
[root@ststor01 kodekloudrepos]# ls
[root@ststor01 kodekloudrepos]# git clone /opt/media.git
Cloning into 'media'...
warning: You appear to have cloned an empty repository.
done.
[root@ststor01 kodekloudrepos]# ls
media
[root@ststor01 kodekloudrepos]#
```

`cd media`

`git checkout -b xfusioncorp_media`

`git branch`

`cp /tmp/readme.md .`

`git commit -m "Readme file”`

`git push origin xfusioncorp_media`

`git checkout -b master`

`git push origin master`

```bash
[root@ststor01 kodekloudrepos]# cd media
[root@ststor01 media]# git checkout -b xfusioncorp_media
Switched to a new branch 'xfusioncorp_media'
[root@ststor01 media]# git branch
[root@ststor01 media]# cp /tmp/readme.md .
[root@ststor01 media]# git add readme.md 
[root@ststor01 media]# git commit -m "Readme file"
[xfusioncorp_media (root-commit) 5f41bfb] Readme file
 1 file changed, 1 insertion(+)
 create mode 100644 readme.md
[root@ststor01 media]# git push origin xfusioncorp_media
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 251 bytes | 251.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/media.git
 * [new branch]      xfusioncorp_media -> xfusioncorp_media
[root@ststor01 media]# git checkout -b master 
Switched to a new branch 'master'
Your branch is based on 'origin/master', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)
[root@ststor01 media]# git push origin master
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: Manual pushes to the master branch is restricted!!
remote: error: hook declined to update refs/heads/master
To /opt/media.git
 ! [remote rejected] master -> master (hook declined)
error: failed to push some refs to '/opt/media.git'
[root@ststor01 media]#
```


