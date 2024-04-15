# Instructions

The Nautilus development team has completed development of one of the node applications, which they are planning to deploy on a Kubernetes cluster. They recently had a meeting with the DevOps team to share their requirements. Based on that, the DevOps team has listed out the exact requirements to deploy the app. Find below more details:

1. Create a deployment using `gcr.io/kodekloud/centos-ssh-enabled:node` image, replica count must be `2`.
2. Create a service to expose this app, the service type must be `NodePort`, targetPort must be `8080` and nodePort should be `30012`.
3. Make sure all the pods are in `Running` state after the deployment.
4. You can check the application by clicking on `NodeApp` button on top bar.

`You can use any labels as per your choice.`

`Note:` The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

To achieve the given tasks, follow the steps below:

1. Create the deployment using the `gcr.io/kodekloud/centos-ssh-enabled:node` image with a replica count of 2:

Create a file named `node-deployment.yaml` and add the following contents:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
        - name: node-container
          image: gcr.io/kodekloud/centos-ssh-enabled:node
          ports:
            - containerPort: 8080
```

Apply the configuration to create the deployment: `kubectl apply -f node-deployment.yaml`

2. Create a NodePort type service to expose the app:

Create a file named `node-service.yaml` and add the following contents:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-service
spec:
  type: NodePort
  selector:
    app: node-app
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
      nodePort: 30012
```

Apply the configuration to create the service: `kubectl apply -f node-service.yaml`

3. Verify that all pods are in the Running state:

Run the following command to check the status of the pods: `kubectl get pods`

Check the result: `k get all -o wide`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/e6a38945-dc8f-4b64-b39d-bf23b7aab430)
