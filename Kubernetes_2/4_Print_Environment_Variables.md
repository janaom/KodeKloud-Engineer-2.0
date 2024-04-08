# Instructions

The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.

1. Create a `pod` named `print-envars-greeting`.
2. Configure spec as, the container name should be `print-env-container` and use `bash` image.
3. Create three environment variables:

a. `GREETING` and its value should be `Welcome to`

b. `COMPANY` and its value should be `xFusionCorp`

c. `GROUP` and its value should be `Datacenter`

1. Use command `["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']` (please use this exact command), also set its `restartPolicy` policy to `Never` to avoid crash loop back.
2. You can check the output using `kubectl logs -f print-envars-greeting` command.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

Create pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  restartPolicy: Never
  containers:
    - name: print-env-container
      image: bash
      command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
      env:
        - name: GREETING
          value: "Welcome to"
        - name: COMPANY
          value: "xFusionCorp"
        - name: GROUP
          value: "Datacenter"
```

Apply the configuration to create the pod: `kubectl apply -f pod.yaml`

To check the output, you can use the following command: `kubectl logs -f print-envars-greeting`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/4f0efcf3-acba-41aa-bfe6-5231a8a8ce11)
