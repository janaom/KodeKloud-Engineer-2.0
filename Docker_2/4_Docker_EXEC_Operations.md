# Instructions

One of the Nautilus DevOps team members was working to configure services on a `kkloud` container that is running on `App Server 1` in `Stratos Datacenter`.
 Due to some personal work he is on PTO for the rest of the week, but we
 need to finish his pending work ASAP. Please complete the remaining 
work as per details given below:

a. Install `apache2` in `kkloud` container using `apt` that is running on  `App Server 1` in `Stratos Datacenter`.

b. Configure Apache to listen on port `3004` instead of default `http`
 port. Do not bind it to listen on specific IP or hostname only, i.e it 
should listen on localhost, 127.0.0.1, container ip, etc.

c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

# Solution

ssh into the App Server 3: `ssh tony@172.16.238.10`

This command helps you identify the running containers and gather information about them: `docker ps`

This command is used to execute an interactive command inside a running Docker container: `docker exec -it kkloud /bin/sh`

This command installs the Apache HTTP Server (Apache2) on the system using the `apt` package manager: `apt install apache2 -y`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b0ada5e7-a09a-4930-9168-bc4843017fab)

This command installs the Vim editor on the system using the `apt` package manager: `apt install vim`

This command changes the current working directory to `/etc/apache2`: `cd /etc/apache2`

Open the `ports.conf` file using the Vim editor and modify the port configuration: `vi ports.conf`, change 80 to 3004

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/9672b742-e6e3-4eb2-b92c-d8564604bb0c)

This command is used to start the Apache HTTP Server (Apache2) on a Linux system: `service apache2 start`

Check the status: `service apache2 status`, you should see: `apache2 is running`

The result of the command will display the response headers returned by the web server: `curl -Ik localhost:3004`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/5c57708e-0f99-40d8-856c-29e67fad7a44)


