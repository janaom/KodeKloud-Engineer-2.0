# Instructions

The Nautilus development team shared with the DevOps team requirements for new application development, setting up a Git repository for that project. Create a Git repository on `Storage server` in Stratos DC as per details given below:

1. Install `git` package using `yum` on `Storage server`.
2. After that, create/init a git repository named `/opt/news.git` (use the exact name as asked and make sure not to create a bare repository).

# Solution

`ssh natasha@ststor01`

`sudo su -`

`yum install -y git`

`cd /opt/`

`git init news.git`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/55729111-610c-4908-9dc0-a8914c61600f)
