# Instructions

We deployed a Nginx and PHP-FPM based setup on Kubernetes cluster last week and it had been working fine so far. This morning one of the team members made a change somewhere which caused some issues, and it stopped working. Please look into the issue and fix it:

The pod name is `nginx-phpfpm` and configmap name is `nginx-config`. Figure out the issue and fix the same.

Once issue is fixed, copy `/home/thor/index.php` file from the `jump host` to the `nginx-container` under nginx document root and you should be able to access the website using `Website` button on top bar.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`kubectl get all -o wide`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/8321f3b5-75ce-40bd-95fc-409c5c7e2abf)


`kubectl describe configmap nginx-config`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/776fb86f-3e83-481e-8188-7e0b9bce79eb)


This command retrieves the YAML definition of the nginx-phpfpm pod and saves it to the /tmp/nginx.yaml file. By examining the pod's configuration, we can identify the specific location where the file needs to be copied:

`kubectl get pod nginx-phpfpm -o yaml  > /tmp/nginx.yaml`

This command displays the contents of the /tmp/nginx.yaml file. It helps in inspecting the pod's YAML definition to locate the mountPath for the nginx container's document root:
`cat /tmp/nginx.yaml`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/5a120f78-9b6e-4539-9a0b-3ce3626855b2)


```YAML
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"php-app"},"name":"nginx-phpfpm","namespace":"default"},"spec":{"containers":[{"image":"php:7.2-fpm-alpine","name":"php-fpm-container","volumeMounts":[{"mountPath":"/var/www/html","name":"shared-files"}]},{"image":"nginx:latest","name":"nginx-container","volumeMounts":[{"mountPath":"/usr/share/nginx/html","name":"shared-files"},{"mountPath":"/etc/nginx/nginx.conf","name":"nginx-config-volume","subPath":"nginx.conf"}]}],"volumes":[{"emptyDir":{},"name":"shared-files"},{"configMap":{"name":"nginx-config"},"name":"nginx-config-volume"}]}}
  creationTimestamp: "2023-11-05T10:38:04Z"
  labels:
    app: php-app
  name: nginx-phpfpm
  namespace: default
  resourceVersion: "2577"
  uid: 1747d70f-26a0-4111-875f-10e6533727b8
spec:
  containers:
  - image: php:7.2-fpm-alpine
    imagePullPolicy: IfNotPresent
    name: php-fpm-container
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/www/html
      name: shared-files
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-nsjtj
      readOnly: true
  - image: nginx:latest
    imagePullPolicy: Always
    name: nginx-container
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: shared-files
    - mountPath: /etc/nginx/nginx.conf
      name: nginx-config-volume
      subPath: nginx.conf
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-nsjtj
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: kodekloud-control-plane
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - emptyDir: {}
    name: shared-files
  - configMap:
      defaultMode: 420
      name: nginx-config
    name: nginx-config-volume
  - name: kube-api-access-nsjtj
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2023-11-05T10:38:04Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2023-11-05T10:38:19Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2023-11-05T10:38:19Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2023-11-05T10:38:04Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: containerd://0661e8feec75074fc73fdd247080a552a848af2518840b99c24483f689ad3eec
    image: docker.io/library/nginx:latest
    imageID: docker.io/library/nginx@sha256:86e53c4c16a6a276b204b0fd3a8143d86547c967dc8258b3d47c3a21bb68d3c6
    lastState: {}
    name: nginx-container
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2023-11-05T10:38:19Z"
  - containerID: containerd://fb09bdc33c2c0b2fcce965a5410454bd4ca7af1281bb7f9410999c90ee0d6250
    image: docker.io/library/php:7.2-fpm-alpine
    imageID: docker.io/library/php@sha256:2e2d92415f3fc552e9a62548d1235f852c864fcdc94bcf2905805d92baefc87f
    lastState: {}
    name: php-fpm-container
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2023-11-05T10:38:10Z"
  hostIP: 172.17.0.2
  phase: Running
  podIP: 10.244.0.5
  podIPs:
  - ip: 10.244.0.5
  qosClass: BestEffort
  startTime: "2023-11-05T10:38:04Z"
```

See {"mountPath":"/usr/share/nginx/html”}

This command filters the output of the previous command to search for the line that contains /usr/share/nginx/html. It helps to confirm the current mount path for the nginx container's document root:

`cat /tmp/nginx.yaml  |grep /usr/share/nginx/html`

`vi /tmp/nginx.yaml`

change this part to `/var/www/html`
The nginx container typically expects website files to be present in the /var/www/html directory, which serves as the default document root for nginx. Placing the index.php file in this directory ensures that nginx can serve it as the default page for the website.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/182d00e4-1293-4795-8f45-c5a675f9d2f1)


`kubectl replace -f /tmp/nginx.yaml --force`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/64949b9d-37f6-4906-95e5-93bb236df0d6)


`kubectl cp  /home/thor/index.php  nginx-phpfpm:/var/www/html -c nginx-container`


Validate the result:
`kubectl exec -it nginx-phpfpm -c nginx-container  -- curl -I  http://localhost:8099`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/0fae3aea-bdf1-4542-9212-ab1b1288db9d)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/3409b652-a29d-4901-9bfe-a9cc5af2d3b0)


