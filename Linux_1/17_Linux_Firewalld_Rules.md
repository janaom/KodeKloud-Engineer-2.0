# Instructions

The `Nautilus` system admins team recently deployed a web UI application for their backup utility running on the `Nautilus backup server` in `Stratos Datacenter`. The application is running on port `8089`. They have `firewalld` installed on that server. The requirements that have come up include the following:

Open all incoming connection  on `8089/tcp` port.  Zone should be `public`.

# Solution

ssh into the Backup server: `ssh clint@172.16.238.16`

Run the following command to open port 8089/tcp and set the zone to public: `sudo firewall-cmd --zone=public --add-port=8089/tcp --permanent`This command adds a rule to the public zone of firewalld, allowing incoming connections on port 8089/tcp. The `--permanent` flag ensures that the rule persists across system reboots.

Reload the firewalld configuration for the changes to take effect: `sudo firewall-cmd --reload`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/4a6a43dc-43ce-412d-86de-146808039b2c)

To check if the firewall rule allowing incoming connections on port 8089/tcp has been successfully applied, you can perform the following steps:

- `curl http://172.16.238.16:8089` If you receive a response or see the web UI, it indicates that the firewall rule is allowing incoming connections on port 8089/tcp.
- or you can verify the status of the firewall rule on the Nautilus backup server by running the following command: `sudo firewall-cmd --zone=public --list-ports`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/63eb61ab-91ca-46b6-a9be-73bad789b6ae)

