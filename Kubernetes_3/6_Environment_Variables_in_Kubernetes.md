# Instructions

There are a number of parameters that are used by the applications. We need to define these as environment variables, so that we can use them as needed within different configs. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about the same.

1. Create a `pod` named `envars`.
2. Container name should be `fieldref-container`, use image `redis` preferable `latest` tag, use command `'sh', '-c'` and args should be

`'while true; do
      echo -en '/n';
                                  printenv NODE_NAME POD_NAME;
                                  printenv POD_IP POD_SERVICE_ACCOUNT;
              sleep 10;
         done;'`

`(Note: please take care of indentations)`

1. Define `Four` environment variables as mentioned below:

a.) The first `env` should be named as `NODE_NAME`, set valueFrom fieldref and fieldPath should be `spec.nodeName`.

b.) The second `env` should be named as `POD_NAME`, set valueFrom fieldref and fieldPath should be `metadata.name`.

c.) The third `env` should be named as `POD_IP`, set valueFrom fieldref and fieldPath should be `status.podIP`.

d.) The fourth `env` should be named as `POD_SERVICE_ACCOUNT`, set valueFrom fieldref and fieldPath shoulbe be `spec.serviceAccountName`.

1. Set restart policy to `Never`.
2. To check the output, exec into the pod and use `printenv` command.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`vi file.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: envars
spec:
  restartPolicy: Never
  containers:
    - name: fieldref-container
      image: redis:latest
      command: ["sh", "-c"]
      args:
        - while true; do
            echo -en '\n';
            printenv NODE_NAME POD_NAME POD_NAME;
            printenv POD_IP POD_SERVICE_ACCOUNT;
            sleep 10;
          done;
      env:
        - name: NODE_NAME
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_IP
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
        - name: POD_SERVICE_ACCOUNT
          valueFrom:
            fieldRef:
              fieldPath: spec.serviceAccountName
```

`kubectl create -f file.yaml`

`kubectl get pods`

`kubectl exec -it envars -- /bin/bash`

`printenv`

```bash
thor@jump_host ~$ kubectl create -f file.yaml
pod/envars created
thor@jump_host ~$ kubectl get pods
NAME     READY   STATUS    RESTARTS   AGE
envars   1/1     Running   0          12s
thor@jump_host ~$ kubectl exec -it envars -- /bin/bash
root@envars:/data# printenv
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_SERVICE_PORT=443
HOSTNAME=envars
REDIS_DOWNLOAD_SHA=5981179706f8391f03be91d951acafaeda91af7fac56beffb2701963103e423d
POD_NAME=envars
PWD=/data
HOME=/root
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
REDIS_VERSION=7.2.5
GOSU_VERSION=1.17
TERM=xterm
NODE_NAME=kodekloud-control-plane
REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-7.2.5.tar.gz
SHLVL=1
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
POD_IP=10.244.0.5
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PORT=443
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
POD_SERVICE_ACCOUNT=default
_=/usr/bin/printenv
root@envars:/data#
```
