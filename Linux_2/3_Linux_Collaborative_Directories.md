# Instructions

The Nautilus team doesn't want its data to be accessed by any of the other groups/teams due to security reasons and want their data to be 
strictly accessed by the `sysops` group of the team.

Setup a collaborative directory `/sysops/data` on `app server 3` in `Stratos Datacenter`.

The directory should be group owned by the group `sysops` and the group should own the files inside the directory. The directory should be `read/write/execute` to the user and group owners, and `others` should not have any access.

# Solution

ssh banner@stapp03

Create the `/sysops/data` directory: `sudo mkdir -p /sysops/data`

Set the group ownership of the directory to `sysops`: `sudo chown :sysops /sysops/data`

The `chown` command changes the group ownership of the directory to `sysops`. The `:` before `sysops` indicates that only the group ownership is being changed.

Set the group ownership of the files inside the directory: `sudo chown -R :sysops /sysops/data`

Set the permissions of the directory to read, write, and execute for the user and group owners: `sudo chmod ug+rwx /sysops/data`

Restrict access to others: `sudo chmod o-rwx /sysops/data`

To verify the ownership of the directory: `ls -ld /sysops/data`

This command will display the ownership and permissions of the `/sysops/data` directory. The group ownership should be set to `sysops`.

To check the ownership and permissions of the files and subdirectories inside the `/sysops/data` directory: `sudo ls -l /sysops/data`

To verify the permissions of the directory: `stat -c "%a %n" /sysops/data`

This command will display the permissions of the `/sysops/data` directory in numeric format. The expected permissions should be `770`, indicating read, write, and execute permissions for the user and group owners, and no access for others.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/213ee95e-631e-448a-b373-438c5e807ebf)

