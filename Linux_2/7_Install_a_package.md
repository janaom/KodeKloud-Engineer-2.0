# Instructions

As per new application requirements shared by the `Nautilus` project development team, serveral new packages need to be installed on all app servers in `Stratos Datacenter`. Most of them are completed except for `epel-release`.

Therefore, install the  `epel-release` package on all `app-servers`.

# Solution

ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03

Once connected to a server, execute the appropriate command to install the `epel-release` package: `sudo yum install epel-release`

Check the result: `rpm -q epel-release`
