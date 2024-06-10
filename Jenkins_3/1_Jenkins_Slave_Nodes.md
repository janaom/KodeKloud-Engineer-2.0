# Instructions

The Nautilus DevOps team has installed and configured new Jenkins server in Stratos DC which they will use for CI/CD and for some automation tasks. There is a requirement to add all app servers as slave nodes in Jenkins so that they can perform tasks on these servers using Jenkins. Find below more details and accomplish the task accordingly.

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

1. Add all app servers as SSH build agent/slave nodes in Jenkins. Slave node name for `app server 1`, `app server 2` and `app server 3` must be `App_server_1`, `App_server_2`, `App_server_3` respectively.

2. Add labels as below:

`App_server_1 : stapp01`

`App_server_2 : stapp02`

`App_server_3 : stapp03`

3. Remote root directory for `App_server_1` must be `/home/tony/jenkins`, for `App_server_2` must be `/home/steve/jenkins` and for `App_server_3` must be `/home/banner/jenkins`.

4. Make sure slave nodes are online and working properly.

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

Dashboard → Manage Jenkins → Plugins → find and install SSH, SSH Build Agents

Dashboard → Manage Jenkins → Credentials → System → Global credentials (unrestricted)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/32e12363-982f-4678-86e1-06c3fb897a1d)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/471c7455-731e-47b0-b3de-323247d775f3)

Dashboard → Manage Jenkins → Nodes

Name: App_server_1

Remote root dir: /home/tony/jenkins

Labels: App_server_1 : stapp01

Launch method: Host: stapp01

Credentials: for tony

Add the same for App_server_2/3

Go to App_server_1 and Launch Agent

```bash
java.io.IOException: Java not found on hudson.slaves.SlaveComputer@69d0faef. Install Java 8 or Java 11 on the Agent.
	at hudson.plugins.sshslaves.JavaVersionChecker.resolveJava(JavaVersionChecker.java:83)
	at hudson.plugins.sshslaves.SSHLauncher.lambda$launch$0(SSHLauncher.java:460)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:829)
[06/10/24 16:40:37] Launch failed - cleaning up connection
[06/10/24 16:40:37] [SSH] Connection closed.
```

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/7c7d0c55-0d95-4274-8541-ed87e6816a43)

`ssh tony@stapp01`

`yum search java*`

`sudo yum install java-17-openjdk-devel.x86_64  -y`

Do that on all apps.

Come back to each App and relaunch agent

```bash
<===[JENKINS REMOTING CAPACITY]===>channel started
Remoting version: 3206.vb_15dcf73f6a_9
Launcher: SSHLauncher
Communication Protocol: Standard in/out
This is a Unix agent
Agent successfully connected and online
```

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f7bb168c-a2c3-426f-ab89-2ccffbf77f99)



