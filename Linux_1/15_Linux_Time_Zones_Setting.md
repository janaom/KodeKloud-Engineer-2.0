# Instructions

During the daily standup, it was pointed out that the timezone across `Nautilus Application Servers` in `Stratos Datacenter` doesn't match with that of the local datacenter's timezone, which is `America/Bogota`.

Correct the mismatch.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

ssh into the App Server 2: `ssh steve@172.16.238.11`

ssh into the App Server 3: `ssh banner@172.16.238.12`

Check the current timezone configuration: `timedatectl show`

Update the timezone setting: `sudo timedatectl set-timezone America/Bogota`

Verify the timezone synchronization: `timedatectl show | grep Timezone`

Example:

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c3785b70-647c-4f05-8226-1d5434c39ce9)
