# Instructions

The xFusionCorp Industries security team recently did a security audit of their infrastructure and came up with ideas to improve the application and server security. They decided to use SElinux for an additional security layer. They are still planning how they will implement it; however, they have decided to start testing with app servers, so based on the recommendations they have the following requirements:

Install the required packages of SElinux on `App server 2` in `Stratos Datacenter`and disable it permanently for now; it will be enabled after making some required configuration changes on this host. Don't worry about rebooting the server as there is already a reboot scheduled for tonight's maintenance window. Also ignore the status of SElinux command line right now; the final status after reboot should be `disabled`.

# Solution

ssh into the App Server 2: `ssh steve@172.16.238.11`

Run the following command to update the package manager and ensure you have the latest package information: `sudo yum update`

Install the required SELinux packages by running the following command: `sudo yum install selinux-policy selinux-policy-targeted`

To disable SELinux permanently, you need to modify the SELinux configuration file: `sudo vi /etc/selinux/config`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c8edad1d-bda4-4194-b8a8-a561ad25a496)

Change `SELINUX=enforcing` to `SELINUX=disabled`

Check the status: `sestatus`

You should see: `SELinux status: disabled`
