# Instructions

`xFusionCorp Industries` has hosted several static websites on `Nautilus Application Servers` in `Stratos DC`. There are some confidential directories in the document root that need to be password protected. Since they are using `Apache` for hosting the websites, the production support team has decided to use `.htaccess` with basic `auth`. There is a website that needs to be uploaded to `/var/www/html/itadmin` on `Nautilus App Server 2`. However, we need to set up the authentication before that.

1. Create `/var/www/html/itadmin` direcotry if doesn't exist.

2. Add a user `jim` in `htpasswd` and set its password to `TmPcZjtRQx`.

3. There is a file `/tmp/index.html` present on `Jump Server`. Copy the same to the directory you created, please make sure default document root should remain `/var/www/html`. Also website should work on URL `http://<app-server-hostname>:8080/itadmin/`

# Solution

`scp /tmp/index.html steve@stapp02:/home/steve/`

`ssh steve@stapp02`

`sudo su -`

`sudo systemctl start httpd`

`sudo systemctl enable httpd`

`sudo systemctl status httpd`

`cd /var/www/html/`

`sudo vi .htaccess`

```bash
  AuthType Basic
  AuthName "Secure Content"
  AuthUserFile /var/www/html/itadmin/.htpasswd
  require valid-user
```

`sudo mkdir itadmin`

`sudo mv /home/steve/index.html /var/www/html/itadmin/`

`sudo htpasswd -c /var/www/html/itadmin/.htpasswd jim`

`cd /etc/httpd/conf/`

`sudo vi httpd.conf`

```bash
<Directory />
    AllowOverride AuthConfig
    Require all denied
</Directory>
```

`sudo systemctl restart httpd`

`sudo curl http://localhost:8080`

`sudo curl http://localhost:8080 -u jim`
