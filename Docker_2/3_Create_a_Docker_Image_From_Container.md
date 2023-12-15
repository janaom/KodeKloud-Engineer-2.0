# Instructions

One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:

a. Create an image `blog:devops` on `Application Server 2` from a container `ubuntu_latest` that is running on same server.

# Solution

ssh into the App Server 2: `ssh steve@172.16.238.11`

Check the running containers on the server to ensure that the "ubuntu_latest" container is active: `docker ps`

Create an image from the "ubuntu_latest" container: `docker commit ubuntu_latest blog:devops`

Verify the image creation: `docker images`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b1151313-9c10-4e70-be28-493c225804b4)
