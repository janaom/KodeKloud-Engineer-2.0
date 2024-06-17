# Instructions

The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.

`BACK-END TIER`

1. Create a deployment named `redis-master` for Redis master.
    
    a.) Replicas count should be `1`.
    
    b.) Container name should be `master-redis-datacenter` and it should use image `redis`.
    
    c.) Request resources as `CPU` should be `100m` and `Memory` should be `100Mi`.
    
    d.) Container port should be redis default port i.e `6379`.
    
2. Create a service named `redis-master` for Redis master. Port and targetPort should be Redis default port i.e `6379`.
3. Create another deployment named `redis-slave` for Redis slave.
    
    a.) Replicas count should be `2`.
    
    b.) Container name should be `slave-redis-datacenter` and it should use `gcr.io/google_samples/gb-redisslave:v3` image.
    
    c.) Requests resources as `CPU` should be `100m` and `Memory` should be `100Mi`.
    
    d.) Define an environment variable named `GET_HOSTS_FROM` and its value should be `dns`.
    
    e.) Container port should be Redis default port i.e `6379`.
    
4. Create another service named `redis-slave`. It should use Redis default port i.e `6379`.

`FRONT END TIER`

1. Create a deployment named `frontend`.
    
    a.) Replicas count should be `3`.
    
    b.) Container name should be `php-redis-datacenter` and it should use `gcr.io/google-samples/gb-frontend@sha256:cbc8ef4b0a2d0b95965e0e7dc8938c270ea98e34ec9d60ea64b2d5f2df2dfbbf` image.
    
    c.) Request resources as `CPU` should be `100m` and `Memory` should be `100Mi`.
    
    d.) Define an environment variable named as `GET_HOSTS_FROM` and its value should be `dns`.
    
    e.) Container port should be `80`.
    
2. Create a service named `frontend`. Its `type` should be `NodePort`, port should be `80` and its `nodePort` should be `30009`.

Finally, you can check the `guestbook app` by clicking on `App` button.

`You can use any labels as per your choice.`

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`vi app.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
  labels:
    app: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master-redis-datacenter
        image: redis
        ports:
        - containerPort: 6379
        resources:
          requests:
            memory: "100Mi"
            cpu: "100m"
---
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
  labels:
    app: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-slave
  template:
    metadata:
      labels:
        app: redis-slave
    spec:
      containers:
      - name: slave-redis-datacenter
        image: gcr.io/google_samples/gb-redisslave:v3
        ports:
        - containerPort: 6379
        resources:
          requests:
            memory: "100Mi"
            cpu: "100m"
        env:
        - name: GET_HOSTS_FROM
          value: "dns"
---
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  selector:
    app: redis-slave
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: php-redis-datacenter
        image: gcr.io/google-samples/gb-frontend@sha256:cbc8ef4b0a2d0b95965e0e7dc8938c270ea98e34ec9d60ea64b2d5f2df2dfbbf
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "100Mi"
            cpu: "100m"
        env:
        - name: GET_HOSTS_FROM
          value: "dns"
---
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30009
```

`k create -f app.yaml`

```bash
thor@jumphost ~$ k get all
NAME                                READY   STATUS    RESTARTS   AGE
pod/frontend-b6847b798-5n4hh        1/1     Running   0          30s
pod/frontend-b6847b798-dh7jf        1/1     Running   0          29s
pod/frontend-b6847b798-h5h6x        1/1     Running   0          29s
pod/redis-master-5656699db6-n42fk   1/1     Running   0          30s
pod/redis-slave-668f85fd-5bz5n      1/1     Running   0          30s
pod/redis-slave-668f85fd-t8zjz      1/1     Running   0          30s

NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/frontend       NodePort    10.96.66.134    <none>        80:30009/TCP   29s
service/kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP        79m
service/redis-master   ClusterIP   10.96.135.210   <none>        6379/TCP       31s
service/redis-slave    ClusterIP   10.96.96.245    <none>        6379/TCP       30s

NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/frontend       3/3     3            3           30s
deployment.apps/redis-master   1/1     1            1           31s
deployment.apps/redis-slave    2/2     2            2           30s

NAME                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/frontend-b6847b798        3         3         3       30s
replicaset.apps/redis-master-5656699db6   1         1         1       31s
replicaset.apps/redis-slave-668f85fd      2         2         2       30s
```

Open the App and check the result, submit any text

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/5456b345-2b10-469a-9f97-e6cd10ec84b4)


