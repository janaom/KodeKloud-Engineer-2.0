# Instructions

We have a requirement where we want to password protect a directory in the Apache web server document root. We want to password protect `http://<website-url>:<apache_port>/protected` URL as per the following  requirements (you can use any `website-url` for it like `localhost` since there are no such specific requirements as of now). Setup the same on `App server 3` as per below mentioned requirements:

a. We want to use basic authentication.

b. We do not want to use `htpasswd` file based authentication. Instead, we want to use `PAM` authentication, i.e `Basic Auth + PAM` so that we can authenticate with a Linux user.

c. We already have a user `ammar` with password `LQfKeWWxWD` which you need to provide access to.

d. You can test the same using a curl command from jump host `curl http://<website-url>:<apache_port>/protected`.

# Solution

`ssh banner@stapp03`

`sudo su -`

`yum --enablerepo=epel -y install mod_authnz_external pwauth`

`vi /etc/httpd/conf.d/authnz_external.conf`

Add these lines at the end

```bash
<Directory /var/www/html/protected>

AuthType Basic

AuthName "PAM Authentication"

AuthBasicProvider external

AuthExternal pwauth

require valid-user

</Directory>
```

`mkdir -p /var/www/html/protected`

`cat /var/www/html/protected/index.html`

```bash
[root@stapp03 ~]# cat /var/www/html/protected/index.html
This is KodeKloud Protected Directory
```

`systemctl start httpd`

`curl -u ammar:LQfKeWWxWD http://localhost:8080/protected/`

```bash
[root@stapp03 ~]# curl -u ammar:LQfKeWWxWD http://localhost:8080/protected/
This is KodeKloud Protected Directory
```
