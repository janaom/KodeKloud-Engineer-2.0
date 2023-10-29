# Instructions

This morning the Nautilus DevOps team rolled out a new release for one of the applications. Recently one of the customers logged a complaint which seems to be about a bug related to the recent release. Therefore, the team wants to rollback the recent release.

There is a deployment named `nginx-deployment`; roll it back to the previous revision.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

`kubectl get all`


`kubectl describe deploy`


To roll back a deployment named nginx-deployment to the previous revision, you can use the kubectl rollout undo command. Here's how you can do it:

`kubectl rollout undo deployment/nginx-deployment`

This command will initiate a rollback process for the specified deployment, reverting it to the previous revision. Kubernetes will automatically handle the rollback, scaling down the new replicas and scaling up the previous replicas to restore the previous state of the deployment.

After running the rollback command, you can monitor the status of the rollback process using the `kubectl rollout status` command:

`kubectl rollout status deployment/nginx-deployment`

This command will display the current status of the rollback process, indicating whether the deployment has successfully rolled back to the previous revision or if the process is still in progress.

Note that the rollback process may take some time depending on the size of the deployment and the number of replicas. It's important to allow Kubernetes to complete the rollback process before verifying the state of the deployment.

By using the `kubectl rollout undo` command, you can roll back a deployment to the previous revision and restore the previous state of the application.
