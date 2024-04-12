# Instructions

The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.

1.) Create a deployment named `grafana-deployment-xfusion` using any grafana image for Grafana app. Set other parameters as per your choice.

2.) Create `NodePort` type service with nodePort `32000` to expose the app.

`You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.`

`Note:` The `kubectl` on `jump_host` has been configured to work with kubernetes cluster.

# Solution

To accomplish the given tasks, you can follow the steps below:

1. Create the Grafana deployment:

Create a file named `grafana-deployment.yaml` and add the following contents:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
        - name: grafana-container
          image: grafana/grafana:latest
          ports:
            - containerPort: 3000
```

Apply the configuration to create the deployment: `kubectl apply -f grafana-deployment.yaml`

2. Create a NodePort type service:

Create a file named `grafana-service.yaml` and add the following contents:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
      nodePort: 32000
```

Apply the configuration to create the service: `kubectl apply -f grafana-service.yaml`

Check the result: `k get all -o wide`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/de8a284d-c09d-435f-9f9c-081bcbd89822)
