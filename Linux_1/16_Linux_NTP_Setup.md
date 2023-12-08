# Instructions

The system admin team of `xFusionCorp Industries` has noticed an issue with some servers in `Stratos Datacenter`where some of the servers are not in sync w.r.t time. Because of this, several application functionalities have been impacted. To fix this issue the team has started using common/standard NTP servers. They are finished with most of the servers except `App Server 2`. Therefore, perform the following tasks on this server:

1. Install and configure NTP server on `App Server 2`.

2. Add NTP `server 1.sg.pool.ntp.org` in NTP configuration on `App Server 2`.

3. Please do not try to start/restart/stop `ntp` service, as we already have a restart for this service scheduled for tonight and we don't want these changes to be applied right now.

# Solution

ssh into the App Server 2: `ssh steve@172.16.238.11`

Install the NTP server software. The specific package and installation method may vary depending on the operating system you are using. This one worked for me:`sudo yum install ntp`

Check the status: `sudo systemctl list-unit-files | grep ntp`

Enable the service:

`sudo systemctl enable ntpd.service`

`sudo systemctl enable ntpdate.service`

Add NTP server 1.sg.pool.ntp.org in NTP configuration on App Server 2:`sudo vi /etc/ntp.conf`

Add the following line to the server configuration section: `server 1.sg.pool.ntp.org`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/6cfded22-d8f0-4075-a3fb-ae3921951371)

`systemctl status ntpd`

`sudo systemctl start ntpd`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b3183503-2366-4a74-aab1-ea95aded60bf)

