# Instructions

One of the DevOps engineers was working on to create a Dockerfile for Nginx. We need to build an image using that Dockerfile. The deployment must be done using a Jenkins pipeline. Below you can find more details about the same.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username `sarah` and password `Sarah_pass123`. There is a repository named sarah/web in Gitea.

Create/configure a Jenkins pipeline job named `nginx-container`, configure it to run on server `App Server 1`.

The pipeline can have just one stage named `Build`. (name is case sensitive)

In the Build stage, build an image named stregi01.stratos.xfusioncorp.com:5000/nginx:latest using the Dockerfile present under the Git repository. stregi01.stratos.xfusioncorp.com:5000 is the image registry server. After building the image push the same to the image registry server.

Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

Open Jenkins, install these Plugins:

SSH Build Agents, Credentials, Pipeline, Pipeline: Step API, Pipeline API, Pipeline: SCM Step, Pipeline: Supporting APIs, Pipeline Job,  Pipeline: Nodes and Processes, Pipeline: Basic Steps, Pipeline: Stage Step, Pipeline: Declarative Extensions Points API, Pipeline Declarative, Docker Pipeline, Git

Create Credentials

![image](https://github.com/user-attachments/assets/99fd8f78-8097-49b9-91dd-a6aa1200ab64)

Create a new Node

![image](https://github.com/user-attachments/assets/83424ab1-c8f1-48dd-84da-042eff0ff38c)

`ssh tony@stapp01`

`sudo su -`

`yum install -y java-17-openjdk.x86_64`

Relaunch agent

![image](https://github.com/user-attachments/assets/b76de462-07ce-4892-8b35-5a76db1b34e5)

Create a new job with this code

![image](https://github.com/user-attachments/assets/9db14aeb-aab2-4a75-b9fd-2f791a2ee646)

Check images before and after building the job

```bash
[root@stapp01 ~]# docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
[root@stapp01 ~]# docker images
REPOSITORY                                    TAG       IMAGE ID       CREATED          SIZE
stregi01.stratos.xfusioncorp.com:5000/ngnix   latest    5f766106767e   25 seconds ago   11.5MB
[root@stapp01 ~]# docker images
```
