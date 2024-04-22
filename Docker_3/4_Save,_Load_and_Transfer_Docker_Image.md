# Instructions

One of the DevOps team members was working on to create a new custom docker image on `App Server 1` in `Stratos DC`. He is done with his changes and image is saved on same server with name `beta:xfusion`. Recently a requirement has been raised by a team to use that image for testing, but the team wants to test the same on `App Server 3`. So we need to provide them that image on `App Server 3` in `Stratos DC`.

a.  On `App Server 1` save the image `beta:xfusion` in an archive.

b. Transfer the image archive to `App Server 3`.

c. Load that image archive on `App Server 3` with same name and tag which was used on `App Server 1`.

`Note:` Docker is already installed on both servers; however, if its service is down please make sure to start it.

# Solution

On App Server 1:

`ssh tony@stapp01`
a. Save the image "beta:xfusion" in an archive: `docker save -o beta_xfusion.tar beta:xfusion`

b. Transfer the image archive to App Server 3:  `scp beta_xfusion.tar banner@stapp03:/home/banner/beta_xfusion.tar`

On App Server 3:

`ssh banner@stapp03`

c. Load the image archive with the same name and tag used on App Server 1:`docker load -i beta_xfusion.tar`

```bash
[banner@stapp03 ~]$ docker load -i beta_xfusion.tar
e0a9f5911802: Loading layer [==================================================>]  80.41MB/80.41MB
b2bf41d0d674: Loading layer [==================================================>]  50.85MB/50.85MB
Loaded image: beta:xfusion
[banner@stapp03 ~]$ docker imag
```

Check the image: `docker image ls beta`

```bash
[banner@stapp03 ~]$ docker image ls beta
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
beta                xfusion             fc605d233533        12 minutes ago      129MB
```
