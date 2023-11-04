# Instructions

There is an application deployed on Kubernetes cluster. Recently, the Nautilus application development team developed a new version of the application that needs to be deployed now. As per new updates some new changes need to be made in this existing setup. So update the deployment and service as per details mentioned below:

We already have a deployment named `nginx-deployment` and service named `nginx-service`. Some changes need to be made in this deployment and service, make sure not to delete the deployment and service.

1.) Change the service nodeport from `30008` to `32165`

2.) Change the replicas count from `1` to `5`

3.) Change the image from `nginx:1.18` to `nginx:latest`

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

Check the cluster `kubectl  get all -o wide`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/e551c002-fb77-4755-bf4e-60766d38f146)


Check deployment and service: `kubectl describe service` `kubectl describe deploy`

Edit the service: `kubectl edit service nginx-service` and save changes

Edit deployment: `kubectl edit deploy` and save changes

Again, check everything with `k get all -o wide` : Replicas, image, Ports

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/d78601cd-04c5-4af1-bdfb-7d18f8820748)

