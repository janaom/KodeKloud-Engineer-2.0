# Instructions

As per details shared by the development team, the new application release has some dependencies on the back end. There are some packages/services that need to be installed on all app servers under `Stratos Datacenter`. As per requirements please perform the following steps:

a. Install `postfix` package on all the application servers.

b. Once installed, make sure it is enabled to start during boot.

# Solution

ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03

Run the following command to install the `postfix` package: `sudo yum install postfix`

After the installation completes successfully, enable the `postfix` service to start automatically during boot. Use the following command: `sudo systemctl enable postfix`

To check if the `postfix` package is installed, run the following command: `rpm -q postfix`

To check if the `postfix` service is enabled to start during boot, run the following command: `systemctl is-enabled postfix`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/114e3e1a-b335-474c-8001-2f9116532b23)
