# Instructions

`Nautilus` system admin's team is planning to deploy a front end application for their backup utility on `Nautilus Backup Server`, so that they can manage the backups of different websites from a graphical user interface. They have shared requirements to set up the same; please accomplish the tasks as per detail given below:

a. Install `Apache` Server on `Nautilus Backup Server` and configure it to use `8087` port (do not bind it to `127.0.0.1` only, keep it default i.e let Apache listen on server's IP, hostname, localhost, 127.0.0.1 etc).

b. Install `Nginx` webserver on `Nautilus Backup Server` and configure it to use `8092`.

c. Configure `Nginx` as a reverse proxy server for `Apache`.

d. There is a sample index file `/home/thor/index.html` on `Jump Host`, copy that file to `Apache's` document root.

e. Make sure to start `Apache` and `Nginx` services.

f. You can test final changes using `curl` command, e.g `curl http://<backup server IP or Hostname>:8092`.

# Solution

`scp /home/thor/index.html clint@stbkp01:/tmp/`

`ssh clint@stbkp01`

`sudo su -`

`yum install httpd`

`yum install epel-release`

`yum install  nginx`

`vi /etc/httpd/conf/httpd.conf`

Change: Listen 8087

`vi /etc/nginx/nginx.conf`

Make changes

```bash
    server {
        listen       8092 default_server;
        listen       [::]:8092 default_server;
        server_name  172.16.238.16;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        proxy_pass http://172.16.238.16:8092;
        }
```

`cp /tmp/index.html /var/www/html/`

`systemctl start httpd`

`systemctl start nginx`

```bash
[root@stbkp01 ~]# curl http://172.16.238.16:8087
Welcome to xFusionCorp Industries!
[root@stbkp01 ~]# curl http://172.16.238.16:8092
<html>
<head><title>500 Internal Server Error</title></head>
<body bgcolor="white">
<center><h1>500 Internal Server Error</h1></center>
<hr><center>nginx/1.14.1</center>
</body>
</html>
[root@stbkp01 ~]#
```
