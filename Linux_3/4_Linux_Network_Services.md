# Instructions

Our monitoring tool has reported an issue in `Stratos Datacenter`. One of our app servers has an issue, as its Apache service is not reachable on port `3000`(which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the 
issue.

Use tools like `telnet`, `netstat`, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command `curl http://stapp01:3000` command from jump host.

# Solution

Check which app has an issue:

`sudo curl 172.16.238.10:3000`

`sudo curl 172.16.238.11:3000`           

`sudo curl 172.16.238.12:3000`   

In my case it was stapp01

`ssh tony@stapp01`

`sudo su -`

`systemctl status httpd &&  systemctl start httpd`

```bash
May 12 11:12:51 stapp01.stratos.xfusioncorp.com httpd[783]: (98)Address already in use: AH00072: make_sock: could not bind to 
address 0.0.0.0:3000
```

`netstat -tulnp`

```bash
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      568/sshd            
tcp        0      0 127.0.0.1:3000          0.0.0.0:*               LISTEN      722/sendmail: accep 
tcp        0      0 127.0.0.11:45349        0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      568/sshd            
udp        0      0 127.0.0.11:52284        0.0.0.0:*                           -
```

`kill 722`

`systemctl start httpd`

`systemctl enable httpd`

`systemctl status -l httpd`

`iptables -I INPUT -p tcp --dport 3000 -j ACCEPT`

`service iptables save`

Check the result: `curl http://stapp01:3000`
