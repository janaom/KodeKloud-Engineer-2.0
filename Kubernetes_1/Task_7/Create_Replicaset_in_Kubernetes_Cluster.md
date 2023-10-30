# Instructions

The Nautilus DevOps team is going to deploy some applications on kubernetes cluster as they are planning to migrate some of their existing applications there. Recently one of the team members has been assigned a task to write a template as per details mentioned below:

1. Create a ReplicaSet using `httpd` image with `latest` tag only and remember to mention tag i.e `httpd:latest` and name it as `httpd-replicaset`.
2. Labels `app` should be `httpd_app`, labels `type` should be `front-end`.
3. The container should be named as `httpd-container`; also make sure replicas counts are `4`.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

Create a yaml file `replicaset.yaml`:

```YAML
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset
  labels:
    app: httpd_app
    type: front-end
spec:
  replicas: 4
  selector:
    matchLabels:
      app: httpd_app
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
        - name: httpd-container
          image: httpd:latest
```
Run: `kubectl apply -f replicaset.yaml`
