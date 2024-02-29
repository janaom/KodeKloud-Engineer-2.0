# Instructions

We are working on hardening Apache web server on all app servers. As a part of this process we want to add some of the Apache response headers for security purpose. We are testing the settings one by one on all app servers. As per details mentioned below enable these headers for Apache:

1. Install `httpd` package on `App Server 3` using yum and configure it to run on `8089` port, make sure to start its service.
2. Create an `index.html` file under Apache's default document root i.e `/var/www/html` and add below given content in it.
    
    `Welcome to the xFusionCorp Industries!`
    
3. Configure Apache to enable below mentioned headers:
    
    `X-XSS-Protection` header with value `1; mode=block`
    
    `X-Frame-Options` header with value `SAMEORIGIN`
    
    `X-Content-Type-Options` header with value `nosniff`
    

`Note:` You can test using curl on the given app server as LBR URL will not work for this task.

# Solution

`ssh banner@stapp03`

`sudo su -`

`yum install httpd -y`

`vi /etc/httpd/conf/httpd.conf`

Find this part and change Listen to 8089 and add Header at the end

```python
#Listen 12.34.56.78:80
Listen 8089 

<...>

Header set X-XSS-Protection "1; mode=block"

Header always append X-Frame-Options SAMEORIGIN

Header set X-Content-Type-Options nosniff
```

`vi /var/www/html/index.html`

Enter:

```python
Welcome to the xFusionCorp Industries!
```

`systemctl start httpd`

`systemctl status  httpd`

`curl http://localhost:8089`

`curl -i http://localhost:8089`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c0f7cf06-eeb2-40a7-ac95-5310759261fc)
