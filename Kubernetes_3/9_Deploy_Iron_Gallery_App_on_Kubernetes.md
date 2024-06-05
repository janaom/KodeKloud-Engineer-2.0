# Instructions

There is an iron gallery app that the Nautilus DevOps team was developing. They have recently customized the app and are going to deploy the same on the Kubernetes cluster. Below you can find more details:

1. Create a namespace `iron-namespace-datacenter`
2. Create a deployment `iron-gallery-deployment-datacenter` for `iron gallery` under the same namespace you created.
    
    :- Labels `run` should be `iron-gallery`.
    
    :- Replicas count should be `1`.
    
    :- Selector's matchLabels `run` should be `iron-gallery`.
    
    :- Template labels `run` should be `iron-gallery` under metadata.
    
    :- The container should be named as `iron-gallery-container-datacenter`, use `kodekloud/irongallery:2.0` image ( use exact image name / tag ).
    
    :- Resources limits for memory should be `100Mi` and for CPU should be `50m`.
    
    :- First volumeMount name should be `config`, its mountPath should be `/usr/share/nginx/html/data`.
    
    :- Second volumeMount name should be `images`, its mountPath should be `/usr/share/nginx/html/uploads`.
    
    :- First volume name should be `config` and give it `emptyDir` and second volume name should be `images`, also give it `emptyDir`.
    
3. Create a deployment `iron-db-deployment-datacenter` for `iron db` under the same namespace.
    
    :- Labels `db` should be `mariadb`.
    
    :- Replicas count should be `1`.
    
    :- Selector's matchLabels `db` should be `mariadb`.
    
    :- Template labels `db` should be `mariadb` under metadata.
    
    :- The container name should be `iron-db-container-datacenter`, use `kodekloud/irondb:2.0` image ( use exact image name / tag ).
    
    :- Define environment, set `MYSQL_DATABASE` its value should be `database_apache`, set `MYSQL_ROOT_PASSWORD` and `MYSQL_PASSWORD` value should be with some complex passwords for DB connections, and `MYSQL_USER` value should be any custom user ( except root ).
    
    :- Volume mount name should be `db` and its mountPath should be `/var/lib/mysql`. Volume name should be `db` and give it an `emptyDir`.
    
4. Create a service for `iron db` which should be named `iron-db-service-datacenter` under the same namespace. Configure spec as selector's db should be `mariadb`. Protocol should be `TCP`, port and targetPort should be `3306` and its type should be `ClusterIP`.
5. Create a service for `iron gallery` which should be named `iron-gallery-service-datacenter` under the same namespace. Configure spec as selector's run should be `iron-gallery`. Protocol should be `TCP`, port and targetPort should be `80`, nodePort should be `32678` and its type should be `NodePort`.

`Note:`

1. We don't need to make connection b/w database and front-end now, if the installation page is coming up it should be enough for now.
2. The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`k get all`

`k get ns`

```bash
thor@jump_host ~$ k get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   18m
thor@jump_host ~$ k get ns
NAME                 STATUS   AGE
default              Active   18m
kube-node-lease      Active   18m
kube-public          Active   18m
kube-system          Active   18m
local-path-storage   Active   18m
```

`vi file.yaml`

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-datacenter
  labels:
    name: iron-namespace-datacenter
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-datacenter
  namespace: iron-namespace-datacenter
  labels:
    app: iron-gallery
spec:
  replicas: 1
  selector:
    matchLabels:
      app: iron-gallery
  template:
    metadata:
      labels:
        app: iron-gallery
    spec:
      volumes:
        - name: config
          emptyDir: {}
        - name: images
          emptyDir: {}
      containers:
        - name: iron-gallery-container-datacenter
          image: kodekloud/irongallery:2.0
          volumeMounts:
            - name: config
              mountPath: /usr/share/nginx/html/data
            - name: images
              mountPath: /usr/share/nginx/html/uploads
          resources:
            limits:
              memory: 100Mi
              cpu: 50m
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-datacenter
  namespace: iron-namespace-datacenter
  labels:
    app: mariadb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mariadb
  template:
    metadata:
      labels:
        app: mariadb
    spec:
      volumes:
        - name: db
          emptyDir: {}
      containers:
        - name: iron-db-container-datacenter
          image: kodekloud/irondb:2.0
          env:
            - name: MYSQL_DATABASE
              value: database_apache
            - name: MYSQL_ROOT_PASSWORD
              value: admin123
            - name: MYSQL_PASSWORD
              value: admin123
            - name: MYSQL_USER
              value: kodekloud
          volumeMounts:
            - name: db
              mountPath: /var/lib/mysql
---
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-datacenter
  namespace: iron-namespace-datacenter
spec:
  type: ClusterIP
  selector:
    app: mariadb
  ports:
    - port: 3306
      targetPort: 3306
      protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-datacenter
  namespace: iron-namespace-datacenter
spec:
  type: NodePort
  selector:
    app: iron-gallery
  ports:
    - port: 80
      targetPort: 80
      nodePort: 32678
      protocol: TCP
```

`kubectl apply -f file.yaml`

```bash
thor@jump_host ~$ kubectl apply -f file.yaml
namespace/iron-namespace-datacenter created
deployment.apps/iron-gallery-deployment-datacenter created
deployment.apps/iron-db-deployment-datacenter created
service/iron-db-service-datacenter created
service/iron-gallery-service-datacenter created
```

`kubectl get all --namespace iron-namespace-datacenter`

```bash
thor@jump_host ~$ kubectl get all --namespace iron-namespace-datacenter
NAME                                                      READY   STATUS    RESTARTS   AGE
pod/iron-db-deployment-datacenter-577c68d8b4-gvq48        1/1     Running   0          29s
pod/iron-gallery-deployment-datacenter-676945776f-kr2vp   1/1     Running   0          29s

NAME                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/iron-db-service-datacenter        ClusterIP   10.96.144.121   <none>        3306/TCP       29s
service/iron-gallery-service-datacenter   NodePort    10.96.83.130    <none>        80:32678/TCP   28s

NAME                                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/iron-db-deployment-datacenter        1/1     1            1           29s
deployment.apps/iron-gallery-deployment-datacenter   1/1     1            1           29s

NAME                                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/iron-db-deployment-datacenter-577c68d8b4        1         1         1       29s
replicaset.apps/iron-gallery-deployment-datacenter-676945776f   1         1         1       29s
```

In case you need to delete and recreate resources:

`kubectl delete all --all --namespace iron-namespace-datacenter`

`kubectl get all --namespace iron-namespace-datacenter`

`kubectl delete namespace iron-namespace-datacenter`
