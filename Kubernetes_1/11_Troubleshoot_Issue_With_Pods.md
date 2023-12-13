# Instructions

One of the junior DevOps team members was working on to deploy a stack on Kubernetes cluster. Somehow the pod is not coming up and its failing with some errors. We need to fix this as soon as possible. Please look into it.

1. There is a pod named `webserver` and the container under it is named as `nginx-container`. It is using image `nginx:latest`
2. There is a sidecar container as well named `sidecar-container` which is using `ubuntu:latest` image.

Look into the issue and fix it, make sure pod is in `running` state and you are able to access the app.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/3bcc7f62-d40f-4271-ae3c-b376c8737477)

Check the pod and the image

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a6573323-3187-450f-b011-c611fdf54026)

Don't forget to check the events

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a5db21c9-27a2-47a5-a8cd-7490ed1cbaf3)

Edit the pod `kubectl edit pod webserver` and save changes (change the image, it should be `nginx:latest`)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b949e5bc-10df-47df-878b-6e48c2f54fdf)

Check the status

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/ce524609-a2cb-47a2-8d0e-1ee71f9c238d)

Check the image

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f04623da-e6b1-4671-bf13-fd046df024c4)

Check events

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/42c0052d-d14d-4425-ae1a-2108d4225b93)

