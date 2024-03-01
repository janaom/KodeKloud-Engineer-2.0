# Instructions

`xFusionCorp Industries`uses some monitoring tools to check the status of every service, application, etc running on the systems. Recently, the monitoring system identified that Apache service is not running on some of the `Nautilus Application Servers` in `Stratos Datacenter`.

1. Identify the faulty `Nautilus Application Server` and fix the issue.  Also, make sure Apache service is up and running on all `Nautilus Application Servers`. Do not try to stop any kind of firewall that is already running.

2. Apache is running on `8089` port on all `Nautilus Application Servers` and its document root must be `/var/www/html` on all app servers.

3. Finally you can test from `jump host` using curl command to access Apache on all app servers and it should be reachable and you should get some static page. E.g. `curl http://172.16.238.10:8089/`.

# Solution

`ssh tony@stapp01`

`ssh steve@stapp02`

`ssh banner@stapp03`

`sudo su -`

`systemctl start httpd`

You should see a similar message

```python
Job for httpd.service failed because the control process exited with error code.
See "systemctl status httpd.service" and "journalctl -xe" for details.
```

`systemctl status  httpd.service`

```python
AH00526: Syntax error on line 45 of /etc/httpd/conf/httpd.conf:
Invalid command 'Listen 8089', perhaps misspelled or defined by a module not included in the server configura>

<...>

AH00526: Syntax error on line 122 of /etc/httpd/conf/httpd.conf:
DocumentRoot '/var/www/html;' is not a directory, or is not readable
```

`vi /etc/httpd/conf/httpd.conf`

Fix your errors

`systemctl start httpd  &&  systemctl  status httpd`

`curl http://172.16.238.10:8089/`
