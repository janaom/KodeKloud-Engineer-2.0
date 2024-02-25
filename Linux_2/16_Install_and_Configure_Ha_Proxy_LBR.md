# Instructions

There is a static website running in Stratos Datacenter. They have already configured the app servers and code is already deployed there. To make it work properly, they need to configure LBR server. There are number of options for that, but team has decided to go with HAproxy. FYI, apache is running on port 8087 on all app servers. Complete this task as per below details.

a. Install and configure HAproxy on LBR server using yum only and make sure all app servers are added to HAproxy load balancer. HAproxy must serve on default http port (Note: Please do not remove stats socket /var/lib/haproxy/stats entry from haproxy default config.).

b. Once done, you can access the website using StaticApp button on the top bar.

# Solution

`ssh loki@stlb01`

`sudo su -`

`yum install haproxy -y`

`cd /etc/haproxy`

`vi haproxy.cfg`

```python
frontend main
    bind *:80

#    server  app4 127.0.0.1:5004 check
```

`ssh tony@stapp01`

`sudo su -`

`cd /etc/httpd/conf`

`vi httpd.conf` (find Listen 8087)

go back to  `haproxy.cfg`  and make these changes:

```python
backend app
    balance     roundrobin
    server  stapp01 172.16.238.10:8087 check
    server  stapp02 172.16.238.11:8087 check
    server  stapp03 172.16.238.12:8087 check
```

`systemctl enable haproxy`

`systemctl start haproxy`

`systemctl status haproxy`
