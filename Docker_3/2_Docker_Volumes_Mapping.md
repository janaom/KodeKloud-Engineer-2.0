# Instructions

The Nautilus DevOps team is testing applications containerization, which issupposed to be migrated on docker container-based environments soon. In today's stand-up meeting one of the team members has been assigned a task to create and test a docker container with certain requirements. Below are more details:

a. On `App Server  1` in `Stratos DC` pull `nginx` image (preferably `latest` tag but others should work too).

b. Create a new container with name `news` from the image you just pulled.

c. Map the host volume `/opt/dba` with container volume `/home`. There is an `sample.txt` file present on same server under `/tmp`; copy that file to  `/opt/dba`. Also please keep the container in running state.

# Solution

`ssh tony@stapp01`

`sudo su -`

`docker pull nginx:latest`

`cp /tmp/sample.txt /opt/dba`

`docker run  --name news -v /opt/dba:/home -d -it nginx:latest`

`docker ps`

```bash
[root@stapp01 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
e76b7da34d4b        nginx:latest        "/docker-entrypoint.…"   23 seconds ago      Up 19 seconds       80/tcp              news
```

`docker exec -it news bash`

`cd /home`

`ls`

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@stapp01 ~]# docker run  --name news -v /opt/dba:/home -d -it nginx:latest
e76b7da34d4b840c1bc45b70043b2617436e791c0ad23f50ad2b70ab5d896221
[root@stapp01 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
e76b7da34d4b        nginx:latest        "/docker-entrypoint.…"   23 seconds ago      Up 19 seconds       80/tcp              news
[root@stapp01 ~]# docker exec -it news bash
root@e76b7da34d4b:/# cd /home
root@e76b7da34d4b:/home# ls
sample.txt
root@e76b7da34d4b:/home#
```
