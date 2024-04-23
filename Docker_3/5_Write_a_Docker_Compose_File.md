# Instructions

The Nautilus application development team shared static website content that needs to be hosted on the `httpd`web server using a containerised platform. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines. Below are the details:

a. On `App Server 2` in `Stratos DC` create a container named `httpd` using a docker compose file `/opt/docker/docker-compose.yml` (please use the exact name for file).

b. Use `httpd` (preferably `latest` tag) image for container and make sure container is named as `httpd`; you can use any name for service.

c. Map `80` number port of container with port `8087` of docker host.

d. Map container's `/usr/local/apache2/htdocs` volume with `/opt/dba` volume of docker host which is already there. (please do not modify any data within these locations).


# Solution

a. Create a Docker Compose file:

Create a file named `/opt/docker/docker-compose.yml` and add the following content to it:

```bash
version: '3'
services:
  httpd:
    container_name: httpd
    image: httpd:latest
    ports:
      - 8087:80
    volumes:
      - /opt/dba:/usr/local/apache2/htdocs
```

b. Start the container using Docker Compose:

Run the following command to start the container using the Docker Compose file: `docker-compose -f /opt/docker/docker-compose.yml up -d`

After executing these steps, the container named "httpd" will be created and running on App Server 2, with port 80 of the container mapped to port 8087 of the Docker host. The static website content shared by the development team will be hosted on the httpd web server within the container, using the mapped volume between the container and the Docker host.

```bash
[root@stapp02 ~]# vi /opt/docker/docker-compose.yml
[root@stapp02 ~]# docker-compose -f /opt/docker/docker-compose.yml up -d
Creating network "docker_default" with the default driver
Pulling httpd (httpd:latest)...
latest: Pulling from library/httpd
13808c22b207: Pull complete
6e9a8835eae4: Pull complete
4f4fb700ef54: Pull complete
b927d001db70: Pull complete
559cc51378ed: Pull complete
d2b091e65160: Pull complete
Digest: sha256:b19cace6539a05579c55fda6be0a873c1d2c2e7392e7c08805141f79852ab07b
Status: Downloaded newer image for httpd:latest
Creating httpd ... done
[root@stapp02 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS                  NAMES
b5f8be36b39d        httpd:latest        "httpd-foreground"   6 minutes ago       Up 5 minutes        0.0.0.0:8087->80/tcp   httpd
[root@stapp02 ~]#
```
