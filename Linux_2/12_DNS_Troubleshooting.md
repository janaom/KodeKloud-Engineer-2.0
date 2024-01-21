# Instructions

The system admins team of xFusionCorp Industries has noticed intermittent issues with DNS resolution in several apps . `App Server 2` in `Stratos Datacenter` is having some DNS resolution issues, so we want to add some additional DNS nameservers on this server.

As a temporary fix we have decided to go with Google public DNS (ipv4). Please make appropriate changes on this server.

# Solution

ssh steve@stapp02

Open the network configuration file for editing. The location of the network configuration file may vary depending on the Linux distribution and version, but it is commonly found at `/etc/resolv.conf`. Use the following command to open the file in a text editor with superuser (sudo) privileges: `sudo vi /etc/resolv.conf`

Add the following lines below the existing `nameserver` line to add the Google public DNS nameserver: `nameserver 8.8.8.8`

After making these changes, the App Server 2 in the Stratos Datacenter should start using the Google public DNS nameservers for DNS resolution. This can help address the intermittent DNS resolution issues in the apps on that server.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/9f5aef88-2395-4561-9444-c1362e1fb199)

To test the DNS resolution, you can use commands like `ping` or `nslookup` to check if DNS queries are resolving successfully. For example: `sudo ping google.com`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f8b27fe1-ceec-4057-b79c-2cf8a743e89d)

