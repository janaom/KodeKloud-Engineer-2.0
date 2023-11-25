# Instructions

The system admins team of `xFusionCorp Industries` has set up a new tool on all app servers, as they have a requirement to create a service user account that will be used by that tool.

Create a user named `anita` in `App Server 2` without a home directory.

# Solution

ssh into the App Server 2: `ssh steve@172.16.238.11`

Run the following command to create the user "anita" without a home directory: `sudo useradd --no-create-home anita`

This command creates a user named "anita" without creating a home directory for the user. The `--no-create-home` option ensures that a home directory is not created during the user creation process.

To check if the user was created: `grep "anita:" /etc/passwd`

To remove user: `sudo userdel --remove anita`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/e7d2ca54-7001-4258-824f-866808ff71e2)
