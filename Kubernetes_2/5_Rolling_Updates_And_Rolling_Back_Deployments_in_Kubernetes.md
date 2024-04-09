# Instructions

There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.

1. Create a namespace `nautilus`. Create a deployment called `httpd-deploy` under this new namespace, It should have one container called `httpd`, use `httpd:2.4.25` image and `3` replicas. The deployment should use `RollingUpdate` strategy with `maxSurge=1`, and `maxUnavailable=2`. Also create a `NodePort` type service named `httpd-service` and expose the deployment on `nodePort: 30008`.
2. Now upgrade the deployment to version `httpd:2.4.43` using a rolling update.
3. Finally, once all pods are updated undo the recent update and roll back to the previous/original version.

`Note:`

a. The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

b. Please make sure you only use the specified image(s) for this deployment and as per the sequence mentioned in the task description. If you mistakenly use a wrong image and fix it later, that will also distort the revision history which can eventually fail this task.


# Solution

1) Create the namespace named "nautilus": `kubectl create namespace nautilus`

2) Create a deployment called "httpd-deploy" with one container named "httpd", using the "httpd:2.4.25" image, and 3 replicas:

  Create a file named `httpd-deploy.yaml` and add the following contents:
  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deploy
  namespace: nautilus
spec:
  replicas: 3
  selector:
    matchLabels:
      app: httpd
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
        - name: httpd
          image: httpd:2.4.25
          ports:
            - containerPort: 80
```

  Apply the configuration to create the deployment: `kubectl apply -f httpd-deploy.yaml`

3) Create a NodePort type service named "httpd-service" and expose the deployment on nodePort: 30008:

  Create a file named `httpd-service.yaml` and add the following contents:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: httpd-service
  namespace: nautilus
spec:
  type: NodePort
  selector:
    app: httpd
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30008
```

  Apply the configuration to create the service: `kubectl apply -f httpd-service.yaml`

4) Upgrade the deployment to version "httpd:2.4.43" using a rolling update:

`kubectl set image deployment/httpd-deploy httpd=httpd:2.4.43 --namespace=nautilus`

5) Monitor the deployment until all pods are updated: 
`kubectl rollout status deployment/httpd-deploy --namespace=nautilus`

6) Undo the recent update and roll back to the previous/original version:
`kubectl rollout undo deployment/httpd-deploy --namespace=nautilus`

To check the final result and verify the status of the deployment and service, you can use the following commands:

1. Check the status of the deployment: `kubectl get deployment httpd-deploy --namespace=nautilus`

This will display information about the deployment, including the number of desired, current, and available replicas.

2. Check the status of the pods: `kubectl get pods --selector=app=httpd --namespace=nautilus`

This will list the pods related to the deployment, along with their current status.

3. Check the status of the service: `kubectl get service httpd-service --namespace=nautilus`

This will provide information about the service, including the service type, cluster IP, and ports.

4. Verify the rollout history of the deployment: `kubectl rollout history deployment/httpd-deploy --namespace=nautilus`
   

To double check the deployment:  `kubectl describe  deployment httpd-deploy --namespace=nautilus`
