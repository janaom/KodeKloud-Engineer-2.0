# Instructions

Recently some of the performance issues were observed with some applications hosted on Kubernetes cluster. The Nautilus DevOps team has observed some resources constraints, where some of the applications are running out of resources like memory, cpu etc., and some of the applications are consuming more resources than needed. Therefore, the team has decided to add some limits for resources utilization. Below you can find more details.

Create a pod named `httpd-pod` and a container under it named as `httpd-container`, use `httpd` image with `latest` tag only and remember to mention tag i.e `httpd:latest` and set the following limits:

Requests: Memory: `15Mi`, CPU: `100m`

Limits: Memory: `20Mi`,  CPU: `100m`

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"

#Create a pod with `kubectl apply -f httpd-pod.yaml` command
```
