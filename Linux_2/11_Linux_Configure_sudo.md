# Instructions

We have some users on all app servers in `Stratos Datacenter`. Some of them have been assigned some new roles and responsibilities,  therefore their users need to be upgraded with sudo access so that they can perform admin level tasks.

a. Provide sudo access to user `rose` on all app servers.

b. Make sure you have set up password-less sudo for the user.

# Solution

ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03

Add the user "rose" to the sudoers file by running the following command: `sudo visudo`

Add the following line below the `%sudo` or `%wheel` line to provide sudo access specifically to the "rose" user: `rose ALL=(ALL) NOPASSWD: ALL`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b1f8469a-4e71-4fcc-89ee-cb98496643df)

Check the result: `sudo cat /etc/sudoers |grep rose`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/60432b5a-56ab-4318-a198-3301e28fa306)

