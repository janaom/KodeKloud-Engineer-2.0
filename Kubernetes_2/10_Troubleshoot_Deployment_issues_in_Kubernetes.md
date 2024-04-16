# Instructions

Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.

The deployment name is `redis-deployment`. The pods are not in running state right now, so please look into the issue and fix the same.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


# Solution

Run: `kubectl get pods`

```bash
thor@jump_host ~$ kubectl get pods
NAME                               READY   STATUS              RESTARTS   AGE
redis-deployment-6fd9d5fcb-pxfpw   0/1     ContainerCreating   0          59s
```

Check the pod: `k describe pod redis-deployment-6fd9d5fcb-pxfpw`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a4516d63-69a4-4b99-ba9f-acd5db2de621)

Check if the ConfigMap "redis-config" actually exists in the cluster: `kubectl get configmap redis-config`

Verify the ConfigMap's name by running: `kubectl get configmap`

```bash
thor@jump_host ~$ kubectl get configmap
NAME               DATA   AGE
kube-root-ca.crt   1      86m
redis-config       2      8m1s
```

Edit the Deployment configuration: `kubectl edit deployment redis-deployment`

Edit these parts

```bash
          spec:
      containers:
      - image: redis:alpin   #should be alpine
        imagePullPolicy: IfNotPresent
        name: redis-container
        ports:
        <...>
     
      
      - configMap:
          defaultMode: 420
          name: redis-cofig  #should be config
        name: config
```

Check the result:

```bash
thor@jump_host ~$ kubectl get deployment redis-deployment -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS        IMAGES         SELECTOR
redis-deployment   1/1     1            1           21m   redis-container   redis:alpine   app=redis

thor@jump_host ~$ k get pods
NAME                                READY   STATUS    RESTARTS   AGE
redis-deployment-7c8d4f6ddf-8vkkn   1/1     Running   0          78s
```

