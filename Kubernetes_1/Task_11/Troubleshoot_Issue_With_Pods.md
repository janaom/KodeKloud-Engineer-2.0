# Instructions

One of the junior DevOps team members was working on to deploy a stack on Kubernetes cluster. Somehow the pod is not coming up and its failing with some errors. We need to fix this as soon as possible. Please look into it.

1. There is a pod named `webserver` and the container under it is named as `nginx-container`. It is using image `nginx:latest`
2. There is a sidecar container as well named `sidecar-container` which is using `ubuntu:latest` image.

Look into the issue and fix it, make sure pod is in `running` state and you are able to access the app.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.
