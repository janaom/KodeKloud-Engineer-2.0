# Instructions

The Nautilus devops team got some requirements related to some Apache config changes. They need to setup some redirects for some URLs. There might be some more changes need to be done. Below you can find more details regarding that:

1.) `httpd` is already installed on `app server 3`. Configure Apache to listen on port `5002`.

1. Configure Apache to add some redirects as mentioned below:
    
    a.) Redirect `http://stapp03.stratos.xfusioncorp.com:<Port>/` to `http://www.stapp03.stratos.xfusioncorp.com:<Port>/` i.e `non www` to `www`. This must be a permanent redirect i.e `301`
    
    b.) Redirect `http://www.stapp03.stratos.xfusioncorp.com:<Port>/blog/` to `http://www.stapp03.stratos.xfusioncorp.com:<Port>/news/`. This must be a temporary redirect i.e `302`.


# Solution

`ssh banner@stapp03`

`sudo su -`

Open the Apache configuration file for editing: `sudo vi /etc/httpd/conf/httpd.conf`

Locate the line that specifies the `Listen` directive and change it to:

```bash
#Listen 12.34.56.78:80
Listen 5002
```

Configure the redirects by adding the following lines to the Apache configuration file, preferably at the end:

```bash
<VirtualHost *:5002>
    ServerName stapp03.stratos.xfusioncorp.com
    Redirect permanent / http://www.stapp03.stratos.xfusioncorp.com:5002/
</VirtualHost>

<VirtualHost *:5002>
    ServerName www.stapp03.stratos.xfusioncorp.com
    Redirect temp /blog/ http://www.stapp03.stratos.xfusioncorp.com:5002/news/
</VirtualHost>
```

Restart the Apache service for the changes to take effect: `sudo systemctl restart httpd`

Check the result.

Test the non-www to www permanent redirect:`curl -I http://stapp03.stratos.xfusioncorp.com:5002/`

The `-I` option is used to fetch the HTTP headers only. Look for the `Location` header in the response. It should contain `http://www.stapp03.stratos.xfusioncorp.com:5002/` and the response code should be `301 Moved Permanently`.

```bash
[root@stapp03 ~]# curl -I http://stapp03.stratos.xfusioncorp.com:5002/
HTTP/1.1 301 Moved Permanently
Date: Sat, 04 May 2024 17:07:30 GMT
Server: Apache/2.4.6 (CentOS) PHP/7.2.26
Location: http://www.stapp03.stratos.xfusioncorp.com:5002/
Content-Type: text/html; charset=iso-8859-1
```

Test the temporary redirect:curl -I http://www.stapp03.stratos.xfusioncorp.com:5002/blog/
Again, look for the Location header in the response. It should contain http://www.stapp03.stratos.xfusioncorp.com:5002/news/ and the response code should be 302 Found.

```bash
[root@stapp03 ~]# curl -I http://www.stapp03.stratos.xfusioncorp.com:5002/blog/
HTTP/1.1 302 Found
Date: Sat, 04 May 2024 17:08:17 GMT
Server: Apache/2.4.6 (CentOS) PHP/7.2.26
Location: http://www.stapp03.stratos.xfusioncorp.com:5002/news/
Content-Type: text/html; charset=iso-8859-1
```
