# Instructions

DevOps team created a new Git repository last week; however, as of now 
no team is using it. The Nautilus application development team recently 
asked for a copy of that repo on `Storage server` in Stratos DC. Please clone the repo as per details shared below:

1. The repo that needs to be cloned is `/opt/beta.git`
2. Clone this git repository under `/usr/src/kodekloudrepos` directory. Please do not try to make any changes in the repo.

# Solution

ssh into the Nautilus Storage Server: `ssh natasha@172.16.238.15`

(Infra: `https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details`)

`cd /usr/src/kodekloudrepos`

`git clone /opt/beta.git` if you get ‘fatal: could not create work tree dir 'beta': Permission denied’ use sudo: `sudo git clone /opt/beta.git /usr/src/kodekloudrepos/beta`

Check the result: `ls -l`


![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/95f6525b-5dd1-4e56-a541-3ff3c9c7568b)
