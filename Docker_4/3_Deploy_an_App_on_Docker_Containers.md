# Instructions

The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose 
fie. Below are the details of the task:

1. On `App Server 1` in `Stratos Datacenter` create a docker compose file `/opt/security/docker-compose.yml` (should be named exactly).
2. The compose should deploy two services (web and DB), and each service should deploy a container as per details below:

`For web service:`

a. Container name must be `php_host`.

b. Use image `php` with any `apache` tag. Check here for more details.

c. Map `php_host` container's port `80` with host port `3003`

d. Map `php_host` container's `/var/www/html` volume with host volume `/var/www/html`.

`For DB service:`

a. Container name must be `mysql_host`.

b. Use image `mariadb` with any tag (preferably `latest`). Check here for more details.

c. Map `mysql_host` container's port `3306` with host port `3306`

d. Map `mysql_host` container's `/var/lib/mysql` volume with host volume `/var/lib/mysql`.

e. Set MYSQL_DATABASE=`database_host` and use any custom user ( except root ) with some complex password for DB connections.

1. After running docker-compose up you can access the app with curl command `curl <server-ip or hostname>:3003/`

For more details check here.

`Note:` Once you click on `FINISH` button, all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.

# Solution

`ssh tony@stapp01`

`sudo su -`

`cd /opt/security`

`vi docker-compose.yml`

```yaml
version: '3'
services:

  web:
    container_name: php_host
    image: php:apache
    depends_on:
      - db
    ports:
      - "3003:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    container_name: mysql_host
    image: mariadb:latest
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_host
      MYSQL_USER: myuser
      MYSQL_PASSWORD: MyComplexPassword123!
      MYSQL_ROOT_PASSWORD: Password123
```

`yum install epel-release`

`yum install docker-compose`

`docker-compose up -d`

`curl http://localhost:3003`

```bash
[root@stapp01 security]# docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                    NAMES
a4b88822ecb8   php:apache       "docker-php-entrypoi…"   26 seconds ago   Up 6 seconds    0.0.0.0:3003->80/tcp     php_host
329140ec5591   mariadb:latest   "docker-entrypoint.s…"   44 seconds ago   Up 28 seconds   0.0.0.0:3306->3306/tcp   mysql_host
[root@stapp01 security]# curl http://localhost:3003
<html>
    <head>
        <title>Welcome to xFusionCorp Industries!</title>
    </head>

    <body>
        Welcome to xFusionCorp Industries!    </body>
</html>[root@stapp01 security]#
```
