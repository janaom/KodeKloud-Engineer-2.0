# Instructions

The Nautilus DevOps team is planning to set up a Jenkins CI server to create/manage some deployment pipelines for some of the projects. They want to set up the Jenkins server on Kubernetes cluster. Below you can find more details about the task:

1) Create a namespace `jenkins`

2) Create a Service for jenkins deployment. Service name should be `jenkins-service` under `jenkins` namespace, type should be `NodePort`, nodePort should be `30008`

3) Create a Jenkins Deployment under `jenkins` namespace, It should be name as `jenkins-deployment` , labels `app` should be `jenkins` , container name should be `jenkins-container` , use `jenkins/jenkins` image , containerPort should be `8080` and replicas count should be `1`.

Make sure to wait for the pods to be in running state and make sure you are able to access the Jenkins login screen in the browser before hitting the `Check` button.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

To accomplish the given tasks, you can follow the steps below:

1. Create the namespace named "jenkins": `kubectl create namespace jenkins`
2. Create a Service for the Jenkins deployment:
Create a file named `jenkins-service.yaml` and add the following contents:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: jenkins-service
  namespace: jenkins
spec:
  type: NodePort
  selector:
    app: jenkins
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
      nodePort: 30008
```

Apply the configuration to create the service: `kubectl apply -f jenkins-service.yaml`

3. Create a Jenkins Deployment:
Create a file named `jenkins-deployment.yaml` and add the following contents:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins-deployment
  namespace: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
        - name: jenkins-container
          image: jenkins/jenkins
          ports:
            - containerPort: 8080
```

Apply the configuration to create the deployment: `kubectl apply -f jenkins-deployment.yaml`

Check the results: `k get all --namespace=jenkins`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/482a1f68-4ec3-40bb-8f92-55acb461acab)
