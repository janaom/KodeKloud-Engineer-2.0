# Instructions

There are specific access levels for users defined by the `xFusionCorp Industries`
 system admin team. Rather than providing access levels to every 
individual user, the team has decided to create groups with required 
access levels and add users to that groups as needed. See the following 
requirements:

a. Create a group  named `nautilus_developers` in all App servers in `Stratos Datacenter`.

b. Add the user `mohammed` to  `nautilus_developers` group in all App servers. (create the user if doesn't exist).

# Solution

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/cf43e37f-acd8-4c66-bda3-1415c72bf481)

ssh into the App Server 1: `ssh tony@172.16.238.10`

ssh into the App Server 2: `ssh steve@172.16.238.11`

ssh into the App Server 3: `ssh banner@172.16.238.12`

To list all existing groups, you can run the command: `cat /etc/group`

Create the group "nautilus_developers" by running the following command on each App server:  `sudo groupadd nautilus_developers`

To check if the "nautilus_developers" group was created, you can use the following command: `grep -E "^nautilus_developers:" /etc/group`

To list all existing users, you can run the command: `cat /etc/passwd`

Create the user "mohammed" if they don't already exist. Run the following command on each App server: `sudo useradd mohammed`

To check if the user "mohammed" exists, you can use the following command: `grep -E "^mohammed:" /etc/passwd` This command searches for a line that starts with "mohammed:" in the `/etc/passwd` file. If the user exists, it will display information about the user, including the username, UID, home directory, and shell.

Add the user "mohammed" to the "nautilus_developers" group on each App server by running the following command: `sudo usermod -aG nautilus_developers mohammed`

- `a`: This option stands for "append" or "add". It is used to add the user to the specified group(s) without removing them from any existing groups. It ensures that the user's current group memberships are preserved while adding the new group.
- `G`: This option specifies the group(s) to which the user should be added. Multiple groups can be specified by separating them with commas. In this case, the group `nautilus_developers` is specified.


![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/e5bc2468-ace7-451c-aef5-b6f2bf8337b2)
