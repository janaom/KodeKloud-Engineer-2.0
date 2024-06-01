# Instructions

The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to
 store the application code, and  the template needs to be designed accordingly. Please find more details below:

1. Create a `PersistentVolume` named as `pv-devops`. Configure the `spec` as storage class should be `manual`, set capacity to `3Gi`, set access mode to `ReadWriteOnce`, volume type should be `hostPath` and set path to `/mnt/itadmin` (this directory is already created, you might not be able to access it directly, so you need not to worry about it).
2. Create a `PersistentVolumeClaim` named as `pvc-devops`. Configure the `spec` as storage class should be `manual`, request `2Gi` of the storage, set access mode to `ReadWriteOnce`.
3. Create a `pod` named as `pod-devops`, mount the persistent volume you created with claim name `pvc-devops` at document root of the web server, the container within the pod should be named as `container-devops` using image `nginx` with `latest` tag only (remember to mention the tag i.e `nginx:latest`).
4. Create a node port type service named `web-devops` using node port `30008` to expose the web server running within the pod.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`vi pv.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-devops
spec:
  storageClassName: manual
  capacity:
    storage: 3Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /mnt/itadmin
```

`vi pvc.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-devops
spec:
  storageClassName: manual
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

`vi pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-devops
  labels:
    app: pod-devops
spec:
  containers:
  - name: container-devops
    image: nginx:latest
    volumeMounts:
    - name: persistent-storage
      mountPath: /mnt/itadmin
  volumes:
  - name: persistent-storage
    persistentVolumeClaim:
      claimName: pvc-devops
```

`vi svc.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-devops
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 30008
  selector:
    app: pod-devops
```

`kubectl create -f pv.yaml`

`kubectl create -f pvc.yaml`

`kubectl create -f pod.yaml`

`kubectl create -f svc.yaml`

`kubectl get pv`

```bash
thor@jump_host ~$ k get pv
NAME        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                STORAGECLASS   REASON   AGE
pv-devops   3Gi        RWO            Retain           Bound    default/pvc-devops   manual                  37m
```

`kubectl get pvc`

```bash
thor@jump_host ~$ k get pvc
NAME         STATUS   VOLUME      CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-devops   Bound    pv-devops   3Gi        RWO            manual         36m
```


```bash
thor@jump_host ~$ k get all -o wide
NAME             READY   STATUS    RESTARTS   AGE   IP           NODE                      NOMINATED NODE   READINESS GATES
pod/pod-devops   1/1     Running   0          14s   10.244.0.7   kodekloud-control-plane   <none>           <none>

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE   SELECTOR
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP        57m   <none>
service/web-devops   NodePort    10.96.207.75   <none>        80:30008/TCP   33m   app=pod-devops
```
