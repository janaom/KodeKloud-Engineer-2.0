# Insructions

One of the DevOps team member was trying to install a WordPress website on a LAMP stack which is essentially deployed on Kubernetes cluster. It was working well and we could see the installation page a few hours ago. However something is messed up with the stack now due to a website went down. Please look into the issue and fix it:

FYI, deployment name is `lamp-wp`  and its using a service named `lamp-service`. The Apache is using http default port and nodeport is `30008`.
 From the application logs it has been identified that application is facing some issues while connecting to the database in addition to other issues. Additionally, there are some environment variables associated 
with the pods like `MYSQL_ROOT_PASSWORD, MYSQL_DATABASE,  MYSQL_USER, MYSQL_PASSWORD, MYSQL_HOST`.

Also do not try to delete/modify any other existing components like deployment name, service name, types, labels etc.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


# Solution

`k get pods`

`k describe pod lamp-wp-56c7c454fc-pmh6t`

`k get service`

`k describe service lamp-service`

`k edit service lamp-service`

Fix this part:
```yaml
NodePort:                 <unset>  30009/TCP  #should be 30008
```

To solve other issues, follow this link: https://github.com/joseeden/KodeKloud_Engineer_Labs/blob/main/Tasks_Kubernetes/Level_2/Lab_011_Fix_issue_with_LAMP_Environment_in_Kubernetes.md
