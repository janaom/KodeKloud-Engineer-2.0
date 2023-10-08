The Nautilus DevOps team has started practicing some pods and services deployment on Kubernetes platform, as they are planning to migrate most of their applications on Kubernetes. Recently one of the team members has been assigned a task to create a deployment as per details mentioned below:

Create a deployment named `httpd` to deploy the application `httpd` using the image `httpd:latest` (remember to mention the tag as well)

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

## Solution

Create httpd.yaml and run `kubectl apply -f httpd.yaml`
or
run `kubectl create deployment httpd --image=httpd:latest`
