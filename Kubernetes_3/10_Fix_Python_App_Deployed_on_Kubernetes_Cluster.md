# Instructions

One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

1. The deployment name is `python-deployment-xfusion`, its using `poroko/flask-demo-app`image. The deployment and service of this app is already deployed.
2. nodePort should be `32345` and targetPort should be python flask app's default port.

`Note:` The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`k get all`

```bash
thor@jump_host ~$ k get all
NAME                                             READY   STATUS             RESTARTS   AGE
pod/python-deployment-xfusion-5dcd4fff84-pbvcj   0/1     ImagePullBackOff   0          47s

NAME                             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes               ClusterIP   10.96.0.1      <none>        443/TCP          17m
service/python-service-xfusion   NodePort    10.96.36.189   <none>        8080:32345/TCP   47s

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/python-deployment-xfusion   0/1     1            0           47s

NAME                                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/python-deployment-xfusion-5dcd4fff84   1         1         0       47s
```

`k describe pod python-deployment-xfusion-5dcd4fff84-pbvcj`

```bash
#check those issues
Containers:
  python-container-xfusion:
    Container ID:   
    Image:          poroko/flask-app-demo
    Image ID:       
    Port:           5000/TCP
    
    <...>
  Warning  Failed     24s (x4 over 105s)  kubelet            Error: ErrImagePull
  Normal   BackOff    11s (x5 over 105s)  kubelet            Back-off pulling image "poroko/flask-app-demo"
  Warning  Failed     11s (x5 over 105s)  kubelet            Error: ImagePullBackOff
```

`k edit deploy python-deployment-xfusion`

```bash
#change
    spec:
      containers:
      - image: poroko/flask-demo-app
```

`k get svc`

```bash
thor@jump_host ~$ k get svc
NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes               ClusterIP   10.96.0.1      <none>        443/TCP          25m
python-service-xfusion   NodePort    10.96.36.189   <none>        8080:32345/TCP   8m53s
```

`k get svc python-service-xfusion -o yaml > python-service-xfusion.yaml`

`vi python-service-xfusion.yaml`

```bash
#change ports
  ports:
  - nodePort: 32345
    port: 5000
    protocol: TCP
    targetPort: 5000
```

`k apply -f python-service-xfusion.yaml`

```bash
thor@jump_host ~$ k get svc
NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes               ClusterIP   10.96.0.1      <none>        443/TCP          29m
python-service-xfusion   NodePort    10.96.36.189   <none>        5000:32345/TCP   12m
```
