# Instructions

We have LEMP stack configured on apps and database server in `Stratos` DC. Its using Nginx + php-fpm, for now we have deployed a sample php page on these apps. Due to some misconfiguration the php page is not loading on the web server. Seems like at least two app servers are having issues. Find below more details and make sure website works on LBR URL and locally on each app as well.

1. Nginx is supposed to run on port `80` on all app servers.

2. Nginx document root is `/var/www/html/`

3. Test the webpage on LBR URL (use `LBR` button on the top bar) and locally on each app server to make sure it works. It must not display any error message or nginx default page.

# Solution

`ssh tony@stapp01`

`ssh steve@stapp02`

`ssh banner@stapp03`

`sudo su -`

`vi /etc/nginx/nginx.conf`

Make changes so blocks look like on App3:

```bash
   #On App 1:
       server {
        listen       8084 default_server;
        listen       [::]:8084 default_server;
        server_name  _;
        root         /var/www/html/;
        index index.html index.htm;   #fix this one too
   #On App 2:
       server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /var/www/html/;
        index index.php index.html index.htm;
        
        <...>
        fastcgi_pass unix:/var/opt/remi/php74/run/php-fpm/www;
   #On App 3:
       server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html/;
        index index.php index.html index.htm;
 
        <...>
        fastcgi_pass unix:/var/opt/remi/php74/run/php-fpm/www.sock;
```

Test the configuration

```bash
[root@stapp03 ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[root@stapp03 ~]#
```

`systemctl reload nginx`

`systemctl restart nginx`

`systemctl status nginx`

`curl http://localhost`

```bash

[root@stapp01 ~]# curl http://localhost
App is able to connect to the database using user kodekloud_sam
[root@stapp02 ~]# curl http://localhost
App is able to connect to the database using user kodekloud_sam
[root@stapp03 ~]# curl http://localhost
App is able to connect to the database using user kodekloud_sam
```

![image](https://github.com/user-attachments/assets/8760adfe-f788-4f78-a7e3-b5958f0db474)

