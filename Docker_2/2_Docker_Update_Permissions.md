# Instructions

One of the Nautilus project developers need access to run docker commands on `App Server 1`. This user is already created on the server. Accomplish this task as per details given below:

User `mariyam` is not able to run docker commands on `App Server 1` in Stratos DC, make the required changes so that this user can run docker commands without `sudo`.

# Solution


[Docker docs](https://docs.docker.com/engine/install/linux-postinstall/): If you don't want to preface the `docker` command with `sudo`, create a Unix group called `docker` and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the `docker` group. On some Linux distributions, the system automatically creates this group when installing Docker Engine using a package manager. In that case, there is no need for you to manually create the group.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/843d9427-4a63-40af-80b8-ef7e09ab3eb5)

ssh into the App Server 1: `ssh tony@172.16.238.10`

Try to add the group: `sudo groupadd docker`, probably you will see the result: `groupadd: group 'docker' already exists`

If the group is already exists, then just add the user "mariyam" to the existing "docker" group: `sudo usermod -aG docker mariyam`

To check the result, run: `groups mariyam`

You should see this: `mariyam : mariyam docker`
