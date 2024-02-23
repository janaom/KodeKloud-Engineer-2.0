# Instructions


Some users of the monitoring app have reported issues with xFusionCorp Industries mail server. They have a mail server in Stork DC where they are using postfix mail transfer agent. Postfix service seems to fail. Try to identify the root cause and fix it.

# Solution

`ssh groot@stmail01`

`sudo su -`

`systemctl start postfix`

`systemctl status postfix -l`

`cat /etc/postfix/main.cf |grep inet_interface`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/fdf054d7-8a41-45a6-a0ae-1b19ec9b3bec)

Change this line:

```
#inet_interfaces = localhost
```

`systemctl start postfix`

`systemctl status postfix`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/85ff84a9-53e5-411e-b80f-02781aeff4c6)

Check the service:

`telnet stmail01 25`

