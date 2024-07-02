# Instructions

Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details:

SSH into `storage server` using user `max` and password `Max_pass123`. Under `/home/max` you will find the `story-blog` repository. Try to push the changes to the origin repo and fix the issues. The `story-index.txt` must have titles for all 4 stories. Additionally, there is a typo in `The Lion and the Mooose` line where `Mooose` should be `Mouse`.

Click on the `Gitea UI` button on the top bar. You should be able to access the `Gitea` page. You can login to `Gitea` server from UI using username `sarah` and password `Sarah_pass123` or username `max` and password `Max_pass123`.

`Note:` For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

# Solution

`sshpass -p 'Max_pass123' ssh -o StrictHostKeyChecking=no  max@172.16.238.15`

`sudo su -`

`cd /home/max/story-blog/`

`git push`

`git pull origin master`

`vi story-index.txt`

```bash
ststor01:/home/max/story-blog# cat story-index.txt 
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dogs
```

`git add story-index.txt` 

`git commit -m "fix typo"`

`git push origin master`

```bash
max $ sudo su -
ststor01:~# cd /home/max/story-blog/
ststor01:/home/max/story-blog# ls
fox-and-grapes.txt  frogs-and-ox.txt    lion-and-mouse.txt  story-index.txt
ststor01:/home/max/story-blog# git push
Username for 'http://git.stratos.xfusioncorp.com': max
Password for 'http://max@git.stratos.xfusioncorp.com': 
To http://git.stratos.xfusioncorp.com/sarah/story-blog.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'http://git.stratos.xfusioncorp.com/sarah/story-blog.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
ststor01:/home/max/story-blog# git pull origin master
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From http://git.stratos.xfusioncorp.com/sarah/story-blog
 * branch            master     -> FETCH_HEAD
   907e8a0..1baf334  master     -> origin/master
Auto-merging story-index.txt
CONFLICT (add/add): Merge conflict in story-index.txt
Automatic merge failed; fix conflicts and then commit the result.
ststor01:/home/max/story-blog# vi story-index.txt
ststor01:/home/max/story-blog# git add story-index.txt 
ststor01:/home/max/story-blog# git commit -m "fix typo"
[master f49a42f] fix typo
 Committer: root <root@ststor01.stratos.xfusioncorp.com>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly:

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

ststor01:/home/max/story-blog# git push origin master
Username for 'http://git.stratos.xfusioncorp.com': max
Password for 'http://max@git.stratos.xfusioncorp.com': 
Counting objects: 7, done.
Delta compression using up to 36 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 1.15 KiB | 0 bytes/s, done.
Total 7 (delta 1), reused 0 (delta 0)
remote: . Processing 1 references
remote: Processed 1 references in total
To http://git.stratos.xfusioncorp.com/sarah/story-blog.git
   1baf334..f49a42f  master -> master
ststor01:/home/max/story-blog#
```

Connect to Gitea as Sarah

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/9ebd8173-edb9-4af4-9cf1-4f08b0c41efd)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/601d1d96-5f50-4a28-a8e9-cba7ce015758)

