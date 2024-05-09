# Instructions

Some of the developers from `Nautilus` project team have asked for SFTP access to at least one of the app server in `Stratos DC`. After going through the requirements,  the system admins team has decided to configure the SFTP server on `App Server 1` server in `Stratos Datacenter`. Please configure it as per the following instructions:

a. Create a SFTP user `javed` and set its password to `BruCStnMT5`. There is already a group called `ftp`, you can utilise the same.

b. Password authentication should be enabled for this user.

c. SFTP user should only be allowed to make SFTP connections.

# Solution

`ssh tony@stapp01`

`sudo su -`

`sudo useradd javed  -m -G ftp`

`sudo passwd javed`

`vi /etc/ssh/sshd_config`

```bash
Subsystem sftp internal-sftp
Match User javed
	ForceCommand internal-sftp
	PasswordAuthentication yes
	ChrootDirectory %h
	PermitTunnel no
	AllowAgentForwarding no
	AllowTcpForwarding no
	X11Forwarding no
```

`sudo systemctl restart sshd`

`sftp javed@localhost`
