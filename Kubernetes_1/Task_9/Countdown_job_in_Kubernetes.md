# Instructions

The Nautilus DevOps team is working on to create few jobs in Kubernetes cluster. They might come up with some real scripts/commands to use, but for now they are preparing the templates and testing the jobs with dummy commands. Please create a job template as per details given below:

1. Create a job `countdown-devops`.
2. The spec template should be named as `countdown-devops` (under metadata), and the container should be named as `container-countdown-devops`
3. Use image `debian` with `latest` tag only and remember to mention tag i.e `debian:latest`, and restart policy should be `Never`.
4. Use command `sleep 5`

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

Create a job `job.yaml`

```YAML
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-devops
spec:
  template:
    metadata:
      name: countdown-devops
    spec:
      restartPolicy: Never
      containers:
      - name: container-countdown-devops
        image: debian:latest
        command: ["sleep", "5"]
```

Run with `kubectl apply -f job.yaml`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/58eb6cc3-3b70-4608-b9ea-66f41872b421)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b287a9a8-777e-4d47-8818-096e812d9661)


# Kubernetes docs

A Job creates one or more Pods and will continue to retry execution of the Pods until a specified number of them successfully terminate. As pods successfully complete, the Job tracks the successful completions. When a specified number of successful completions is reached, the task (ie, Job) is complete. Deleting a Job will clean up the Pods it created. Suspending a Job will delete its active Pods until the Job is resumed again.

A simple case is to create one Job object in order to reliably run one Pod to completion. The Job object will start a new Pod if the first Pod fails or is deleted (for example due to a node hardware failure or a node reboot).

You can also use a Job to run multiple Pods in parallel.

If you want to run a Job (either a single task, or several in parallel) on a schedule, see CronJob.
`https://kubernetes.io/docs/concepts/workloads/controllers/job/`

## Running an example Job

```YAML
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```
