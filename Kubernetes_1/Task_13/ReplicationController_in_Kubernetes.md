# Instructions

The Nautilus DevOps team wants to create a `ReplicationController`
 to deploy several pods. They are going to deploy applications on these 
pods, these applications need highly available infrastructure. Below you
 can find exact details, create the `ReplicationController` accordingly.

1. Create a `ReplicationController` using `httpd` image, preferably with `latest` tag, and name it as `httpd-replicationcontroller`.
2. Labels `app` should be `httpd_app`, and labels `type` should be `front-end`. The container should be named as `httpd-container` and also make sure replica counts are `3`.

All `pods` should be running state after deployment.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

Create `rc.yaml`

```YAML
apiVersion: v1
kind: ReplicationController
metadata:
  name: httpd-replicationcontroller
spec:
  replicas: 3
  selector:
    app: httpd_app
    type: front-end
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest
        ports:
        - containerPort: 80
```

Run with `kubectl create -f rc.yaml`
