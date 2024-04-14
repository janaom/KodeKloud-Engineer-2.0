# Instructions

A new java-based application is ready to be deployed on a Kubernetes cluster. The development team had a meeting with the DevOps team to share the requirements and application scope. The team is ready to setup an application stack for it under their existing cluster. Below you can find the details for this:

1. Create a namespace named `tomcat-namespace-devops`.
2. Create a `deployment` for tomcat app which should be named as `tomcat-deployment-devops` under the same namespace you created. Replica count should be `1`, the container should be named as `tomcat-container-devops`, its image should be `gcr.io/kodekloud/centos-ssh-enabled:tomcat` and its container port should be `8080`.
3. Create a `service` for tomcat app which should be named as `tomcat-service-devops` under the same namespace you created. Service type should be `NodePort` and nodePort should be `32227`.

Before clicking on `Check` button please make sure the application is up and running.

`You can use any labels as per your choice.`

`Note:` The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

To accomplish the given tasks, follow the steps below:

1. Create the namespace named "tomcat-namespace-devops":  `kubectl create namespace tomcat-namespace-devops`
2. Create a deployment for the Tomcat app:

Create a file named `tomcat-deployment.yaml` and add the following contents:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-deployment-devops
  namespace: tomcat-namespace-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat
  template:
    metadata:
      labels:
        app: tomcat
    spec:
      containers:
        - name: tomcat-container-devops
          image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
          ports:
            - containerPort: 8080
```

Apply the configuration to create the deployment: `kubectl apply -f tomcat-deployment.yaml`

1. Create a service for the Tomcat app:

Create a file named `tomcat-service.yaml` and add the following contents:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: tomcat-service-devops
  namespace: tomcat-namespace-devops
spec:
  type: NodePort
  selector:
    app: tomcat
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
      nodePort: 32227
```

Apply the configuration to create the service: `kubectl apply -f tomcat-service.yaml`

Check the result: `k get all -o wide --namespace=tomcat-namespace-devops`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/83d1582f-a3e5-4ca6-adc6-277f2f5431b1)
