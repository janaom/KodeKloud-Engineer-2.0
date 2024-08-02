# Instructions

xFusionCorp Industries is planning to host a `WordPress` website on their infra in `Stratos Datacenter`. They have already done infrastructure configuration—for example, on the storage server they already have a shared directory `/vaw/www/html` that is mounted on each app host under `/var/www/html` directory. Please perform the following steps to accomplish the task:

a. Install httpd, php and its dependencies on all app hosts.

b. Apache should serve on port `6200` within the apps.

c. Install/Configure `MariaDB server` on DB Server.

d. Create a database named `kodekloud_db2` and create a database user named `kodekloud_cap` identified as password `GyQkFRVNr3`. Further make sure this newly created user is able to perform all operation on the database you created.

e. Finally you should be able to access the website on LBR link, by clicking on the `App` button on the top bar. You should see a message like `App is able to connect to the database using user kodekloud_cap`

# Solution

On DB Server:

`ssh peter@stdb01`

`yum install mariadb-server`

`systemctl start mariadb`

`systemctl enable mariadb`

`systemctl status mariadb`

`mysql -u root -p`

```bash
create database kodekloud_db2;
use kodekloud_db2;
create user kodekloud_cap identified by 'GyQkFRVNr3'; 
select host, user, password from mysql.user;
grant all on kodekloud_db2.* to 'kodekloud_cap' IDENTIFIED BY 'GyQkFRVNr3' WITH GRANT OPTION;
grant all privileges on kodekloud_db2.* to 'kodekloud_cap'@'%' WITH GRANT OPTION;
exit
```

```bash
[root@stdb01 ~]# mysql -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 10.5.22-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database kodekloud_db2;
Query OK, 1 row affected (0.000 sec)

MariaDB [(none)]> use kodekloud_db2;
Database changed
MariaDB [kodekloud_db2]> create user kodekloud_cap identified by 'GyQkFRVNr3'; 
Query OK, 0 rows affected (0.001 sec)

MariaDB [kodekloud_db2]> select host, user, password from mysql.user;
+-----------+---------------+-------------------------------------------+
| Host      | User          | Password                                  |
+-----------+---------------+-------------------------------------------+
| localhost | mariadb.sys   |                                           |
| localhost | root          | invalid                                   |
| localhost | mysql         | invalid                                   |
| %         | kodekloud_cap | *FF0F3C6657DD0E5CD304C0364FFA63CBC03493D8 |
+-----------+---------------+-------------------------------------------+
4 rows in set (0.001 sec)

MariaDB [kodekloud_db2]> grant all on kodekloud_db2.* to 'kodekloud_cap' IDENTIFIED BY 'GyQkFRVNr3' WITH GRANT OPTION;
Query OK, 0 rows affected (0.001 sec)

MariaDB [kodekloud_db2]> grant all privileges on kodekloud_db2.* to 'kodekloud_cap'@'%' WITH GRANT OPTION;
Query OK, 0 rows affected (0.001 sec)

MariaDB [kodekloud_db2]>
```

`systemctl restart mariadb`

`systemctl status mariadb`

On each App server:

`ssh tony@stapp01`

`ssh steve@stapp02`

`ssh banner@stapp03`

`sudo su -`

Run these commands for each app:

`yum install -y net-tools mc`

`yum install -y httpd php.x86_64 php-mysqlnd.x86_64`

`vi /etc/httpd/conf/httpd.conf`

Change:

Listen 6200

`systemctl restart httpd`

Check the results:

```bash
[root@stapp01 ~]# curl http://stlb01
App is able to connect to the database using user kodekloud_cap
[root@stapp02 ~]# curl http://stlb01
App is able to connect to the database using user kodekloud_cap
[root@stapp03 ~]# curl http://stlb01
App is able to connect to the database using user kodekloud_cap
```

![image](https://github.com/user-attachments/assets/f8673d69-ddbe-4616-a014-125f5108ea58)

