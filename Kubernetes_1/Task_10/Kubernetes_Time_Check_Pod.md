# Instructions

The Nautilus DevOps team want to create a time check pod in a particular Kubernetes namespace and record the logs. This might be initially used only for testing purposes, but later can be implemented in an existing cluster. Please find more details below about the task and perform it.

1. Create a pod called `time-check` in the `xfusion` namespace. This pod should run a container called `time-check`, container should use the `busybox` image with `latest tag` only and remember to mention tag i.e `busybox:latest`.
2. Create a config map called `time-config` with the data `TIME_FREQ=8` in the same namespace.
3. The time-check container should run the command: `while true; do date; sleep $TIME_FREQ;done` and should write the result to the location `/opt/devops/time/time-check.log`. Remember you will also need to add an environmental variable `TIME_FREQ` in the container, which should pick value from the config map `TIME_FREQ` key.
4. Create a volume `log-volume` and mount the same on `/opt/devops/time` within the container.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

Create a namespace: `kubectl create namespace xfusion`

Create a `config.yaml`:

Run with: `kubectl apply -f config.yaml`

To check configmap: `kubectl get configmap time-config -n xfusion`


Create a `pod.yaml`:

Run with: `kubectl apply -f pod.yaml`
