# Instructions

A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:

1.) Create a PersistentVolume `mysql-pv`, its capacity should be `250Mi`, set other parameters as per your preference.

2.) Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as `mysql-pv-claim` and request a `250Mi` of storage. Set other parameters as per your preference.

3.) Create a deployment named `mysql-deployment`, use any mysql image as per your preference. Mount the PersistentVolume at mount path `/var/lib/mysql`.

4.) Create a `NodePort` type service named `mysql` and set nodePort to `30007`.

5.) Create a secret named `mysql-root-pass` having a key pair value, where key is `password` and its value is `YUIidhb667`, create another secret named `mysql-user-pass` having some key pair values, where frist key is `username` and its value is `kodekloud_tim`, second key is `password` and value is `YchZHRcLkL`, create one more secret named `mysql-db-url`, key name is `database` and value is `kodekloud_db6`

6.) Define some Environment variables within the container:

a) `name: MYSQL_ROOT_PASSWORD`, should pick value from secretKeyRef `name: mysql-root-pass` and `key: password`

b) `name: MYSQL_DATABASE`, should pick value from secretKeyRef `name: mysql-db-url` and `key: database`

c) `name: MYSQL_USER`, should pick value from secretKeyRef `name: mysql-user-pass` key `key: username`

d) `name: MYSQL_PASSWORD`, should pick value from secretKeyRef `name: mysql-user-pass` and `key: password`

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`kubectl create secret generic mysql-root-pass --from-literal=password=YUIidhb667`

`kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_tim --from-literal=password=YchZHRcLkL`

`kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db6`

`kubectl get secret`

```bash
thor@jumphost ~$ kubectl get secret
NAME              TYPE     DATA   AGE
mysql-db-url      Opaque   1      9s
mysql-root-pass   Opaque   1      117s
mysql-user-pass   Opaque   2      18s
```

`vi volumes.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  hostPath:
    path: "/var/lib/mysql"
  accessModes:
    - ReadWriteOnce
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  resources:
    requests:
      storage: 250Mi
  accessModes:
    - ReadWriteOnce
```

`kubectl create -f volumes.yaml`

```bash
thor@jumphost ~$ k get pv
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
mysql-pv   250Mi      RWO            Retain           Available                                   7s
thor@jumphost ~$ k get pvc
NAME             STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pv-claim   Pending
```

`vi mysql.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        ports:
        - name: mysql
          containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password          
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database          
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username          
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password          
        volumeMounts:
        - name: mysql-pv
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-pv
        persistentVolumeClaim:
          claimName: mysql-pv-claim
---
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
    - nodePort: 30007
      targetPort: 3306
      port: 3306
```

`kubectl create -f mysql.yaml`

```bash
thor@jumphost ~$ k get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-685cc9f5fd-8gfhx   1/1     Running   0          36s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          32m
service/mysql        NodePort    10.96.87.182   <none>        3306:30007/TCP   36s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           37s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-685cc9f5fd   1         1         1       36s
```

```bash
thor@jumphost ~$ k get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                    STORAGECLASS   REASON   AGE
mysql-pv                                   250Mi      RWO            Retain           Available                                                    4m31s
pvc-2afd7fe3-aa33-44c1-ae91-671adc28d446   250Mi      RWO            Delete           Bound       default/mysql-pv-claim   standard                60s
thor@jumphost ~$ k get pvc
NAME             STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pv-claim   Bound    pvc-2afd7fe3-aa33-44c1-ae91-671adc28d446   250Mi      RWO            standard       4m50s
```

`kubectl exec deployment/mysql-deployment -- printenv`

```bash
thor@jumphost ~$ kubectl exec deployment/mysql-deployment -- printenv
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=mysql-deployment-685cc9f5fd-8gfhx
GOSU_VERSION=1.12
MYSQL_MAJOR=5.6
MYSQL_VERSION=5.6.51-1debian9
MYSQL_ROOT_PASSWORD=YUIidhb667
MYSQL_DATABASE=kodekloud_db6
MYSQL_USER=kodekloud_tim
MYSQL_PASSWORD=YchZHRcLkL
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_PORT=tcp://10.96.0.1:443
MYSQL_PORT_3306_TCP=tcp://10.96.87.182:3306
MYSQL_PORT_3306_TCP_PROTO=tcp
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
MYSQL_SERVICE_PORT=3306
MYSQL_PORT_3306_TCP_ADDR=10.96.87.182
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PORT=443
MYSQL_PORT=tcp://10.96.87.182:3306
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
MYSQL_SERVICE_HOST=10.96.87.182
MYSQL_PORT_3306_TCP_PORT=3306
HOME=/root
```
