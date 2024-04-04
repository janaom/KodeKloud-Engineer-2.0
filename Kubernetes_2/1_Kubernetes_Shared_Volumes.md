# Instructions

We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.

1. Create a pod named `volume-share-xfusion`.
2. For the first container, use image `debian` with `latest` tag only and remember to mention the tag i.e `debian:latest`, container should be named as `volume-container-xfusion-1`, and run a `sleep` command for it so that it remains in running state. Volume `volume-share` should be mounted at path `/tmp/official`.
3. For the second container, use image `debian` with the `latest` tag only and remember to mention the tag i.e `debian:latest`, container should be named as `volume-container-xfusion-2`, and again run a `sleep` command for it so that it remains in running state. Volume `volume-share` should be mounted at path `/tmp/apps`.
4. Volume name should be `volume-share` of type `emptyDir`.
5. After creating the pod, exec into the first container i.e `volume-container-xfusion-1`, and just for testing create a file `official.txt` with any content under the mounted path of first container i.e `/tmp/official`.
6. The file `official.txt` should be present under the mounted path `/tmp/apps` on the second container `volume-container-xfusion-2` as well, since they are using a shared volume.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`kubectl get services`

`kubectl get pods`

Create a file.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-xfusion
spec:
  containers:
    - name: volume-container-xfusion-1
      image: debian:latest
      command: ["sleep", "infinity"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/official
    - name: volume-container-xfusion-2
      image: debian:latest
      command: ["sleep", "infinity"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/apps
  volumes:
    - name: volume-share
      emptyDir: {}
```

`kubectl create -f file.yaml`

To execute a command in the first container (`volume-container-xfusion-1`) and create the `official.txt` file, you can use the following command:
`kubectl exec -it volume-share-xfusion -c volume-container-xfusion-1 -- touch /tmp/official/official.txt`

To check the result, you can use the following command to get a list of running pods: `kubectl get pods`

To verify if the file `official.txt` exists in both containers, you can execute the following commands:

For the first container (`volume-container-xfusion-1`):
`kubectl exec -it volume-share-xfusion -c volume-container-xfusion-1 -- ls /tmp/official`

You should see the `official.txt` file listed in the output.

For the second container (`volume-container-xfusion-2`):
`kubectl exec -it volume-share-xfusion -c volume-container-xfusion-2 -- ls /tmp/apps`

You should also see the `official.txt` file listed in the output.

If the file exists in both containers, it confirms that the shared volume is working correctly.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/7aad3e5d-38f5-406d-a358-6060493b9017)
