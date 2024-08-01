# Instructions

The Nautilus DevOps team is working on to develop a bash script to automate some tasks. As per the requirements shared with the team database related tasks needed to be automated. Below you can find more details about the same:

1. Write a bash script named `/opt/scripts/database.sh` on `Database Server`. The mariadb database server is already installed on this server.

2. Add code in the script to perform some database related operations as per conditions given below:

a. Create a new database named `kodekloud_db01`. If this database already exists on the server then script should print a message `Database already exists` and if the database does not exist then create the same and script should print `Database kodekloud_db01 has been created`. Further, create a user named `kodekloud_roy` and set its password to `asdfgdsd`, also give full access to this user on newly created database (remember to use wildcard host while creating the user).

b. Now check if the database (if it was already there) already contains some data (tables)`if so then script should print 'database is not empty` otherwise import the database dump `/opt/db_backups/db.sql` and print `imported database dump into kodekloud_db01 database.`

c. Take a mysql dump which should be named as `kodekloud_db01.sql` and save it under `/opt/db_backups/` directory.

# Solution

`ssh peter@stdb01`

`sudo su -`

`cd /opt/scripts`

`vi database.sh`

```bash
#!/bin/bash

DB_NAME="kodekloud_db01"
DB_USER="kodekloud_roy"
DB_USER_PASS="asdfgdsd"

if mysql -u root -e "use $DB_NAME" 2>/dev/null; then
  echo "Database already exists"
else
  mysql -u root -e "create database $DB_NAME"
  echo "Database $DB_NAME has been created"

  mysql -u root -e "CREATE USER '$DB_USER'@'%' IDENTIFIED BY '$DB_USER_PASS';"
  mysql -u root -e "GRANT ALL PRIVILEGES ON $DB_NAME.* TO '$DB_USER'@'%';"
fi

if mysql -u root $DB_NAME -e "SHOW TABLES" | grep -q "."; then
  echo "database is not empty"
else
  mysql -u root $DB_NAME < /opt/db_backups/db.sql
  echo "imported database dump into $DB_NAME database."
fi

mysqldump -u root $DB_NAME > /opt/db_backups/$DB_NAME.sql
```

`chmod +x  database.sh`

`sh database.sh`

```bash
[root@stdb01 scripts]# sh database.sh 
Database already exists
imported database dump into kodekloud_db01 database.
[root@stdb01 scripts]# mysqlshow
+--------------------+
|     Databases      |
+--------------------+
| information_schema |
| kodekloud_db01     |
| mysql              |
| performance_schema |
+--------------------+
[root@stdb01 scripts]# mysqlshow kodekloud_db01 
Database: kodekloud_db01
+-----------------------+
|        Tables         |
+-----------------------+
| wp_commentmeta        |
| wp_comments           |
| wp_links              |
| wp_options            |
| wp_postmeta           |
| wp_posts              |
| wp_term_relationships |
| wp_term_taxonomy      |
| wp_termmeta           |
| wp_terms              |
| wp_usermeta           |
| wp_users              |
+-----------------------+
[root@stdb01 scripts]# ls -ls /opt/db_backups/
total 92
48 -rw-r--r-- 1 root root 46379 Aug  1 15:25 db.sql
44 -rw-r--r-- 1 root root 44917 Aug  1 15:45 kodekloud_db01.sql
```
