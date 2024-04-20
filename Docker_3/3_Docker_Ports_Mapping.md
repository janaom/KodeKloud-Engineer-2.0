# Instructions

The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on `Application Server 1` in `Stratos Datacenter`. Please perform the task as per details mentioned below:

a. Pull `nginx:alpine-perl` docker image on `Application Server 1`.

b. Create a container named `beta` using the image you pulled.

c. Map host port  `5000` to container port `80`. Please keep the container in running state.

# Solution

`ssh tony@stapp01`

`docker pull nginx:alpine-perl`

`docker run -d --name beta -p 5000:80 nginx:alpine-perl`

This command will start a new container named "beta" using the nginx image. The `-d` flag indicates that the container should run in the background. The `-p` flag specifies the port mapping, where `5000` is the host port and `80` is the container port. This means that incoming traffic on port 5000 of the host will be forwarded to port 80 of the container.

`docker ps`

```bash
[tony@stapp01 ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
9248673dc915        nginx:alpine-perl   "/docker-entrypoint.…"   15 seconds ago      Up 13 seconds       0.0.0.0:5000->80/tcp   beta
```
