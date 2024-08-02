# Instructions

Day by day traffic is increasing on one of the websites managed by the `Nautilus`production support team. Therefore, the team has observed a degradation
 in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on `Nautilus` infra in `Stratos DC`. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:

a. Install `nginx` on `LBR` (load balancer) server.

b. Configure load-balancing with the an `http` context making use of all `App Servers`.

c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.

d. Once done, you can access the website using `StaticApp` button on the top bar.

# Solution

Check the port at least in 2 Apps just to be sure:

`ssh tony@stapp01`

`vi /etc/httpd/conf/httpd.conf`

`Check the port: Listen 5004`

`ssh steve@stapp02`

`cat /etc/httpd/conf/httpd.conf`

`Check the port: Listen 5004`

Connect to LBR:

`ssh loki@stlb01`

`sudo su -`

`yum update`

`yum install nginx`

`vi /etc/nginx/nginx.conf`

Add this part:
```bash
include /etc/nginx/conf.d/*.conf;
upstream app_servers {
    server stapp01:5004;
    server stapp02:5004;
    server stapp03:5004;
# Add more servers as needed
}
server {
    listen       80;
    listen       [::]:80;
    location / {
    proxy_pass http://app_servers;
    }
```

`systemctl restart nginx`

![image](https://github.com/user-attachments/assets/86f6198a-c98b-4c0b-b7b1-20d0a0919104)
