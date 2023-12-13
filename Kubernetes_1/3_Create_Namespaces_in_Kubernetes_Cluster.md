# Instructions

The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want set up some namespaces, deployments etc. Based on the current requirements, the team has shared some details as below:

Create a namespace named `dev` and create a POD under it; name the pod `dev-nginx-pod` and use `nginx` image with `latest` tag only and remember to mention tag i.e `nginx:latest`.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solutions

- to create a namespace: `kubectl create namespace dev`

- to check namespaces: `kubectl get namespace`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/34f312f8-4775-43c6-b8b3-4b7e4bbf769d)


- to create the pod: `kubectl run dev-nginx-pod --image=nginx:latest --namespace=dev`

- to check the pod: `kubectl get pod -n dev`

- to check the details about the pod: `kubectl describe pod dev-nginx-pod -n dev`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/5b6e25cc-c028-41af-906c-bc0ae9812902)
