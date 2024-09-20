# Instructions

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username `sarah` and password `Sarah_pass123`.

There is a repository named sarah/mr_job in Gitea, which is cloned on the Storage server under `/home/natasha/mr_job` directory.

Update the index.html file under dev branch, and change its content from Welcome to Nautilus Group! to Welcome to xFusionCorp Industries!. Remember to push your changes to the origin repository.

After pushing the required changes, login to the Gitea server and you will find a pull request with title My First PR under `mr_job` repository. Merge this pull request.

Create/configure a Jenkins pipeline job named nginx-container, configure a pipeline as per details given below and run the pipeline on server `App Server 2`.

The pipeline must have two stages `Build` and `Deploy` (names are case sensitive).

In the Build stage, first clone the `sarah/mr_job` repository, then build an image named `stregi01.stratos.xfusioncorp.com:5000/nginx:latest` using the Dockerfile present under the root of the repository. stregi01.stratos.xfusioncorp.com:5000 is the image registry server. After building the image push the same to the image registry server.

In the Deploy stage, create a container named nginx-app using the image you built in the Build stage. Make sure to map container port to the host port `8080` and run the container in detached mode.

Make sure to build a successful job at least once so that you have at least one successful build # in the job history. Further, you can test the app using command curl `http://stapp02:8080` from the jump host.

Note:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

# Solution

I updated Gitea

