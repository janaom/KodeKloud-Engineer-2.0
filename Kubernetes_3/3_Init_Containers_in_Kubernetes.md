# Instructions

There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.

1. Create a `Deployment` named as `ic-deploy-nautilus`.
2. Configure `spec` as replicas should be `1`, labels `app` should be `ic-nautilus`, template's metadata lables `app` should be the same `ic-nautilus`.
3. The `initContainers` should be named as `ic-msg-nautilus`, use image `debian`, preferably with `latest` tag and use command `'/bin/bash'`, `'-c'` and `'echo Init Done - Welcome to xFusionCorp Industries > /ic/ecommerce'`. The volume mount should be named as `ic-volume-nautilus` and mount path should be `/ic`.
4. Main container should be named as `ic-main-nautilus`, use image `debian`, preferably with `latest` tag and use command `'/bin/bash'`, `'-c'` and `'while true; do cat /ic/ecommerce; sleep 5; done'`. The volume mount should be named as `ic-volume-nautilus` and mount path should be `/ic`.
5. Volume to be named as `ic-volume-nautilus` and it should be an emptyDir type.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`vi file.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-nautilus
  labels:
    app: ic-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-nautilus
  template:
    metadata:
      labels:
        app: ic-nautilus
    spec:
      volumes:
        - name: ic-volume-nautilus
          emptyDir: {}
      initContainers:
        - name: ic-msg-nautilus
          image: debian:latest
          command:
            [
              "/bin/bash",
              "-c",
              "echo Init Done - Welcome to xFusionCorp Industries > /ic/ecommerce",
            ]
          volumeMounts:
            - name: ic-volume-nautilus
              mountPath: /ic

      containers:
        - name: ic-main-nautilus
          image: debian:latest
          command:
            [
              "/bin/bash",
              "-c",
              "while true; do cat /ic/ecommerce; sleep 5; done",
            ]
          volumeMounts:
            - name: ic-volume-nautilus
              mountPath: /ic
```

`kubectl create -f file.yaml`

`kubectl get deploy`

`kubectl get pods`

```bash
thor@jump_host ~$ kubectl get deploy
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
ic-deploy-nautilus   1/1     1            1           11s
thor@jump_host ~$ kubectl get pods
NAME                                  READY   STATUS    RESTARTS   AGE
ic-deploy-nautilus-685556fdb5-x6w44   1/1     Running   0          46s
```

`k logs -f ic-deploy-nautilus-685556fdb5-x6w44`

```bash
thor@jump_host ~$ k logs -f ic-deploy-nautilus-685556fdb5-x6w44
Defaulted container "ic-main-nautilus" out of: ic-main-nautilus, ic-msg-nautilus (init)
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
```

`kubectl exec ic-deploy-nautilus-685556fdb5-x6w44 -- cat /ic/ecommerce`

```bash
thor@jump_host ~$ kubectl exec ic-deploy-nautilus-685556fdb5-x6w44 -- cat /ic/ecommerce
Defaulted container "ic-main-nautilus" out of: ic-main-nautilus, ic-msg-nautilus (init)
Init Done - Welcome to xFusionCorp Industries
```
