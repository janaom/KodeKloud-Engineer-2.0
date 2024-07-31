# Instructions

We need to setup a database server on `Nautilus DB Server` in `Stratos Datacenter`. Please perform the below given steps on DB Server:

a. Install/Configure MariaDB server.

b. Create a database named `kodekloud_db2`.

c. Create a user called `kodekloud_joy` and set its password to `8FmzjvFU6S`.

d. Grant full permissions to user `kodekloud_joy` on database `kodekloud_db2`.

# Solution

`ssh peter@stdb01`

`sudo su -`

`yum install openssh-clients -y`

`yum install -y mariadb-server`

`yum install -y mariadb*`

```bash
[root@stdb01 ~]# yum install -y mariadb
Last metadata expiration check: 0:00:45 ago on Wed Jul 31 10:40:23 2024.
Package mariadb-3:10.5.22-1.el9.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
```

`systemctl restart mariadb`

`systemctl enable mariadb && systemctl start mariadb && systemctl status mariadb`

`sudo mysql_secure_installation`  

```bash
[root@stdb01 ~]# sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y     --Not sure how is should be, but I entered pass '8FmzjvFU6S' here
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] n
 ... skipping.

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
[root@stdb01 ~]#
```

`mysql -u root -p`

```bash
create database kodekloud_db2;
use kodekloud_db2;
create user kodekloud_joy identified by '8FmzjvFU6S'; #if you make a mistake use ALTER, e.g. ALTER USER 'kodekloud_joy'@localhost IDENTIFIED BY '8FmzjvFU6S';
select host, user, password from mysql.user;
grant all on kodekloud_db2.* to 'kodekloud_joy' IDENTIFIED BY '8FmzjvFU6S' WITH GRANT OPTION;
grant all privileges on kodekloud_db2.* to 'kodekloud_joy'@'%' WITH GRANT OPTION;
exit
  
  #Since I made a mistake in the password, but created the correct db, I went for these:
ALTER USER 'kodekloud_joy'@localhost IDENTIFIED BY '8FmzjvFU6S';
grant all on kodekloud_db2.* to 'kodekloud_joy' IDENTIFIED BY '8FmzjvFU6S' WITH GRANT OPTION;
grant all privileges on kodekloud_db2.* to 'kodekloud_joy'@'%' WITH GRANT OPTION;
```


Tbh, not sure if this part is necessary

`vi /etc/my.cnf`

```bash
[mysqld]
bind-address = 172.16.239.10

[client]
port = 3306
```

`systemctl restart mysql`

`mysql -u kodekloud_joy -p -h stdb01`

`mysql -u kodekloud_joy -p -h localhost`

```bash
[root@stdb01 peter]# mysql -u kodekloud_joy -p -h stdb01
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 10.5.22-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> exit
Bye
[root@stdb01 peter]# mysql -u kodekloud_joy -p -h localhost
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 10.5.22-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```
