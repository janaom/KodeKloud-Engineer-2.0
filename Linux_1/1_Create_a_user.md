# Instructions

For security reasons the `xFusionCorp Industries` security team has decided to use custom Apache users for each web application hosted, rather than its default user. This will be the Apache user, so it shouldn't use the default home directory. Create the user as per requirements given below:

a. Create a user named `jim` on the `App server 2` in Stratos Datacenter.

b. Set its UID to `1582` and home directory to `/var/www/jim`.


# Solution

ssh into the App Server 2: `ssh steve@172.16.238.11`

This command creates a new user with the username "jim", sets the UID to 1582, creates the home directory `/var/www/jim`, and the `-m` option ensures that the home directory is created: `sudo useradd -u 1582 -d /var/www/jim -m jim` 

To check if the user "jim" was created successfully, you can use the following command: `id jim`

Running this command will display information about the user "jim", including the UID, GID (group ID), and the groups the user belongs to. If the user was created successfully, you will see output similar to the following:

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/80bbf37f-f762-4885-a136-c7b1b687d9b3)
