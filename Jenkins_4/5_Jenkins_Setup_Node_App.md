# Instructions

The Nautilus application development team is working on to develop a Node app. They are still in the development phase however they want to deploy and test their app on a containerized environment and using a Jenkins pipeline. Please find below more details to complete this task.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123.

There is a repository named sarah/web in Gitea, which is cloned on the Storage server under /home/sarah/web directory.

A Dockerfile is already present under the git repository, please push the same to the origin repo if not pushed already.
Create a jenkins pipeline job named node-app and configure it as below:

Configure it to deploy the app on App Server 2
The pipeline must have two stages Build and Deploy (names are case sensitive)
In the Build stage, build an image named stregi01.stratos.xfusioncorp.com:5000/node-app:latest using the Dockerfile present under the Git repository. stregi01.stratos.xfusioncorp.com:5000 is the image registry server. After building the image push the same to the image registry server.
In the Deploy stage, create a container named node-app using the image you build it the Build stage. Make sure to map the container port with host port 8080.

Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

Install these Plugins SSH, SSH Credentials, SSH Build Agents, Git, Credentials, Pipeline.

ssh steve@stapp02

ssh natasha@ststor01

sudo su -

yum install java-17-openjdk -y

Create Credentials

![image](https://github.com/user-attachments/assets/6b028c9b-a6c0-4225-b941-41668eab3c28)

Create a new Node

![image](https://github.com/user-attachments/assets/c1d8b132-b5a4-41bf-8f9f-01dc01bf5675)

![image](https://github.com/user-attachments/assets/07c19252-d3e9-46f0-930b-e37226dd01d2)

![image](https://github.com/user-attachments/assets/dd297ff8-aa63-417f-abcb-f840dbe227a9)

Relaunch your Node

![image](https://github.com/user-attachments/assets/b2dac650-6f78-4da4-bf1f-2c004aafdd5b)

Create a new job

![image](https://github.com/user-attachments/assets/92bf412c-36d0-49db-8214-0b6b6d0cbfa3)

Build the job

Logs
```python
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on app02 in /home/steve/workspace/node-app
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] git
The recommended git tool is: NONE
No credentials specified
Fetching changes from the remote Git repository
Checking out Revision c229def6711440befe367932dd4d814403f13293 (refs/remotes/origin/master)
Commit message: "Added Dockerfile"
 > git rev-parse --resolve-git-dir /home/steve/workspace/node-app/.git # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web.git # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web.git
 > git --version # timeout=10
 > git --version # 'git version 2.43.0'
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f c229def6711440befe367932dd4d814403f13293 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master c229def6711440befe367932dd4d814403f13293 # timeout=10
 > git rev-list --no-walk c229def6711440befe367932dd4d814403f13293 # timeout=10
[Pipeline] sh
+ git checkout master
Already on 'master'
[Pipeline] sh
+ docker build -t stregi01.stratos.xfusioncorp.com:5000/node-app:latest .
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 279B done
#1 DONE 0.0s

#2 [internal] load metadata for docker.io/library/node:10-alpine
#2 DONE 0.6s

#3 [internal] load .dockerignore
#3 transferring context: 2B done
#3 DONE 0.0s

#4 [1/6] FROM docker.io/library/node:10-alpine@sha256:dc98dac24efd4254f75976c40bce46944697a110d06ce7fa47e7268470cf2e28
#4 resolve docker.io/library/node:10-alpine@sha256:dc98dac24efd4254f75976c40bce46944697a110d06ce7fa47e7268470cf2e28 0.0s done
#4 sha256:ddad3d7c1e96adf9153f8921a7c9790f880a390163df453be1566e9ef0d546e0 0B / 2.82MB 0.1s
#4 sha256:de915e575d22c7e33c83fddaf7aee0672e5d6a67e598a26fe0b30c7022f53cdd 0B / 22.21MB 0.1s
#4 sha256:7150aa69525b95f82b3df6a61a002f82382b2f3ea8ce51b9000b965f7476a5cc 0B / 2.35MB 0.1s
#4 sha256:dc98dac24efd4254f75976c40bce46944697a110d06ce7fa47e7268470cf2e28 1.65kB / 1.65kB done
#4 sha256:02767d92553e465bf51e0bd661074f2e70bd575c4a69a0d610aa6e78fd20a9bf 1.16kB / 1.16kB done
#4 sha256:aa67ba258e1877ed6ec455a7f4cc69e25cf0f0b027a7f6f3c63a8eca2c8a440c 6.73kB / 6.73kB done
#4 sha256:7150aa69525b95f82b3df6a61a002f82382b2f3ea8ce51b9000b965f7476a5cc 1.05MB / 2.35MB 0.3s
#4 ...

#5 [internal] load build context
#5 transferring context: 2.40MB 0.3s done
#5 DONE 0.3s

#4 [1/6] FROM docker.io/library/node:10-alpine@sha256:dc98dac24efd4254f75976c40bce46944697a110d06ce7fa47e7268470cf2e28
#4 sha256:ddad3d7c1e96adf9153f8921a7c9790f880a390163df453be1566e9ef0d546e0 2.82MB / 2.82MB 0.4s
#4 sha256:7150aa69525b95f82b3df6a61a002f82382b2f3ea8ce51b9000b965f7476a5cc 2.35MB / 2.35MB 0.3s done
#4 sha256:d7aa47be044e5a988e3e7f204e2e28cb9f070daa32ed081072ad6d5bf6c085d1 0B / 280B 0.4s
#4 sha256:ddad3d7c1e96adf9153f8921a7c9790f880a390163df453be1566e9ef0d546e0 2.82MB / 2.82MB 0.4s done
#4 sha256:d7aa47be044e5a988e3e7f204e2e28cb9f070daa32ed081072ad6d5bf6c085d1 280B / 280B 0.4s done
#4 extracting sha256:ddad3d7c1e96adf9153f8921a7c9790f880a390163df453be1566e9ef0d546e0 0.1s
#4 sha256:de915e575d22c7e33c83fddaf7aee0672e5d6a67e598a26fe0b30c7022f53cdd 5.97MB / 22.21MB 0.6s
#4 sha256:de915e575d22c7e33c83fddaf7aee0672e5d6a67e598a26fe0b30c7022f53cdd 9.44MB / 22.21MB 0.7s
#4 sha256:de915e575d22c7e33c83fddaf7aee0672e5d6a67e598a26fe0b30c7022f53cdd 13.33MB / 22.21MB 0.8s
#4 sha256:de915e575d22c7e33c83fddaf7aee0672e5d6a67e598a26fe0b30c7022f53cdd 22.21MB / 22.21MB 1.0s done
#4 extracting sha256:ddad3d7c1e96adf9153f8921a7c9790f880a390163df453be1566e9ef0d546e0 0.5s done
#4 extracting sha256:de915e575d22c7e33c83fddaf7aee0672e5d6a67e598a26fe0b30c7022f53cdd 0.1s
#4 extracting sha256:de915e575d22c7e33c83fddaf7aee0672e5d6a67e598a26fe0b30c7022f53cdd 3.9s done
#4 extracting sha256:7150aa69525b95f82b3df6a61a002f82382b2f3ea8ce51b9000b965f7476a5cc
#4 extracting sha256:7150aa69525b95f82b3df6a61a002f82382b2f3ea8ce51b9000b965f7476a5cc 0.9s done
#4 extracting sha256:d7aa47be044e5a988e3e7f204e2e28cb9f070daa32ed081072ad6d5bf6c085d1
#4 extracting sha256:d7aa47be044e5a988e3e7f204e2e28cb9f070daa32ed081072ad6d5bf6c085d1 0.8s done
#4 DONE 6.8s

#6 [2/6] RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app
#6 DONE 1.4s

#7 [3/6] WORKDIR /home/node/app
#7 DONE 0.8s

#8 [4/6] COPY package*.json ./
#8 DONE 0.7s

#9 [5/6] RUN npm install
#9 3.714 npm WARN nodejs-image-demo@1.0.0 No repository field.
#9 3.715 
#9 3.717 added 50 packages from 37 contributors and audited 50 packages in 2.049s
#9 3.739 found 3 vulnerabilities (1 moderate, 2 high)
#9 3.739   run `npm audit fix` to fix them, or `npm audit` for details
#9 DONE 3.9s

#10 [6/6] COPY --chown=node:node . .
#10 DONE 0.8s

#11 exporting to image
#11 exporting layers
#11 exporting layers 2.7s done
#11 writing image sha256:f949ac78061d6053479c31f70f5685ffaa254d169077b3ef4150bb3eed71a9eb done
#11 naming to stregi01.stratos.xfusioncorp.com:5000/node-app:latest done
#11 DONE 2.7s
[Pipeline] sh
+ docker push stregi01.stratos.xfusioncorp.com:5000/node-app:latest
The push refers to repository [stregi01.stratos.xfusioncorp.com:5000/node-app]
4089ee6df87c: Preparing
9c5fc203a215: Preparing
da984b639057: Preparing
5f70bf18a086: Preparing
76f73566786f: Preparing
edff9ff691d5: Preparing
cbe4b9146f86: Preparing
a6524c5b12a6: Preparing
9a5d14f9f550: Preparing
9a5d14f9f550: Waiting
cbe4b9146f86: Waiting
a6524c5b12a6: Waiting
edff9ff691d5: Waiting
5f70bf18a086: Pushed
76f73566786f: Pushed
da984b639057: Pushed
edff9ff691d5: Pushed
4089ee6df87c: Pushed
9c5fc203a215: Pushed
9a5d14f9f550: Pushed
cbe4b9146f86: Pushed
a6524c5b12a6: Pushed
latest: digest: sha256:39ffa0231bae6fc2dae5e7595592731be869c279317c914306efe9506cbc12a1 size: 2201
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] sh
+ docker run -d --name node-app -p 8080:8080 stregi01.stratos.xfusioncorp.com:5000/node-app:latest
93b8f29172ebd7f2fb8d446d8839fba46543626f1ae6ffcbb6b06b03c98a831c
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

![image](https://github.com/user-attachments/assets/50d39505-2337-4939-a8c2-c4b5265ac96c)

Check before running your job and after

```bash
[root@stapp02 ~]# docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
[root@stapp02 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@stapp02 ~]# docker images
REPOSITORY                                       TAG       IMAGE ID       CREATED         SIZE
stregi01.stratos.xfusioncorp.com:5000/node-app   latest    f949ac78061d   2 minutes ago   87.4MB
[root@stapp02 ~]# docker ps
CONTAINER ID   IMAGE                                                   COMMAND                  CREATED         STATUS         PORTS                    NAMES
93b8f29172eb   stregi01.stratos.xfusioncorp.com:5000/node-app:latest   "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:8080->8080/tcp   node-app
[root@stapp02 ~]# 
```
Curl before and after running the job

```bash
thor@jumphost ~$ curl stapp02:8080
curl: (7) Failed to connect to stapp02 port 8080: Connection refused
thor@jumphost ~$ curl stapp02:8080
<!DOCTYPE html>
<html lang="en">

<head>
    <title>About Sharks</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

<...>
```






