# Instructions

A new developer just joined the Nautilus development team and has been assigned a new project for which he needs to create a new repository under his account on Gitea server. Additionally, there is some existing data that need to be added to the repo. Below you can find more details about the task:

Click on the `Gitea UI` button on the top bar. You should be able to access the `Gitea` UI. Login to `Gitea` server using username `max` and password `Max_pass123`.

a. Create a new git repository `story_beta` under max user.

b. SSH into `storage server` using user `max` and password `Max_pass123` and clone this newly created repository under user `max` home directory i.e `/home/max`.

c. Copy all files from location `/usr/devops` to the repository and commit/push your changes to the `master` branch. The commit message must be `"add stories"` (must be done in single commit).

d. Create a new branch `max_demo` from `master`.

e. Copy a file `story-index-max.txt` from location `/tmp/stories/` to the repository. This file has a typo, which you can fix by changing the word `Mooose` to `Mouse`. Commit and push the changes to the newly created branch. Commit message must be `"typo fixed for Mooose"` (must be done in single commit).

`Note:` For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

# Solution

Open Gitea link, sign in as max.

From his profile create a new repository with the name `story_beta`

SSH into storage server: sshpass -p 'Max_pass123' ssh -o StrictHostKeyChecking=no  [max@172.16.238.15](mailto:max@172.16.238.15)

Clone the repo. Check the files you need to copy under /usr/devops

```bash
thor@jumphost ~$ sshpass -p 'Max_pass123' ssh -o StrictHostKeyChecking=no  max@172.16.238.15
Warning: Permanently added '172.16.238.15' (ED25519) to the list of known hosts.
Welcome to xFusionCorp Storage server.
max $ pwd
/home/max
max $ git clone http://git.stratos.xfusioncorp.com/max/story_beta.git
Cloning into 'story_beta'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
max $ ls -la
total 32
drwxr-sr-x    1 max      max           4096 Jul  1 12:28 .
drwxr-xr-x    1 root     root          4096 Oct 26  2020 ..
-rw-r--r--    1 max      max            202 Oct 26  2020 .bash_profile
-rw-r--r--    1 max      max            202 Oct 26  2020 .bashrc
-rw-r--r--    1 max      max             50 Oct 26  2020 .vimrc
drwxr-sr-x    3 max      max           4096 Jul  1 12:28 story_beta
max $ ls -la /usr/devops
total 20
drwxr-xr-x    2 max      max           4096 Jul  1 12:22 .
drwxr-xr-x    1 root     root          4096 Jul  1 12:22 ..
-rw-r--r--    1 max      max            792 Jul  1 12:22 frogs-and-ox.txt
-rw-r--r--    1 max      max           1086 Jul  1 12:22 lion-and-mouse.txt
```

Copy files:
cp /usr/devops/* ~/story_beta/

```bash
max $ ls -la ~/story_beta/
total 24
drwxr-sr-x    3 max      max           4096 Jul  1 12:31 .
drwxr-sr-x    1 max      max           4096 Jul  1 12:28 ..
drwxr-sr-x    7 max      max           4096 Jul  1 12:28 .git
-rw-r--r--    1 max      max            792 Jul  1 12:31 frogs-and-ox.txt
-rw-r--r--    1 max      max           1086 Jul  1 12:31 lion-and-mouse.txt
max $ cd ~/story_beta/
max $ ls
frogs-and-ox.txt    lion-and-mouse.txt
```

Commit changes. Enter Username and Password. And switch to a new branch. Check the file you need to copy

```bash
max $ git add -A; git commit -m "add stories"; git push
[master (root-commit) 6955288] add stories
 Committer: Linux User <max@ststor01.stratos.xfusioncorp.com>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 2 files changed, 42 insertions(+)
 create mode 100644 frogs-and-ox.txt
 create mode 100644 lion-and-mouse.txt
Username for 'http://git.stratos.xfusioncorp.com': max
Password for 'http://max@git.stratos.xfusioncorp.com': 
Counting objects: 4, done.
Delta compression using up to 36 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1.19 KiB | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
remote: . Processing 1 references
remote: Processed 1 references in total
To http://git.stratos.xfusioncorp.com/max/story_beta.git
 * [new branch]      master -> master
max (master)$ git checkout -b max_demo
Switched to a new branch 'max_demo'
max (max_demo)$ 
max (max_demo)$ ls -la  /tmp/stories/story-index-max.txt
-rw-r--r--    1 sarah    sarah          102 Jul  1 12:21 /tmp/stories/story-index-max.txt
max (max_demo)$ cp  /tmp/stories/story-index-max.txt ~/story_beta/ 
max (max_demo)$ pwd
/home/max/story_beta
max (max_demo)$ ls -la
total 28
drwxr-sr-x    3 max      max           4096 Jul  1 12:46 .
drwxr-sr-x    1 max      max           4096 Jul  1 12:28 ..
drwxr-sr-x    8 max      max           4096 Jul  1 12:37 .git
-rw-r--r--    1 max      max            792 Jul  1 12:31 frogs-and-ox.txt
-rw-r--r--    1 max      max           1086 Jul  1 12:31 lion-and-mouse.txt
-rw-r--r--    1 max      max            102 Jul  1 12:46 story-index-max.txt
```

Open txt file and correct the text

```bash
1. The Lion and the Mouse 
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
```

```bash
max (max_demo)$ vi story-index-max.txt
max (max_demo)$ git status
On branch max_demo
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        story-index-max.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Push the changes
git add story-index-max.txt; git commit -m "typo fixed for Mooose"; git push origin max_demo

```bash
max (max_demo)$ git add story-index-max.txt; git commit -m "typo fixed for Mooose"; git push origin max_demo
[max_demo b94c4a7] typo fixed for Mooose
 Committer: Linux User <max@ststor01.stratos.xfusioncorp.com>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 1 file changed, 4 insertions(+)
 create mode 100644 story-index-max.txt
Username for 'http://git.stratos.xfusioncorp.com': max
Password for 'http://max@git.stratos.xfusioncorp.com': 
Counting objects: 3, done.
Delta compression using up to 36 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 414 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a new pull request for 'max_demo':
remote:   http://git.stratos.xfusioncorp.com/max/story_beta/compare/master...max_demo
remote: 
remote: . Processing 1 references
remote: Processed 1 references in total
To http://git.stratos.xfusioncorp.com/max/story_beta.git
 * [new branch]      max_demo -> max_demo
max (max_demo)$
```

Go back to Gitea and check the changes

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/547ab95b-bef9-41e4-98b8-4a7fe7bc3d93)
