# Instructions

The Nautilus DevOps team want to create a time check pod in a particular Kubernetes namespace and record the logs. This might be initially used only for testing purposes, but later can be implemented in an existing cluster. Please find more details below about the task and perform it.

1. Create a pod called `time-check` in the `xfusion` namespace. This pod should run a container called `time-check`, container should use the `busybox` image with `latest tag` only and remember to mention tag i.e `busybox:latest`.
2. Create a config map called `time-config` with the data `TIME_FREQ=8` in the same namespace.
3. The time-check container should run the command: `while true; do date; sleep $TIME_FREQ;done` and should write the result to the location `/opt/devops/time/time-check.log`. Remember you will also need to add an environmental variable `TIME_FREQ` in the container, which should pick value from the config map `TIME_FREQ` key.
4. Create a volume `log-volume` and mount the same on `/opt/devops/time` within the container.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

Create a namespace: `kubectl create namespace xfusion`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f6407cce-3947-42c9-9965-61c643ff15de)


Create a `config.yaml`:
```YAML
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: xfusion
data:
  TIME_FREQ: "8"
```

Run with: `kubectl apply -f config.yaml`

To check configmap: `kubectl get configmap time-config -n xfusion`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/924ef355-045a-48bf-9bea-6dc2c6669bac)

Create a `pod.yaml`:
```YAML
apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: xfusion
spec:
  containers:
  - name: time-check
    image: busybox:latest
    command: ["/bin/sh", "-c", "while true; do date; sleep $TIME_FREQ; done >> /opt/devops/time/time-check.log"]
    env:
    - name: TIME_FREQ
      valueFrom:
        configMapKeyRef:
          name: time-config
          key: TIME_FREQ
    volumeMounts:
    - name: log-volume
      mountPath: /opt/devops/time
  volumes:
  - name: log-volume
    emptyDir: {}
```

Run with: `kubectl apply -f pod.yaml`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/be3e594a-a6d9-4ef0-addd-758ee3925342)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/7dcb2b03-876b-4752-a6eb-adab2e8c9069)

# Kubernetes docs
`https://kubernetes.io/docs/concepts/configuration/configmap/`

## ConfigMaps

A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

A ConfigMap allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable.


## ConfigMaps and Pods

You can write a Pod spec that refers to a ConfigMap and configures the container(s) in that Pod based on the data in the ConfigMap. The Pod and the ConfigMap must be in the same namespace.

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:

    Inside a container command and args
    Environment variables for a container
    Add a file in read-only volume, for the application to read
    Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap
