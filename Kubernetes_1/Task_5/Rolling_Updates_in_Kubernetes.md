# Instructions
We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest changes and those changes need be deployed. The Nautilus DevOps team has created an image `nginx:1.19` with the latest changes.

Perform a rolling update for this application and incorporate `nginx:1.19` image. The deployment name is `nginx-deployment`

Make sure all pods are up and running after the update.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution
`kubectl get all`
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2ebaada8-1c06-4883-9db1-fa5bff723e3f/b35069fc-269f-4c31-a57a-e5d027447461/Untitled.png)

`kubectl describe deploy `nginx-deployment``
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2ebaada8-1c06-4883-9db1-fa5bff723e3f/e44eb223-91f1-4e4d-8a27-59f487bb69af/Untitled.png)

`kubectl edit deployment nginx-deployment`  change the image
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2ebaada8-1c06-4883-9db1-fa5bff723e3f/bd233b94-3a3f-4be7-bc1b-316604ce0470/Untitled.png)

`kubectl describe deploy `nginx-deployment``
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2ebaada8-1c06-4883-9db1-fa5bff723e3f/f09e39a3-5b0c-4811-9eee-a56699e1709c/Untitled.png)

```
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
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2ebaada8-1c06-4883-9db1-fa5bff723e3f/f6cf6a23-dc05-4ac1-80f0-16b2a2a81140/Untitled.png)

`kubectl get pods`
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2ebaada8-1c06-4883-9db1-fa5bff723e3f/ecda46be-c470-48ea-9f21-05fb77fd0407/Untitled.png)


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
