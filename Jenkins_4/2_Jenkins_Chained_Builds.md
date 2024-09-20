# Instructions

The DevOps team was looking for a  solution where they want to restart Apache service on all app servers  if the deployment goes fine on these servers in Stratos Datacenter.  After having a discussion, they came up with a solution to use Jenkins  chained builds so that they can use a downstream job for services which  should only be triggered by the deployment job. So as per the  requirements mentioned below configure the required Jenkins jobs.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

Similarly you can access Gitea UI on port 8090 and username and password for Git is `sarah` and `Sarah_pass123` respectively. Under user sarah you will find a repository named web.

Apache is already installed and configured on all app server so no changes are needed there. The doc root `/var/www/html` on all these app servers is shared among the Storage server under `/var/www/html` directory.

1. Create a Jenkins job named `nautilus-app-deployment` and configure it to pull change from the master branch of web repository on Storage server under /var/www/html directory, which is already a local git repository tracking the origin web repository. Since /var/www/html on Storage server is a shared volume so changes should auto reflect on all apps.

2. Create another Jenkins job named `manage-services` and make it a downstream job for nautilus-app-deployment job. Things to take care about this job are:

a. This job should restart httpd service on all app servers.
b. Trigger this job only if the upstream job i.e `nautilus-app-deployment` is stable.

LB server is already configured. Click on the App  button on the top bar to access the app. You should be able to see the  latest changes you made. Please make sure the required content is  loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like  https://<LBR-URL>/web etc.

Note: 

 1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre.  Also some times Jenkins UI gets stuck when Jenkins service restarts in  the back end so in such case please make sure to refresh the UI page.

2. Make sure Jenkins job passes even on repetitive runs as validation may try to build the job multiple times.

3. Deployment related tasks should be done by sudo user on the destination server to avoid any permission issues so make sure to configure your Jenkins job accordingly.

4. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

Install these Jenkins plugins: SSH, SSH Credentials, SSH Build Agents, Git, Credentials, Pipeline, Publish over SSH

Create Credentials

![image](https://github.com/user-attachments/assets/a11bd8ba-c275-4277-bea8-89093edce419)

Connect to the Storage server

`ssh natasha@ststor01`

`sudo su -`

`yum install java-17-openjdk -y`

Create/Relaunch Node

![image](https://github.com/user-attachments/assets/f46455a2-fe5c-4898-9ffd-bb8f6a350614)

Check `/var/www/html`

```bash
[root@ststor01 ~]# cd /var/www/html
[root@ststor01 html]# ls
index.html
[root@ststor01 html]# cat index.html 
Welcome
[root@ststor01 html]#
```

Check Gitea index file

![image](https://github.com/user-attachments/assets/20047fa8-e332-4abb-8a24-cd1f4ce09cab)

Add SSH remote hosts to the System for each App

![image](https://github.com/user-attachments/assets/c8ff4983-6b19-4502-b17f-3237c1b94393)

Add SSH server (Publish Over SSH)

![image](https://github.com/user-attachments/assets/1328ed3c-4070-4626-b36a-d50bb103b6f9)

Create `nautilus-app-deployment` job

![image](https://github.com/user-attachments/assets/d7dcadae-d078-4c56-b6ac-974b4c2e3e70)

Run it (these are the final logs together with `Triggering a new build of manage-services`)

```bash
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on ststor01 in /var/www/html/workspace/nautilus-app-deployment
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Checkout)
[Pipeline] git
The recommended git tool is: NONE
using credential sarah
Fetching changes from the remote Git repository
Checking out Revision 95386b9b89b6b747ab02fc3774b09684093c2e13 (refs/remotes/origin/master)
Commit message: "Updated index.html file"
 > git rev-parse --resolve-git-dir /var/www/html/workspace/nautilus-app-deployment/.git # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web.git # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web.git
 > git --version # timeout=10
 > git --version # 'git version 2.43.0'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 95386b9b89b6b747ab02fc3774b09684093c2e13 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master 95386b9b89b6b747ab02fc3774b09684093c2e13 # timeout=10
 > git rev-list --no-walk 95386b9b89b6b747ab02fc3774b09684093c2e13 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Update Code)
[Pipeline] sh
+ cd /var/www/html
+ git pull origin master
From http://git.stratos.xfusioncorp.com/sarah/web
 * branch            master     -> FETCH_HEAD
Already up to date.
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Triggering a new build of manage-services #2
Finished: SUCCESS
```

Before and after running the job

```bash
[root@ststor01 html]# cat index.html 
Welcome
[root@ststor01 html]# cat index.html 
Welcome to KodeKloud!
```

Also check Apps

```bash
[root@ststor01 ~]# cd /var/www/html
[root@ststor01 html]# ls
caches  index.html  remoting  remoting.jar  workspace
[root@ststor01 html]# cat index.html 
Welcome to KodeKloud!
[root@ststor01 html]#
```

Then create a new job `manage-service` as a freestyle project

![image](https://github.com/user-attachments/assets/e324ef9a-6fe2-44a9-bd74-aef9c289eb4e)

Add this for all Apps

![image](https://github.com/user-attachments/assets/04b011e3-9fa3-42d1-a6e4-6bf289264c89)

Run it, here are the final logs

```bash
Started by upstream project "nautilus-app-deployment" build number 3
originally caused by:
 Started by user admin
Running as SYSTEM
Building on the built-in node in workspace /var/lib/jenkins/workspace/manage-services
[SSH] script:

echo Am3ric@ | sudo -S systemctl restart httpd

[SSH] executing...
[sudo] password for steve: 
[SSH] completed
[SSH] exit-status: 0

[SSH] script:

echo Ir0nM@n | sudo -S systemctl restart httpd

[SSH] executing...
[sudo] password for tony: 
[SSH] completed
[SSH] exit-status: 0

[SSH] script:

echo BigGr33n | sudo -S systemctl restart httpd

[SSH] executing...
[sudo] password for banner: 
[SSH] completed
[SSH] exit-status: 0

Finished: SUCCESS
```

Double check Apps `sudo systemctl status httpd`

![image](https://github.com/user-attachments/assets/b3268572-26ac-4ded-b21f-3264d0f258ca)

Open App

![image](https://github.com/user-attachments/assets/52e71f16-70ec-4cae-8691-63498b75f147)


