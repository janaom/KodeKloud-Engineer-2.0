# Instructions

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username `sarah` and password `Sarah_pass123`. There under user sarah you will find a repository named web_app that is already cloned on Storage server under /var/www/html. sarah is a developer who is working on this repository.

Add a slave node named `Storage Server`. It should be labeled as `ststor01` and its remote root directory should be /var/www/html.

We have already cloned repository on Storage Server under `/var/www/html`.

Apache is already installed on all app Servers its running on port 8080.

Create a Jenkins pipeline job named `devops-webapp-job` (it must not be a Multibranch pipeline) and configure it to:

Add a string parameter named BRANCH.
It should conditionally deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.
The pipeline should be conditional, if the value master is passed to the BRANCH parameter then it must deploy the master branch, on the other hand if the value feature is passed to the BRANCH parameter then it must deploy the feature branch.

LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.

Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

`ssh natasha@ststor01`

`sudo su -`

`yum install java-17-openjdk -y`

Open Jenkins, install Plugins

![image](https://github.com/user-attachments/assets/174a3b56-d00c-424d-a8a9-42f675020474)

Go back to the terminal:

```bash
[root@ststor01 ~]# cd /var/www
[root@ststor01 www]# ls
html
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 sarah sarah 4096 Aug 29 13:38 html
[root@ststor01 www]# chown -R natasha html/
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 natasha sarah 4096 Aug 29 13:38 html
[root@ststor01 www]# 
```

Go back to the Jenkins add credentials

![image](https://github.com/user-attachments/assets/ddf9597c-28e7-4327-8b1e-c979894ccbd9)

Create a new Node

![image](https://github.com/user-attachments/assets/5e4f178d-b4d3-4ea5-86d5-970aa47dc215)

![image](https://github.com/user-attachments/assets/656ae295-18b2-456d-81d6-8b5cbf014d2f)

![image](https://github.com/user-attachments/assets/50b41235-193c-487a-bde2-77177013f259)

![image](https://github.com/user-attachments/assets/375b1cb4-91a9-4c6a-b17f-9e56978e1a9e)

Relaunch agent

![image](https://github.com/user-attachments/assets/85195d33-8875-45fb-92f1-41b10bbf9bc7)

![image](https://github.com/user-attachments/assets/a045a718-2278-479e-9a7d-8b3989eb1a37)

Create a new job, add this script

```python
pipeline {
    agent {
        label 'ststor01'
    }
    
    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to deploy (master or feature)')
    }
    
    stages {
        stage('Deploy') {
            when {
                expression {
                    params.BRANCH == 'master' || params.BRANCH == 'feature'
                }
            }
            steps {
                script {
                    def repositoryPath = '/var/www/html/'

                    if (params.BRANCH == 'master') {
                        git branch: 'master',
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    } else if (params.BRANCH == 'feature') {
                        git branch: 'feature',
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    }
                    
                    sh "cp -r /var/www/html/workspace/devops-webapp-job/* /var/www/html/"
            }
                }
            }
        }
    }

```

Try to build

![image](https://github.com/user-attachments/assets/171cc941-1d88-42b6-9e17-4ce60c5f0c03)

![image](https://github.com/user-attachments/assets/741abda6-e9c7-4edb-b158-e8c868a7f51d)

![image](https://github.com/user-attachments/assets/215ed234-b336-4c7a-9368-178e82ebb87b)

![image](https://github.com/user-attachments/assets/24a0bd84-ad73-4087-afd0-e499ab52a476)

Open Build with Parameters, change Branch to 'feature' and Build

![image](https://github.com/user-attachments/assets/1a6a5d30-2f2f-4371-acb6-6eee89b44cb6)

![image](https://github.com/user-attachments/assets/32b97b1b-6329-4ce5-8cb3-e1d8d7e4d2cb)

Again rebuild with the master

![image](https://github.com/user-attachments/assets/58b86883-f07c-4472-b33d-94ca3422a943)

Changes come from Gitea

![image](https://github.com/user-attachments/assets/a408eb4d-368c-4fb6-8825-c10693ec6b40)

![image](https://github.com/user-attachments/assets/48deaed9-17b3-4342-abac-7506db08bcc2)
















