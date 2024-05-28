# Instrutions

There is an application that needs to be deployed on Kubernetes cluster under Apache web server. The Nautilus application development team has asked the DevOps team to 
deploy it. We need to develop a template as per requirements mentioned below:

1. Create a `namespace` named as `httpd-namespace-devops`.
2. Create a `deployment` named as `httpd-deployment-devops` under newly created namespace. For the deployment use `httpd` image with `latest` tag only and remember to mention the tag i.e `httpd:latest`, and make sure replica counts are `2`.
3. Create a `service` named as `httpd-service-devops` under same namespace to expose the deployment, nodePort should be `30004`.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`kubectl create namespace httpd-namespace-devops`

`kubectl get namespace`

```bash
thor@jump_host ~$ kubectl get namespace
NAME                       STATUS   AGE
default                    Active   65m
httpd-namespace-devops     Active   15s
kube-node-lease            Active   65m
kube-public                Active   65m
kube-system                Active   65m
local-path-storage         Active   65m
```

`vi file.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: httpd-service-devops
  namespace: httpd-namespace-devops
  labels:
     app: httpd_app
spec:
  type: NodePort
  selector:
     app: httpd_app
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30004
    
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deployment-devops
  namespace: httpd-namespace-devops
  labels:
    app: httpd_app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: httpd_app
  template:
    metadata:
      labels:
        app: httpd_app
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest
```

`kubectl apply -f file.yaml`

`kubectl get all -n httpd-namespace-devops -o wide`

```bash
thor@jump_host ~$ kubectl get all -n httpd-namespace-devops -o wide
NAME                                          READY   STATUS    RESTARTS   AGE   IP           NODE                      NOMINATED NODE   READINESS GATES
pod/httpd-deployment-devops-6b69c6bcd-9pwgl   1/1     Running   0          30s   10.244.0.7   kodekloud-control-plane   <none>           <none>
pod/httpd-deployment-devops-6b69c6bcd-v4np4   1/1     Running   0          28s   10.244.0.8   kodekloud-control-plane   <none>           <none>

NAME                           TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE   SELECTOR
service/httpd-service-devops   NodePort   10.96.184.124   <none>        80:30004/TCP   14m   app=httpd_app

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS        IMAGES         SELECTOR
deployment.apps/httpd-deployment-devops   2/2     2            2           14m   httpd-container   httpd:latest   app=httpd_app

NAME                                                 DESIRED   CURRENT   READY   AGE   CONTAINERS               IMAGES         SELECTOR
replicaset.apps/httpd-deployment-devops-6b69c6bcd    2         2         2       30s   httpd-container          httpd:latest   app=httpd_app,pod-template-hash=6b69c6bcd
replicaset.apps/httpd-deployment-devops-7cbfc84f4f   0         0         0       14m   httpd-container-devops   httpd:latest   app=httpd_app,pod-template-hash=7cbfc84f4f
```
**kubectl get namespace**
