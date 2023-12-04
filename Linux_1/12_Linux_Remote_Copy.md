# Instructions

One of the `Nautilus` developers has copied confidential data on the jump host in `Stratos DC`. That data must be copied to one of the app servers. Because developers do not have access to app servers, they asked the system admins team to accomplish the task for them.

Copy `/tmp/nautilus.txt.gpg` file from jump server to `App Server 3`  at location `/home/data`.

# Solution

The `scp` command is used to securely copy files between hosts on a network:`scp /tmp/nautilus.txt.gpg banner@172.16.238.12:/home/data`

ssh into the App Server 3 and check the result: `ssh banner@172.16.238.12`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f1f01bac-29e6-409a-b44e-261f661cc3ec)
