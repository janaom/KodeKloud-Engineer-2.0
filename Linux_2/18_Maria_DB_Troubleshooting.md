# Instructions

There is a critical issue going on with the `Nautilus` application in `Stratos DC`. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.

Look into the issue and fix the same.

# Solution

`ssh peter@stdb01`

`sudo su -`

`systemctl status mariadb`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/8c52c1ce-f8c5-4ea1-aa93-7a42daf33941)

`cd /bin`

`sudo yum remove mariadb-server`

`sudo yum install mariadb-server`

`systemctl start mariadb`

`systemctl status  mariadb`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/d1a08a7f-0215-467d-b0b9-8940970aff48)