![image](https://github.com/user-attachments/assets/c7c26599-0b30-4ef6-9e40-23495ecd79ab)

![image](https://github.com/user-attachments/assets/e1f47282-ab28-451e-bc46-6ee01bff7833)

Merged PR

![image](https://github.com/user-attachments/assets/e94d9055-9b9d-44ee-b4ab-cdd04b056553)

Installed Plugins on Jenkins (not sure if we need all of them since I tried different solutions):
SSH, SSH Build Agents, Credentials, Pipeline, Pipeline: Step API, Pipeline  API, Pipeline: SCM Step, Pipeline: Supporting APIs, Pipeline Job, Pipeline: Nodes and Processes, Pipeline: Basic Steps, Pipeline: Stage  Step, Pipeline: Declarative Extensions Points API, Pipeline: Declarative, Docker Pipeline, Git, Docker

Created Credentials

![image](https://github.com/user-attachments/assets/6f9ad4d7-98de-41a4-97ad-ea96a47365f9)

Connect to your App

`ssh steve@stapp02`

`sudo su -`

`yum install java-17-openjdk -y`

Check if `/home/steve/workspace/nginx-container` already exists

``` bash
[root@stapp02 workspace]# ls
nginx-container
[root@stapp02 workspace]# pwd
/home/steve/workspace
[root@stapp02 workspace]#
```

Created Node, Relaunched the Node

![image](https://github.com/user-attachments/assets/f5016e60-590d-4918-9a00-8b1677f534a8)

Created a new job

![image](https://github.com/user-attachments/assets/5e382ca2-29df-4e1c-a4b0-d792040f0b37)

Logs

```bash
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on stapp02 in /home/steve/workspace/nginx-container/workspace/nginx-container
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ rm -rf /home/steve/workspace/nginx-container/mr_job
[Pipeline] sh
+ git clone http://git.stratos.xfusioncorp.com/sarah/mr_job.git
Cloning into 'mr_job'...
[Pipeline] sh
+ cd mr_job
+ docker build -t stregi01.stratos.xfusioncorp.com:5000/nginx:latest .
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 109B done
#1 DONE 0.0s

#2 [internal] load metadata for docker.io/library/nginx:stable-alpine3.17-slim
#2 DONE 30.3s

#3 [internal] load .dockerignore
#3 transferring context: 2B done
#3 DONE 0.0s

#4 [1/2] FROM docker.io/library/nginx:stable-alpine3.17-slim@sha256:5893dc08a2cb01e21592ff469346ebaacf49167fbc949f45e1c29111981b0427
#4 DONE 0.0s

#5 [internal] load build context
#5 transferring context: 71B done
#5 DONE 0.0s

#6 [2/2] COPY index.html /usr/share/nginx/html/
#6 CACHED

#7 exporting to image
#7 exporting layers done
#7 writing image sha256:2f13ef1852a0eed80c06a70b221bc96c95b8c2d64cf213bc5f38ad5b9bda9676 done
#7 naming to stregi01.stratos.xfusioncorp.com:5000/nginx:latest done
#7 DONE 0.0s
[Pipeline] isUnix
[Pipeline] withEnv
[Pipeline] {
[Pipeline] sh
+ docker tag stregi01.stratos.xfusioncorp.com:5000/nginx:latest stregi01.stratos.xfusioncorp.com:5000/nginx:latest
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] isUnix
[Pipeline] withEnv
[Pipeline] {
[Pipeline] sh
+ docker push stregi01.stratos.xfusioncorp.com:5000/nginx:latest
The push refers to repository [stregi01.stratos.xfusioncorp.com:5000/nginx]
c011dd6ac196: Preparing
66afb7c3e6d1: Preparing
439be94dd989: Preparing
08761a690f84: Preparing
419b89bbfa3b: Preparing
5fa2288e0d6e: Preparing
f4111324080c: Preparing
5fa2288e0d6e: Waiting
f4111324080c: Waiting
66afb7c3e6d1: Layer already exists
08761a690f84: Layer already exists
419b89bbfa3b: Layer already exists
439be94dd989: Layer already exists
c011dd6ac196: Layer already exists
5fa2288e0d6e: Layer already exists
f4111324080c: Layer already exists
latest: digest: sha256:5566e23314b6c5c056dcc77dce3fc648525698ce9a2d184f1869eef79aa660c7 size: 1775
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ docker run -d --name nginx-app -p 8080:80 stregi01.stratos.xfusioncorp.com:5000/nginx:latest
b2512bdfb942f6324a924d2583b8b74daaac0b357713d265758d5266adcf3549
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

Check container and image

```bash
[root@stapp02 nginx-container]# docker ps
CONTAINER ID   IMAGE                                                COMMAND                  CREATED          STATUS          PORTS                  NAMES
b2512bdfb942   stregi01.stratos.xfusioncorp.com:5000/nginx:latest   "/docker-entrypoint.…"   26 seconds ago   Up 24 seconds   0.0.0.0:8080->80/tcp   nginx-app
[root@stapp02 nginx-container]# docker images
REPOSITORY                                    TAG       IMAGE ID       CREATED          SIZE
stregi01.stratos.xfusioncorp.com:5000/nginx   latest    2f13ef1852a0   15 minutes ago   11.5MB
```

Curl `http://stapp02:8080`

```bash
thor@jumphost ~$ curl http://stapp02:8080
Welcome to xFusionCorp Industries!
```

If you have any issues and want to rerun the job, run this first: `rm -rf mr_job`

Also you can check `mr_job`, `Dockerfile`, `index` file

```bash
[root@stapp02 ~]# cd /home
[root@stapp02 home]# ls
ansible  steve
[root@stapp02 home]# cd steve/
[root@stapp02 steve]# ls
workspace
[root@stapp02 steve]# cd workspace/
[root@stapp02 workspace]# ls
nginx-container
[root@stapp02 workspace]# cd nginx-container/
[root@stapp02 nginx-container]# ls
caches  remoting  remoting.jar  workspace
[root@stapp02 nginx-container]# cd workspace/
[root@stapp02 workspace]# ls
nginx-container  nginx-container@tmp
[root@stapp02 workspace]# cd nginx-container
[root@stapp02 nginx-container]# ls
mr_job
[root@stapp02 nginx-container]# cd mr_job/
[root@stapp02 mr_job]# ls
Dockerfile  index.html
[root@stapp02 mr_job]# cat Dockerfile 
FROM nginx:stable-alpine3.17-slim
COPY index.html /usr/share/nginx/html/
[root@stapp02 mr_job]# cat index.html 
Welcome to xFusionCorp Industries!
[root@stapp02 mr_job]# 
```
