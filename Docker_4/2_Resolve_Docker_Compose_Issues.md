# Instructions

The Nautilus DevOps team is working to deploy one of the applications on `App Server 3` in `Stratos DC`.Due to a misconfiguration in the docker compose file, the deployment is failing. We would like you to take a look into it to identify and fix the issues. More details can be found below:

a. `docker-compose.yml` file is present on `App Server 3` under `/opt/docker` directory.

b. Try to run the same and make sure it works fine.

c. Please do not change the `container names` being used. Also, do not update or alter any other valid config settings in the compose file or any other relevant data that can cause app failure.

`Note:` Please note that once you click on `FINISH` button all existing running/stopped containers will be destroyed, and your compose will be run.

# Solution

`ssh banner@stapp03`

`sudo su -`

`cd /opt/docker`

`cat docker-compose.yml`

```bash
[root@stapp03 docker]# cat docker-compose.yml 
name: myapp

services:
    web:
        build: ./app
        container_name: python
        ports:
            - "5000:5000"
        volume:
            - ./app:/code
        depends:
            - redis
    redis:
        build: redis
        container_name: redis
```

`vi docker-compose.yml`

```bash
version: '3'
services:
  web:
    build:
        context: app
        dockerfile: Dockerfile
    container_name: python
    ports:
      - "5000:5000"
    volumes:
      - ./app:/code
    depends_on:
      - redis_app
  redis_app:
    image: redis
    container_name: redis
```

`yum install epel-release`

`yum install docker-compose`

`docker-compose up -d`

`docker ps`

```bash
[root@stapp03 docker]# docker ps
CONTAINER ID   IMAGE        COMMAND                  CREATED          STATUS          PORTS                    NAMES
21270c74a095   docker_web   "/bin/sh -c 'python …"   43 seconds ago   Up 39 seconds   0.0.0.0:5000->5000/tcp   python
c12bf5e9319b   redis        "docker-entrypoint.s…"   46 seconds ago   Up 43 seconds   6379/tcp                 redis
```

`curl -ik http://localhost:5000`

```bash
[root@stapp03 docker]# curl -ik http://localhost:5000
HTTP/1.1 200 OK
Server: Werkzeug/3.0.3 Python/3.13.0b1
Date: Wed, 22 May 2024 16:38:37 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 53
Connection: close

This Compose/Flask demo has been viewed b'3' time(s)
```
