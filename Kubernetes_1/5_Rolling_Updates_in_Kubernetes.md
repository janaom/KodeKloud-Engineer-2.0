# Instructions
We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest changes and those changes need be deployed. The Nautilus DevOps team has created an image `nginx:1.19` with the latest changes.

Perform a rolling update for this application and incorporate `nginx:1.19` image. The deployment name is `nginx-deployment`

Make sure all pods are up and running after the update.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution
`kubectl get all`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/1d58c04b-22b2-4688-aef9-eebfe15b61e6)

`kubectl describe deploy nginx-deployment`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/97854fd8-c3d7-44ea-a700-6b5c947fb2a4)


`kubectl edit deployment nginx-deployment`  change the image

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/cf2fc308-0d2c-465c-9a91-609e91e08844)


`kubectl describe deploy nginx-deployment`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/bb674fa4-f4c8-4627-9a12-1d45958a443b)


```YAML
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  11m    deployment-controller  Scaled up replica set nginx-deployment-989f57c54 to 3
  Normal  ScalingReplicaSet  5m     deployment-controller  Scaled up replica set nginx-deployment-dc49f85cc to 1
  Normal  ScalingReplicaSet  4m49s  deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 2 from 3
  Normal  ScalingReplicaSet  4m49s  deployment-controller  Scaled up replica set nginx-deployment-dc49f85cc to 2 from 1
  Normal  ScalingReplicaSet  4m47s  deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 1 from 2
  Normal  ScalingReplicaSet  4m47s  deployment-controller  Scaled up replica set nginx-deployment-dc49f85cc to 3 from 2
  Normal  ScalingReplicaSet  4m45s  deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 0 from 1
```

Verify that the rolling update is in progress by checking the status of the deployment: `kubectl rollout status deployment/nginx-deployment`
If the rollout has been successfully completed, you'll see output similar to the following:

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/fd53ee9f-b9c2-4348-90d9-264dd801f576)


`kubectl get pods`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c115aefd-6f36-4f19-999e-0413a9b10ad5)



# RollingUpdate Strategy
The RollingUpdate strategy is a deployment strategy in Kubernetes that allows for controlled updates of a deployment with minimal disruption to the application.

When using the RollingUpdate strategy, Kubernetes ensures that the new version of the deployment is rolled out gradually while maintaining a specified number of replicas available at all times. This strategy follows these key principles:

    Incremental Updates: Kubernetes updates the deployment incrementally, one replica at a time, to avoid bringing down the entire application during the update process. It replaces old replicas with new ones gradually.

    Availability: The RollingUpdate strategy ensures that a minimum number of replicas are always available during the update. It ensures that the desired number of replicas is maintained as much as possible, preventing downtime or service disruptions.

    Rollback Capability: If any issues are detected during the update process, Kubernetes provides the ability to roll back to the previous version of the deployment, effectively undoing the update and restoring the previous state.

    Configurable Parameters: The RollingUpdate strategy allows for configuration of parameters such as the maximum number of unavailable replicas, the maximum number of newly created replicas, and the maximum surge of replicas. These parameters control the rate and capacity of the rolling update process.

By using the RollingUpdate strategy, you can ensure that your application remains available and responsive during the deployment update. It allows for seamless upgrades of your application while minimizing the impact on users.

To specify the RollingUpdate strategy in a deployment YAML file, you can use the strategy field with the type set to RollingUpdate, along with any additional parameters you want to configure, such as maxUnavailable or maxSurge. For example:
```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.19
        ports:
        - containerPort: 80
```
In the above example, the RollingUpdate strategy is specified with a maximum of 1 unavailable replica and a maximum of 1 surge replica during the update process.

Overall, the RollingUpdate strategy is a powerful mechanism provided by Kubernetes for managing controlled and gradual updates of deployments, ensuring high availability and seamless updates for your applications.
