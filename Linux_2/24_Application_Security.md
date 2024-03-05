# Instructions

We have a backup management application UI hosted on `Nautilus's` backup server in `Stratos DC`.  That backup management application code is deployed under Apache on the backup server itself, and Nginx is running as a reverse proxy on the same server. Apache and Nginx ports are `6200` and `8097`, respectively. We have iptables firewall installed on this server. Make the appropriate changes to fulfill the requirements mentioned below:

We want to open all incoming connections to Nginx's port and block all incoming connections to Apache's port. Also make sure rules are permanent.

# Solution

`ssh clint@stbkp01`

`sudo su -`

`systemctl status iptables`

`ss -tlnp |grep http`

`ss -tlnp |grep nginx`

`systemctl start iptables`

`systemctl status iptables`

`iptables -A INPUT -p tcp --dport 8097 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT`

`iptables -A INPUT -p tcp --dport 6200 -m conntrack --ctstate NEW -j REJECT`

`iptables -L --line-numbers`

`iptables -R INPUT 5 -p icmp -j REJECT`

`service iptables save`

`cat /etc/sysconfig/iptable`

`iptables -L -n -v`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c2ee0f41-a60d-4a0a-b8bd-e4136c1faa11)
