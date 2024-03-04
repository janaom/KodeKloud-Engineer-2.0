# Instructions

The `Nautilus` DevOps team is ready to launch a new application, which they will deploy on app servers in `Stratos Datacenter`. They are expecting significant traffic/usage of `httpd` on app servers after that. This will generate massive logs, creating huge log files. To utilise the storage efficiently, they need to compress the log files and need to rotate old logs. Check the requirements shared below:

a. In all app servers install `httpd` package.

b. Using `logrotate` configure `httpd` logs rotation to monthly and keep only 3 rotated logs.

(If by default log rotation is set, then please update configuration as needed)

# Solution

`ssh tony@stapp01`

`ssh steve@stapp02`

`ssh banner@stapp03`

`sudo su -`

`yum install -y httpd`

`vi /etc/logrotate.d/httpd`

Change as per task

```python
/var/log/httpd/*log {
    monthly
    rotate 3
    missingok
    notifempty
    sharedscripts
    delaycompress
    postrotate
        /bin/systemctl reload httpd.service > /dev/null 2>/dev/null || true
    endscript
}
~
```

`systemctl start httpd`

`systemctl status httpd`
