# Instructions

The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored 
securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

1. We already have a secret key file `ecommerce.txt` under `/opt` location on `jump host`. Create a `generic secret` named `ecommerce`, it should contain the password/license-number present in `ecommerce.txt` file.
2. Also create a `pod` named `secret-devops`.
3. Configure pod's `spec` as container name should be `secret-container-devops`, image should be `debian` preferably with `latest` tag (remember to mention the tag with image). Use `sleep` command for container so that it remains in running state. Consume the created secret and mount it under `/opt/cluster` within the container.
4. To verify you can exec into the container `secret-container-devops`, to check the secret key under the mounted path `/opt/cluster`. Before hitting the `Check` button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


# Solution

```bash
thor@jump_host ~$ cd /opt
thor@jump_host /opt$ ls
ecommerce.txt
thor@jump_host /opt$ cat ecommerce.txt 
5ecur3
thor@jump_host /opt$
```

`vi secret.yaml`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: ecommerce
stringData:
  password: 5ecur3
```

`k create -f secret.yaml`

`vi pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-devops
spec:
   containers:
     - name: secret-container-devops
       image: debian:latest
       command: ['/bin/bash', '-c', 'sleep 10000']
       volumeMounts:
         - mountPath: "/opt/cluster"
           name: secret-volume-devops
           readOnly: true
   volumes:
     - name: secret-volume-devops
       projected:
        sources:
          - secret:
              name: ecommerce
```

`k create -f pod.yaml`

```bash
thor@jump_host /opt$ k get all
NAME                READY   STATUS    RESTARTS   AGE
pod/secret-devops   1/1     Running   0          17s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   101m
thor@jump_host /opt$ k get secret
NAME        TYPE     DATA   AGE
ecommerce   Opaque   1      44s
```

`kubectl exec -it secret-devops -- bash`

```bash
thor@jump_host /opt$ kubectl exec -it secret-devops -- bash
root@secret-devops:/# ls -la /opt/cluster
total 4
drwxrwxrwt 3 root root  100 Jun  1 13:50 .
drwxr-xr-x 1 root root 4096 Jun  1 13:50 ..
drwxr-xr-x 2 root root   60 Jun  1 13:50 ..2024_06_01_13_50_03.64425314
lrwxrwxrwx 1 root root   30 Jun  1 13:50 ..data -> ..2024_06_01_13_50_03.64425314
lrwxrwxrwx 1 root root   15 Jun  1 13:50 password -> ..data/password
```

`cat /opt/cluster/password`

```bash
root@secret-devops:/# cat /opt/cluster/password
5ecur3
```
