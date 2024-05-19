# Instructions

The Nautilus DevOps team is working to create new images per requirements shared by the development team. One of the team members is working to create a `Dockerfile` on `App Server 2` in `Stratos DC`. While working on it she ran into issues in which the docker build is 
failing and displaying errors. Look into the issue and fix it to build 
an image as per details mentioned below:

a. The `Dockerfile` is placed on `App Server 2` under `/opt/docker` directory.

b. Fix the issues with this file and make sure it is able to build the image.

c. Do not change base image, any other valid configuration within Dockerfile, or any of the data  been used — for example, index.html.

`Note:` Please note that once you click on `FINISH` button all existing images, the containers will be destroyed and new image will be built from your `Dockerfile`.

# Solution

`ssh steve@stapp02`

`sudo su -`

`cd /opt/docker`

Try to build first to identify the issue: `docker build .`

```bash
[root@stapp02 docker]# docker build .
Sending build context to Docker daemon  8.704kB
Step 1/8 : FROM httpd:2.4.43
2.4.43: Pulling from library/httpd
bf5952930446: Pull complete 
3d3fecf6569b: Pull complete 
b5fc3125d912: Pull complete 
3c61041685c0: Pull complete 
34b7e9053f76: Pull complete 
Digest: sha256:cd88fee4eab37f0d8cd04b06ef97285ca981c27b4d685f0321e65c5d4fd49357
Status: Downloaded newer image for httpd:2.4.43
 ---> f1455599cc2e
Step 2/8 : RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf
 ---> Running in e000f301edaf
Removing intermediate container e000f301edaf
 ---> ecaf142fcae0
Step 3/8 : RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf
 ---> Running in e8014d1b3cb2
Removing intermediate container e8014d1b3cb2
 ---> d38edd1142dd
Step 4/8 : RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf
 ---> Running in 8bd3e225fede
Removing intermediate container 8bd3e225fede
 ---> 9809b559aa7c
Step 5/8 : RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf
 ---> Running in 2d28c6667f01
Removing intermediate container 2d28c6667f01
 ---> 5b6fc7199dd1
Step 6/8 : COPY /server.crt /usr/local/apache2/conf/server.crt
COPY failed: stat /var/lib/docker/tmp/docker-builder162016997/server.crt: no such file or directory
```

`vi Dockerfile`

```bash
FROM httpd:2.4.43

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf

RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf

RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf

COPY /server.crt /usr/local/apache2/conf/server.crt

COPY /server.key /usr/local/apache2/conf/server.key

COPY ./index.html /usr/local/apache2/htdocs/
```

Make changes:

```bash
FROM httpd:2.4.43

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf

RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf

RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf

COPY certs/server.crt /usr/local/apache2/conf/

COPY certs/server.key /usr/local/apache2/conf/

COPY html/index.html /usr/local/apache2/htdocs
```

```bash
[root@stapp02 docker]# docker build .
Sending build context to Docker daemon  8.704kB
Step 1/8 : FROM httpd:2.4.43
 ---> f1455599cc2e
Step 2/8 : RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf
 ---> Using cache
 ---> ecaf142fcae0
Step 3/8 : RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf
 ---> Using cache
 ---> d38edd1142dd
Step 4/8 : RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf
 ---> Using cache
 ---> 9809b559aa7c
Step 5/8 : RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf
 ---> Using cache
 ---> 5b6fc7199dd1
Step 6/8 : COPY certs/server.crt /usr/local/apache2/conf/
 ---> df47a6776ed4
Step 7/8 : COPY certs/server.key /usr/local/apache2/conf/
 ---> fd7b2ed20fc5
Step 8/8 : COPY html/index.html /usr/local/apache2/htdocs
 ---> 6e33296aba9e
Successfully built 6e33296aba9e
```
