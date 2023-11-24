# Instructions

The System admin team of `xFusionCorp Industries` has installed a backup agent tool on all app servers. As per the tool's requirements they need to create a user with a non-interactive shell.

Therefore, create a user named `mariyam` with a non-interactive shell on the `App Server 1`.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

Run the following command to create the user "mariyam" with a non-interactive shell: `sudo useradd --shell /bin/false mariyam`

This command creates a user named "mariyam" and sets the shell to `/bin/false`, which is a non-interactive shell. Non-interactive shells are typically used for system accounts or accounts that do not require direct login access.

To check if a user named "mariyam" was successfully created on App Server 1, you can use the following command: `id mariyam`

Running this command will display information about the user "mariyam", including the UID (user ID), GID (group ID), and the groups the user belongs to. If the user was created successfully, you will see output similar to the following:  `uid=1001(mariyam) gid=1001(mariyam) groups=1001(mariyam)`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/58bc8e8e-b08a-4e72-9429-098c83181cfa)
