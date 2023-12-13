# Instructions

Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:

a.  Pull `busybox:musl` image on `App Server 1` in Stratos DC and re-tag (create new tag) this image as `busybox:news`.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

Run the following command to pull the busybox:musl image from the Docker registry: `docker pull busybox:musl`

After pulling the busybox:musl image, you can create a new tag for the image using the following command: `docker tag busybox:musl busybox:news`

You can verify that the new tag has been created by running the following command:  `docker images`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/4989f2ee-439f-4cbe-9815-5a0edc161c0f)
