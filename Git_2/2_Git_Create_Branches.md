# Instructions

Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/ecommerce`. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:

1. On `Storage server` in Stratos DC create a new branch `xfusioncorp_ecommerce` from `master` branch in `/usr/src/kodekloudrepos/ecommerce` git repo. 
2. Please do not try to make any changes in the code.

# Solution

`ssh natasha@ststor01`

`sudo su -`

`cd /usr/src/kodekloudrepos/ecommerce`

`git checkout master`

`git checkout -b xfusioncorp_ecommerce`

`git status`

`git branch -a`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/dcf516b4-7b91-45eb-89f8-7a309c3577ec)
