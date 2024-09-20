# Instructions

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123.

There is a repository named sarah/web in Gitea that is already cloned on Storage server under /var/www/html directory.

Update the content of the file index.html under the same repository to Welcome to xFusionCorp Industries and push the changes to the origin into the master branch.

Apache is already installed on all app Servers its running on port 8080.

Create a Jenkins pipeline job named deploy-job (it must not be a Multibranch pipeline job) and pipeline should have two stages Deploy and Test ( names are case sensitive ). Configure these stages as per details mentioned below.

a. The Deploy stage should deploy the code from web repository under /var/www/html on the Storage Server, as this location is already mounted to the document root /var/www/html of all app servers.

b. The Test stage should just test if the app is working fine and website is accessible. Its up to you how you design this stage to test it out, you can simply add a curl command as well to run a curl against the LBR URL (http://stlb01:8091) to see if the website is working or not. Make sure this stage fails in case the website/app is not working or if the Deploy stage fails.

Click on the App button on the top bar to see the latest changes you deployed. Please make sure the required content is loading on the main URL http://stlb01:8091 i.e there should not be a sub-directory like http://stlb01:8091/web etc.

Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

First, I updated manually index.html in Gitea

![image](https://github.com/user-attachments/assets/feaf21ba-4f2a-4f04-90e7-eb8a7e2a1aea)

I installed these Plugins in Jenkins

![image](https://github.com/user-attachments/assets/0aa73d69-5137-4160-abb6-30fa6a735a96)

I connected to ststor01

`ssh natasha@ststor01`

`sudo su -`

`yum install java-17-openjdk -y`

```bash
[root@ststor01 ~]# cd /var/www/html
[root@ststor01 html]# ls
index.html
[root@ststor01 html]# ls -la
total 16
drwxr-xr-x 3 natasha natasha 4096 Sep  5 09:01 .
drwxr-xr-x 3 natasha natasha 4096 Sep  5 08:59 ..
drwxr-xr-x 8 natasha natasha 4096 Sep  5 09:01 .git
-rw-r--r-- 1 natasha natasha    8 Sep  5 09:01 index.html
[root@ststor01 html]# cat index.html 
Welcome
[root@ststor01 html]# cat index.html   #check after you run the job in Jenkins
Welcome to xFusionCorp Industries
[root@ststor01 html]# 
```

I created credentials

![image](https://github.com/user-attachments/assets/7beb9f5c-672f-48da-8c8f-37310d992c45)

I created a new Node and relaunched it

![image](https://github.com/user-attachments/assets/a3de1d62-2d32-4547-a154-1d3d5928d5de)

![image](https://github.com/user-attachments/assets/0b1e9075-8383-46ca-9105-f74304b5b040)

I created a job

![image](https://github.com/user-attachments/assets/f8a50645-9f45-484c-92ea-f388bc48a25d)

Here are the logs

```bash
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Storage server in /var/www/html/workspace/deploy-job
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] git
The recommended git tool is: NONE
using credential GIT_CREDS
Cloning the remote Git repository
Cloning repository http://git.stratos.xfusioncorp.com/sarah/web.git
 > git init /var/www/html/workspace/deploy-job # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web.git
 > git --version # timeout=10
 > git --version # 'git version 2.43.0'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
Checking out Revision 73498fc65ec0097fad16dc9bead8e6b8fca80ca8 (refs/remotes/origin/master)
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 73498fc65ec0097fad16dc9bead8e6b8fca80ca8 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git checkout -b master 73498fc65ec0097fad16dc9bead8e6b8fca80ca8 # timeout=10
Commit message: "Update 'index.html'"
First time build. Skipping changelog.
[Pipeline] sh
+ cd /var/www/html
+ git pull origin master
From http://git.stratos.xfusioncorp.com/sarah/web
 * branch            master     -> FETCH_HEAD
   90188e6..73498fc  master     -> origin/master
Updating 90188e6..73498fc
Fast-forward
 index.html | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] withEnv
[Pipeline] {
[Pipeline] sh
+ curl http://stapp01:8080/
+ grep -F 'Welcome to xFusionCorp Industries'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100    34  100    34    0     0  17000      0 --:--:-- --:--:-- --:--:-- 34000
Welcome to xFusionCorp Industries
+ true
[Pipeline] sh
+ curl http://stapp02:8080/
+ grep -F 'Welcome to xFusionCorp Industries'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100    34  100    34    0     0  34000      0 --:--:-- --:--:-- --:--:-- 34000
Welcome to xFusionCorp Industries
+ true
[Pipeline] sh
+ curl http://stapp03:8080/
+ grep -F 'Welcome to xFusionCorp Industries'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100    34  100    34    0     0  34000      0 --:--:-- --:--:-- --:--:-- 34000
Welcome to xFusionCorp Industries
+ true
[Pipeline] sh
+ curl http://stlb01:8091/
+ grep -F 'Welcome to xFusionCorp Industries'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100    34  100    34    0     0  34000      0 --:--:-- --:--:-- --:--:-- 34000
Welcome to xFusionCorp Industries
+ true
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

Checked on each app, restarted on each app: `sudo systemctl restart httpd`

```bash
[root@stapp01 ~]# cd /var/www/html
[root@stapp01 html]# ls
caches  index.html  remoting  remoting.jar  workspace
[root@stapp01 html]# cat index.html 
Welcome to xFusionCorp Industries
[root@stapp01 html]#
[root@stapp01 html]# curl http://stapp01:8080
Welcome to xFusionCorp Industries
[root@stapp01 html]# 
[root@stapp01 html]# curl http://stlb01:8091
Welcome to xFusionCorp Industries
[root@stapp01 html]# 
```
Check the App

![image](https://github.com/user-attachments/assets/a9287c2d-b676-4c74-94ab-3eb2ee497551)





