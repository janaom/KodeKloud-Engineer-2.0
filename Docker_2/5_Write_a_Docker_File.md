# Instructions

As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file `/opt/docker/Dockerfile` (please keep `D` capital of Dockerfile) on `App server 2` in `Stratos DC` and configure to build an image with the following requirements:

a. Use `ubuntu` as the base image.

b. Install `apache2` and configure it to work on `5003` port. (do not update any other Apache configuration settings like document root etc).

# Solution

ssh into the App Server 2: `ssh steve@172.16.238.11`

Navigate to the `/opt/docker` directory: `cd /opt/docker`

Create a new file called `Dockerfile` (with a capital "D") using a text editor: `sudo vi Dockerfile`

Add the following content to the Dockerfile:
```Python
# Use ubuntu as the base image
FROM ubuntu

# Install apache2
RUN apt-get update && apt-get install -y apache2

# Configure apache2 to work on port 5003
RUN echo "Listen 5003" >> /etc/apache2/ports.conf

# Start apache2 service
CMD ["apachectl", "-D", "FOREGROUND"]
```

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/6d07e293-b561-4de9-a8c9-ce8dd5c3e792)

To build the Docker image, you can use the following command:`docker build -t my-apache-image /opt/docker`

Once the image is built, you can run a container based on this image using the following command:`docker run -d -p 5003:5003 my-apache-image`

Check the results: `docker ps` and `curl -Ik [http://localhost:5003](http://localhost:5003/)`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/2455a271-2d50-480e-8a7f-3dfb61254f9d)

