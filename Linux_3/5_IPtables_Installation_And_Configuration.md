# Instructions

We have one of our websites up and running on our `Nautilus` infrastructure in `Stratos DC`. Our security team has raised a concern that right now Apache’s port i.e `5000`is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:

1. Install `iptables` and all its dependencies on each app host.

2. Block incoming port `5000` on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.


# Solution

`ssh tony@stapp01`

`ssh steve@stapp02`

`ssh banner@stapp03`

`sudo su -`

`sudo yum install iptables-services -y`

`sudo systemctl start iptables`

`sudo systemctl enable iptables`

`sudo systemctl status iptables`

`sudo iptables -nvL`

`sudo iptables -R INPUT 5 -p tcp --dport 5000 -s 172.16.238.14 -j ACCEPT`

`sudo iptables -A INPUT -p tcp --dport 5000 -j DROP`

`sudo service iptables save`

`curl 172.16.238.10:5000`

`curl 172.16.238.11:5000`

`curl 172.16.238.12:5000`
