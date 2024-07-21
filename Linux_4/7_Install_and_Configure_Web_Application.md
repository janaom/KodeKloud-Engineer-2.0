# Instructions

xFusionCorp Industries is planning to host two static websites on their infra in `Stratos Datacenter`. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:

a. Install `httpd` package and dependencies on `app server 1`.

b. Apache should serve on port `5001`.

c. There are two website's backups `/home/thor/beta` and `/home/thor/cluster` on `jump_host`. Set them up on Apache in a way that `beta` should work on the link `http://localhost:5001/beta/` and `cluster` should work on link `http://localhost:5001/cluster/` on the mentioned app server.

d. Once configured you should be able to access the website using `curl` command on the respective app server, i.e `curl http://localhost:5001/beta/` and `curl http://localhost:5001/cluster/`

# Solution

`ssh tony@stapp01`

`sudo su -`

`yum install openssh-clients -y`

`yum install -y httpd`

`cd /etc/httpd/conf`

`sed -i 47s/80/5001/ httpd.conf`

```bash
[root@stapp01 conf]# grep -n -i listen httpd.conf
37:# Listen: Allows you to bind Apache to specific IP addresses and/or
41:# Change this to Listen on a specific IP address, but note that if
46:#Listen 12.34.56.78:80
47:Listen 80
[root@stapp01 conf]# sed -i 47s/80/5001/ httpd.conf
[root@stapp01 conf]# grep -n -i listen httpd.conf
37:# Listen: Allows you to bind Apache to specific IP addresses and/or
41:# Change this to Listen on a specific IP address, but note that if
46:#Listen 12.34.56.78:80
47:Listen 5001
```

`systemctl enable httpd && systemctl start httpd && systemctl status httpd`

```bash
[root@stapp01 conf]# systemctl enable httpd && systemctl start httpd && systemctl status httpd
Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/lib/systemd/system/httpd.service.
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
     Active: active (running) since Sun 2024-07-21 08:13:20 UTC; 40ms ago
       Docs: man:httpd.service(8)
   Main PID: 3422 (httpd)
     Status: "Started, listening on: port 5001"
      Tasks: 5 (limit: 1340692)
     Memory: 7.3M
     CGroup: /docker/9fc20eb9250817d8cc4b8bd05f040e35e919f99aab39126f2f7deb737a66c619/system.slice/httpd.service
             ├─3422 /usr/sbin/httpd -DFOREGROUND
             ├─3429 /usr/sbin/httpd -DFOREGROUND
             ├─3430 /usr/sbin/httpd -DFOREGROUND
             ├─3431 /usr/sbin/httpd -DFOREGROUND
             └─3432 /usr/sbin/httpd -DFOREGROUND

Jul 21 08:13:20 stapp01.stratos.xfusioncorp.com systemd[1]: Starting The Apache HTTP Server...
Jul 21 08:13:20 stapp01.stratos.xfusioncorp.com httpd[3422]: AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using stapp01.stratos.xfusioncorp.com. Set the 'ServerName' directive globally to suppress this message
Jul 21 08:13:20 stapp01.stratos.xfusioncorp.com httpd[3422]: Server configured, listening on: port 5001
Jul 21 08:13:20 stapp01.stratos.xfusioncorp.com systemd[1]: Started The Apache HTTP Server.
```

Go back to thor

```bash
thor@jumphost ~$ sudo scp -r /home/thor/beta tony@172.16.238.10:/tmp
index.html                                                                                  100%  117   233.7KB/s   00:00 
thor@jumphost ~$ sudo scp -r /home/thor/cluster tony@172.16.238.10:/tmp
index.html                                                                                  100%  120   531.6KB/s   00:00
```

Go back to tony, if you are still inside /tmp, run: `mv beta cluster /var/www/html`

```bash
[root@stapp01 tmp]# mv beta cluster /var/www/html 
[root@stapp01 tmp]# cd /var/www/html
[root@stapp01 html]# ls
beta  cluster
```

`curl -4 http://stapp01:5001/beta/`

`curl -4 http://stapp01:5001/cluster/`

```bash
thor@jumphost ~$ curl http://stapp01:5001/beta/
<!DOCTYPE html>
<html>
<body>

<h1>KodeKloud</h1>

<p>This is a sample page for our beta website</p>

</body>
</html>thor@jumphost ~$ curl http://stapp01:5001/cluster/
<!DOCTYPE html>
<html>
<body>

<h1>KodeKloud</h1>

<p>This is a sample page for our cluster website</p>

</body>
</html>thor@jumphost ~$
```


