# Instructions

The system admins team of `xFusionCorp Industries` needs to deploy a new application on `App Server 2` in `Stratos Datacenter`. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:

1. Install and configure `nginx` on `App Server 2`.

2. On `App Server 2` there is a self signed SSL certificate and key present at location `/tmp/nautilus.crt` and `/tmp/nautilus.key`. Move them to some appropriate location and deploy the same in Nginx.

3. Create an `index.html` file with content `Welcome!` under Nginx document root.

4. For final testng try to access the `App Server 2` link (either hostname or IP) from `jump host` using curl command. For example `curl -Ik https://<app-server-ip>/`.

# Solution

`ssh steve@stapp02`

`sudo su -`

`yum install epel-release -y`

`yum install  -y nginx`

`systemctl start nginx`

`systemctl enable nginx`

`systemctl status nginx`

`yum -y install mod_ssl`

`sudo unlink index.html`

`vi index.html  add Welcome!`

`cd /tmp`

`chmod 777 nautilus.crt nautilus.key`

`cp nautilus.crt /usr/share/nginx/html/`

`cp nautilus.key /usr/share/nginx/html/`

`cd /usr/share/nginx/html/`

`vi /etc/nginx/nginx.conf`

adjust the file

```bash
    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        listen       443 ssl;
        server_name  172.16.238.11;
        root         /usr/share/nginx/html;
        index        index.html
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_ciphers ECDHE+RSAGCM:ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:!aNULL!eNull:!EXPORT:!DES:!3DES:!MD5:!DSS;
        ssl_certificate      /usr/share/nginx/html/nautilus.crt;
        ssl_certificate_key  /usr/share/nginx/html/nautilus.key;

```

`systemctl restart nginx`

```bash
thor@jump_host ~$ curl -Ik https://172.16.238.11/
HTTP/1.1 200 OK
Server: nginx/1.14.1
Date: Sat, 18 May 2024 15:10:33 GMT
Content-Type: text/html
Content-Length: 9
Last-Modified: Sat, 18 May 2024 15:04:29 GMT
Connection: keep-alive
ETag: "6648c37d-9"
Accept-Ranges: bytes
```
