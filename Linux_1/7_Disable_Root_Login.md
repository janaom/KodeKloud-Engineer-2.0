# Instructions

After doing some security audits of servers, `xFusionCorp Industries` security team has implemented some new security policies. One of them is to disable direct root login through SSH.

Disable direct SSH root login on all app servers in `Stratos Datacenter`.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

ssh into the App Server 2: `ssh steve@172.16.238.11`

ssh into the App Server 3: `ssh banner@172.16.238.12`

Open the SSH server configuration file (usually located at `/etc/ssh/sshd_config`) using a text editor such as `vi:` `sudo vi /etc/ssh/sshd_config`

Modify the value of `PermitRootLogin` to `no` or `without-password`. This disables direct root login.

- Setting it to `no` will completely disable root login.
- Setting it to `without-password` will allow root login only with public key authentication.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a389993a-5f50-4dec-8a51-ae07424b1aa7)

Restart the SSH server daemon (`sshd`)  `sudo systemctl restart sshd`

When you run `sudo systemctl restart sshd`, it will stop the SSH server daemon if it is currently running and then start it again. This command is often used after making changes to the SSH server configuration file (`sshd_config`) to apply the new configuration without requiring a system reboot. Restarting the SSH server ensures that the changes take effect and that the updated configuration is loaded.
