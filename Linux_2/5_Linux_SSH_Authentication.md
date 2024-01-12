# Instructions

The system admins team of `xFusionCorp Industries` has set up some scripts on `jump host` that run on regular intervals and perform operations on all app servers in `Stratos Datacenter`. To make these scripts work properly we need to make sure the `thor` user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e `tony` for app server 1). Based on the requirements, perform the following:

Set up a password-less authentication from user `thor` on jump host to all app servers through their respective sudo users.

# Solution

ssh-keygen -t rsa

ssh-copy-id -i ~/.ssh/id_rsa.pub tony@stapp01

ssh tony@stapp01

ssh-copy-id -i ~/.ssh/id_rsa.pub steve@stapp02

ssh steve@stapp02

ssh-copy-id -i ~/.ssh/id_rsa.pub banner@stapp03

ssh banner@stapp03

Follow this example

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/499ea343-2e5c-4419-a670-e3f569339274)
