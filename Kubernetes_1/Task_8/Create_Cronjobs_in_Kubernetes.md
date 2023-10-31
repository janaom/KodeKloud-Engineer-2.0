# Instructions

There are some jobs/tasks that need to be run regularly on different schedules. Currently the Nautilus DevOps team is working on developing some scripts that will be executed on different schedules, but for the time being the team is creating some cron jobs in Kubernetes cluster with some dummy commands (which will be replaced by original scripts later). Create a cronjob as per details given below:

1. Create a cronjob named `nautilus`.
2. Set Its schedule to something like `/4 * * * *`, you set any schedule for now.
3. Container name should be `cron-nautilus`.
4. Use `httpd` image with `latest tag` only and remember to mention the tag i.e `httpd:latest`.
5. Run a dummy command `echo Welcome to xfusioncorp!`.
6. Ensure restart policy is `OnFailure`.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

Create `cronjob.yaml`

```YAML
apiVersion: batch/v1
kind: CronJob
metadata:
  name: nautilus
spec:
  schedule: "*/4 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
            - name: cron-nautilus
              image: httpd:latest
              command: ["echo", "Welcome to xfusioncorp!"]
```

Run with: `kubectl apply -f cronjob.yaml`

Check the result with `kubectl get cronjob  -o wide`

And: `kubectl describe cronjob`
