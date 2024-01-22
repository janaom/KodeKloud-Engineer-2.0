# Instructions

To secure our `Nautilus` infrastructure in `Stratos Datacenter` we have decided to install and configure `firewalld`on all app servers. We have Apache and Nginx services running on these 
apps. Nginx is running as a reverse proxy server for Apache. We might have more robust firewall settings in the future, but for now we have decided to go with the given requirements listed below:

a. Allow all incoming connections on Nginx port, i.e `80`.

b. Block all incoming connections on Apache port, i.e `8080`.

c. All rules must be permanent.

d. Zone should be public.

e. If Apache or Nginx services aren't running already, please make sure to start them.

# Solution

ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03

Install firewalld: `sudo yum install firewalld`

Start and enable firewalld to ensure it starts automatically on system boot:

```
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

Configure the firewall rules as per the given requirements:

```
# Allow incoming connections on Nginx port (80)
sudo firewall-cmd --permanent --zone=public --add-port=80/tcp

# Block incoming connections on Apache port (8080)
sudo firewall-cmd --permanent --zone=public --remove-port=8080/tcp

# Reload the firewall to apply the changes
sudo firewall-cmd --reload
```

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/9662d85a-1d95-4c87-8cba-3d9631d505a3)

To check the existing Apache httpd & Nginx service status: `systemctl status httpd &&  systemctl status nginx`
