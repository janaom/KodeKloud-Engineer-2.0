# Instructions

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in `Stratos DC`.

Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not hosted any code yet on these servers so you need not to worry about if Apache isn't serving any pages or not, just make sure service is up and running. Also, make sure Apache is running on port `3003` on all app servers.

# Solution

`ssh tony@stapp01`

`ssh steve@stapp02`

`ssh banner@stapp03`

`sudo su -`

`systemctl start httpd`

Issue on the first app

```bash
[root@stapp01 ~]# systemctl start httpd
Job for httpd.service failed because the control process exited with error code. See "systemctl status httpd.service" and "journalctl -xe" for details.
```

`systemctl status httpd -l`

`netstat -ntpl`

```bash
[root@stapp01 ~]# netstat -ntpl
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:3003          0.0.0.0:*               LISTEN      553/sendmail: accep 
tcp        0      0 127.0.0.11:39261        0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/init              
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      429/sshd            
tcp6       0      0 :::111                  :::*                    LISTEN      412/rpcbind         
tcp6       0      0 :::22                   :::*                    LISTEN      429/sshd
```

`kill 553`

`systemctl start httpd`

`systemctl enable httpd`

```bash
[root@stapp01 ~]# netstat -ntpl
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:3003            0.0.0.0:*               LISTEN      649/httpd           
tcp        0      0 127.0.0.11:39261        0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/init              
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      429/sshd            
tcp6       0      0 :::111                  :::*                    LISTEN      412/rpcbind         
tcp6       0      0 :::22                   :::*                    LISTEN      429/sshd
```

`curl localhost:3003`
