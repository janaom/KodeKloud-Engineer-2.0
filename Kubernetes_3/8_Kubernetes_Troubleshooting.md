# Instructions

One of the Nautilus DevOps team members was working on to update an existing Kubernetes template. Somehow, he made some mistakes in the template and it is failing while applying. We need to fix this as soon as possible, so take a look into it and make sure you are able to apply it without any issues. Also, do not remove any component from the template like pods/deployments/volumes etc.

`/home/thor/mysql_deployment.yml` is the template that needs to be fixed.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`vi /home/thor/mysql_deployment.yml`

```yaml
apiVersion: apps/v1 
kind: Persistentvolume 
metadata:
  name: mysql-pv
  labels:
    type: local
spec:
  storageClassName: standard       
  capacity:
    storage: 250Mi
  accessModes: ReadWriteOnce 
  hostPath:                       
    path: "/mnt/data"
  persistentVolumeReclaimPolicy: 
    - Retain   
---    
apiVersion: apps/v1 
kind: Persistentvolumeclaim       
metadata:                          
  name: mysql-pv-claim
  labels:
    app: mysql-app 
spec:                              
  storageClassName: standard       
  accessModes: ReadWriteOnce             
  resources:
    requests:
      storage: 250MB 
---
apiVersion: v1                    
kind: Service                      
metadata:
  name: mysql         
  labels:             
  app: mysql-app  
spec:
  type: NodePort
  ports:
    - targetPort: 3306
      port: 3306
      nodePort: 30011
  selector:                       
    app: mysql_app
    tier: mysql
---
apiVersion: app/v1 
kind: Deployment                    
metadata:
  name: mysql-deployment           
  labels:                         
  app: mysql-app   
spec:
  selector:
    matchlabels:                  
    app: mysql-app 
    tier: mysql 
  strategy:
    type: Recreate 
  template:         
    metadata:
      labels:        
        app: mysql-app
        tier: mysql
    spec:            
      containers:
      - image: mysql:5.6 
        name: mysql
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
        ports:
        - containerPort: 3306              
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage  
          mountPath: /var/lib/mysql
      volumes:                        
      - name: mysql-persistent-storage
          persistentVolumeClaim: 
          claimName: mysql-pv-claim
```

There are a few errors in the YAML configuration you provided. Let's go through them:

1. In the `PersistentVolume` definition, the `persistentVolumeReclaimPolicy` field should not be a list. It should be a single value. Remove the hyphen before `Retain`.
2. In the `PersistentVolumeClaim` definition, the `requests` section should specify the storage size in lowercase "Mi" units instead of uppercase "MB". Change `250MB` to `250Mi`.
3. In the `Service` definition, the `selector` field refers to labels that are not present in the `Deployment` definition. Update the `Deployment` labels to match the `selector` field in the `Service`. Change `app: mysql_app` to `app: mysql-app`.
4. In the `Deployment` definition, the `matchlabels` field should be `matchLabels` (with an uppercase "L").
5. The indentation of the `persistentVolumeClaim` field under `volumes` is incorrect. Indent it one level further, aligning it with the `name` field.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
  labels:
    type: local
spec:
  storageClassName: standard
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
  persistentVolumeReclaimPolicy: Retain
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
  labels:
    app: mysql-app
spec:
  storageClassName: standard
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
---
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: mysql-app
spec:
  type: NodePort
  ports:
    - targetPort: 3306
      port: 3306
      nodePort: 30011
  selector:
    app: mysql-app
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql-app
spec:
  selector:
    matchLabels:
      app: mysql-app
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql-app
    spec:
      containers:
        - image: mysql:5.6
          name: mysql
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
          ports:
            - containerPort: 3306
              name: mysql
          volumeMounts:
            - name: mysql-persistent-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-persistent-storage
          persistentVolumeClaim:
            claimName: mysql-pv-claim
```

`kubectl create -f  /home/thor/mysql_deployment.yml`

```bash
thor@jump_host ~$ k get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-65fd74cf7b-s5xc8   1/1     Running   0          53s

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          19m
service/mysql        NodePort    10.96.238.125   <none>        3306:30011/TCP   54s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           53s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-65fd74cf7b   1         1         1       53s
```

`kubectl logs mysql-deployment-65fd74cf7b-s5xc8`
