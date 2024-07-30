# Instructions

The `Nautilus` application development team is planning to launch a new PHP-based application, which they want to deploy on `Nautilus` infra in `Stratos DC`. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:

a. Install `nginx` on `app server 1` , configure it to use port `8099` and its document root should be `/var/www/html`.

b. Install `php-fpm` version `8.1` on `app server 1`, it must use the unix socket `/var/run/php-fpm/default.sock` (create the parent directories if don't exist).

c. Configure php-fpm and nginx to work together.

d. Once configured correctly, you can test the website using `curl http://stapp01:8099/index.php` command from jump host.

# Solution

`ssh tony@stapp01`

`sudo su -`

`yum install epel-release`

`yum install nginx`

`dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm`

`dnf module enable php:remi-8.1`

`dnf install -y php-fpm`

`php-fpm --version`

```bash
[root@stapp01 ~]# php-fpm --version
PHP 8.1.29 (fpm-fcgi) (built: Jun  5 2024 05:51:57)
Copyright (c) The PHP Group
Zend Engine v4.1.29, Copyright (c) Zend Technologies
```

`vi /etc/php-fpm.d/www.conf`

Change: user, group, listen

```bash
; RPM: apache user chosen to provide access to the same directories as httpd
user = nginx
; RPM: Keep a group allowed to write in log dir.
group = nginx
<...>
listen = /run/php-fpm/default.sock
```

`chown -R root:nginx /var/lib/php`

`vi /etc/nginx/nginx.conf`

Should look like this:

```bash
    server {
        listen       8099;
        listen       [::]:8099;
        server_name  _;
        root         /var/www/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;
        location ~ \.php$ {
            try_files $uri =404;
            fastcgi_pass php-fpm;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }  
        error_page 404 /404.html;
        location = /404.html {
        }
```

`vi /etc/nginx/conf.d/php-fpm.conf`

Change here to 'default'

```bash
# PHP-FPM FastCGI server
# network or unix domain socket configuration

upstream php-fpm {
    server unix:/run/php-fpm/default.sock;
}
```

`systemctl restart php-fpm nginx`

```bash
[root@stapp01 ~]# curl http://stapp01:8099/index.php
Welcome to xFusionCorp Industries
```

For any questions check this thread: https://kodekloud.com/community/t/configure-nginx-php-fpm-using-unix-sock-error/386486/12
