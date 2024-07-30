# Instructions

The `Nautilus` application development team has shared that they are planning to deploy one newly developed application on `Nautilus` infra in `Stratos DC`. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:

PostgreSQL database server is already installed on the `Nautilus` database server.

a. Create a database user `kodekloud_gem` and set its password to `LQfKeWWxWD`.

b. Create a database `kodekloud_db6` and grant full permissions to user `kodekloud_gem` on this database.

`Note:` Please do not try to restart PostgreSQL server service.

# Solution

`ssh peter@stdb01`

`sudo su -`

`yum install -y postgresql-server postgresql-contrib`

`postgresql-setup initdb`

If you are getting errors, follow these steps:

```bash
[root@stdb01 ~]# postgresql-setup initdb
WARNING: using obsoleted argument syntax, try --help
WARNING: arguments transformed to: postgresql-setup --initdb --unit postgresql
 * Initializing database in '/var/lib/pgsql/data'
ERROR: Data directory /var/lib/pgsql/data is not empty!
ERROR: Initializing database failed, possibly see /var/lib/pgsql/initdb_postgresql.log
[root@stdb01 ~]# ^C
[root@stdb01 ~]# postgresql-setup --initdb --unit postgresql
 * Initializing database in '/var/lib/pgsql/data'
ERROR: Data directory /var/lib/pgsql/data is not empty!
ERROR: Initializing database failed, possibly see /var/lib/pgsql/initdb_postgresql.log
[root@stdb01 ~]# systemctl stop postgresql
[root@stdb01 ~]# mv /var/lib/pgsql/data /var/lib/pgsql/data.backup
[root@stdb01 ~]# postgresql-setup --initdb --unit postgresql
 * Initializing database in '/var/lib/pgsql/data'
 * Initialized, logs are in /var/lib/pgsql/initdb_postgresql.log
[root@stdb01 ~]#
```
`systemctl enable postgresql && systemctl start postgresql && systemctl status postgresql`

`sudo -u postgres psql`

`create user kodekloud_gem with encrypted password 'LQfKeWWxWD';`

`create database kodekloud_db6 owner kodekloud_gem;`

`grant all privileges on database kodekloud_db6 to kodekloud_gem;`

```bash
[root@stdb01 ~]# sudo -u postgres psql
postgres=# create user kodekloud_top with encrypted password 'LQfKeWWxWD'; 
CREATE ROLE
postgres=# create database kodekloud_db1 owner kodekloud_top;
CREATE DATABASE
postgres=# grant all privileges on database kodekloud_db1 to kodekloud_top;
GRANT
postgres=# exit
[root@stdb01 ~]#
```

 `vi /var/lib/pgsql/data/postgresql.conf`

 ```bash
[root@stdb01 ~]# cat /var/lib/pgsql/data/postgresql.conf |grep localhost
#listen_addresses = 'localhost'         # what IP address(es) to listen on;
                                        # defaults to 'localhost'; use '*' for all
[root@stdb01 ~]# vi /var/lib/pgsql/data/postgresql.conf
[root@stdb01 ~]# cat /var/lib/pgsql/data/postgresql.conf |grep localhost
listen_addresses = 'localhost'          # what IP address(es) to listen on;
                                        # defaults to 'localhost'; use '*' for all
```

`vi /var/lib/pgsql/data/pg_hba.conf`

```bash
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
```

`systemctl restart postgresql`

`systemctl status postgresql`

```bash
[root@stdb01 ~]# psql -U kodekloud_gem  -d kodekloud_db6 -h 127.0.0.1 -W
Password: 
psql (13.14)
Type "help" for help.

kodekloud_db6=> exit
[root@stdb01 ~]#  psql -U kodekloud_gem  -d kodekloud_db6 -h localhost -W
Password: 
psql (13.14)
Type "help" for help.

kodekloud_db6=> exit
[root@stdb01 ~]# 
```
