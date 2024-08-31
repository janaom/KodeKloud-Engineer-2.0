# Instructions

The Nautilus development team had a meeting with the DevOps team where they discussed automating the deployment of one of their apps using Jenkins (the one in Stratos Datacenter). They want to auto deploy the new changes in case any developer pushes to the repository. As per the requirements mentioned below configure the required Jenkins job.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.

Similarly, you can access the Gitea UI using Gitea button, username and password for Git is sarah and Sarah_pass123 respectively. Under user sarah you will find a repository named web that is already cloned on the Storage server under sarah's home. sarah is a developer who is working on this repository.

1. Install httpd (whatever version is available in the yum repo by default) and configure it to serve on port 8080 on All app servers. You can make it part of your Jenkins job or you can do this step manually on all app servers.

2. Create a Jenkins job named nautilus-app-deployment and configure it in a way so that if anyone pushes any new change to the origin repository in master branch, the job should auto build and deploy the latest code on the Storage server under /var/www/html directory. Since /var/www/html on Storage server is shared among all apps.

3. SSH into Storage Server using sarah user credentials mentioned above. Under sarah user's home you will find a cloned Git repository named web. Under this repository there is an index.html file, update its content to Welcome to the xFusionCorp Industries, then push the changes to the origin into master branch. This push must trigger your Jenkins job and the latest changes must be deployed on the servers, also make sure it deploys the entire repository content not only index.html file.

Click on the App button on the top bar to access the app, you should be able to see the latest changes you deployed. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be any sub-directory like https://<LBR-URL>/web etc.

Note:
1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also some times Jenkins UI gets stuck when Jenkins service restarts in the back end so in such case please make sure to refresh the UI page.

2. Make sure Jenkins job passes even on repetitive runs as validation may try to build the job multiple times.

3. Deployment related tasks should be done by sudo user on the destination server to avoid any permission issues so make sure to configure your Jenkins job accordingly.

4. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

Connect to app 3 Apps

`ssh tony@stapp01`

`ssh steve@stapp02`

`ssh banner@stapp03`

`sudo su -`

Run these commands on each App:

```bash
sudo yum install httpd -y  
sudo sed -i 's/Listen 80/Listen 8080/g' /etc/httpd/conf/httpd.conf 
sudo systemctl restart httpd
```

Open the App

![image](https://github.com/user-attachments/assets/127c745f-0451-4c9b-aa09-2c7a921f8bb1)

Open Jenkins go to Plugins and install these: Git, SSH, SSH Credentials, SSH Build Agents.

Create a new job

![image](https://github.com/user-attachments/assets/347c8de7-931d-4547-a015-7a5744df6e25)

Use Poll SCM to run it every minute

![image](https://github.com/user-attachments/assets/981ccb4a-c3ee-4815-ade0-a66c3ede614e)

![image](https://github.com/user-attachments/assets/758de41d-9eaa-41fd-a058-cd97f6c9a7b5)

Connect to the Jenkins server:

`ssh jenkins@jenkins`

```bash
jenkins@jenkins:~$ ssh-keygen -t rsa
jenkins@jenkins:~$ cd .ssh/
jenkins@jenkins:~/.ssh$ ls
id_rsa  id_rsa.pub
jenkins@jenkins:~/.ssh$ 
jenkins@jenkins:~/.ssh$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDng22gkXs9P6pB9O1XDOa/z7uYYp911Lool2SkTtqnGWYOvdYkulBUxy34PTBDjLG9XU9CLmwgMWgP/fjaIEs8jQW+oMcHsdr0PygEAylwV3wIlyksQTKhn8Q22K4BDp25Q8AK77FnbHxPKJ8JWTwBi6yfV4R9ScMseA1oYhdVxBVp2JpByevraWWUfA7ah/WLGJbIkvmuGwBB6kBcYszb8/S3pekK0J94sO0M237Iw+YGjiabynl7JAj4bHMjY/daVmQbDYdL/oj/UB0o1fjxXpmvQoNT/wk8nnnLuQClkJfAPXvqcmNdFQ3fMTjc3AHwmWakMQb1WT8oAWOQAX81wK8nH0faevCXxg3PB1r2VVDlA+igI9qImlh9oycZUy1LGlvsUrnwP8LFDLxkX1GR1EFOKlWgy2NRfSw8JIgKD3UiF7F8T2uxsVsMFrIvme631hfVdHRij5cz2ixP7StpRb1Seu77+jIL1VBSkr+gu7NEWOGFP8i2KYVSnb5xCM0= jenkins@jenkins.stratos.xfusioncorp.com
jenkins@jenkins:~/.ssh$ 
```

Copy the key

`ssh natasha@ststor01`

`sudo su -`

```bash
[root@ststor01 ~]# pwd
/root
[root@ststor01 ~]# ls -a
.   .bash_logout   .bashrc  .ssh     anaconda-ks.cfg             anaconda-post.log
..  .bash_profile  .cshrc   .tcshrc  anaconda-post-nochroot.log  original-ks.cfg
[root@ststor01 ~]# cd .ssh/
[root@ststor01 .ssh]# ls
authorized_keys
[root@ststor01 .ssh]# cat authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDng22gkXs9P6pB9O1XDOa/z7uYYp911Lool2SkTtqnGWYOvdYkulBUxy34PTBDjLG9XU9CLmwgMWgP/fjaIEs8jQW+oMcHsdr0PygEAylwV3wIlyksQTKhn8Q22K4BDp25Q8AK77FnbHxPKJ8JWTwBi6yfV4R9ScMseA1oYhdVxBVp2JpByevraWWUfA7ah/WLGJbIkvmuGwBB6kBcYszb8/S3pekK0J94sO0M237Iw+YGjiabynl7JAj4bHMjY/daVmQbDYdL/oj/UB0o1fjxXpmvQoNT/wk8nnnLuQClkJfAPXvqcmNdFQ3fMTjc3AHwmWakMQb1WT8oAWOQAX81wK8nH0faevCXxg3PB1r2VVDlA+igI9qImlh9oycZUy1LGlvsUrnwP8LFDLxkX1GR1EFOKlWgy2NRfSw8JIgKD3UiF7F8T2uxsVsMFrIvme631hfVdHRij5cz2ixP7StpRb1Seu77+jIL1VBSkr+gu7NEWOGFP8i2KYVSnb5xCM0= jenkins@jenkins.stratos.xfusioncorp.com
```

Go back to the Jenkins job

![image](https://github.com/user-attachments/assets/94a63b1d-afb2-49b9-a87e-1e7c77facdf1)

Go back to natasha server

```bash
[root@ststor01 .ssh]# cd /var/www
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 root root 4096 Aug 12 13:20 html
[root@ststor01 www]# chown -R natasha html/
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 natasha root 4096 Aug 12 13:20 html
[root@ststor01 www]# 
```

Go back to Jenkins and rebuild the job

![image](https://github.com/user-attachments/assets/4273a593-92f4-4434-a6af-1b0ddd3466dd)

```bash
thor@jumphost ~$ ssh sarah@ststor01
sarah@ststor01's password: 
[sarah@ststor01 ~]$ pwd
/home/sarah
[sarah@ststor01 ~]$ git clone http://git.stratos.xfusioncorp.com/sarah/web.git
Cloning into 'web'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
[sarah@ststor01 ~]$ ls
web
[sarah@ststor01 ~]$ cd web/
[sarah@ststor01 web]$ ls
index.html
[sarah@ststor01 web]$ cat index.html 
Welcome
[sarah@ststor01 web]$ cat > index.html
Welcome to the xFusionCorp Industries
^C
[sarah@ststor01 web]$ cat index.html 
Welcome to the xFusionCorp Industries
[sarah@ststor01 web]$ git commit -am "Updated index.html"
[master 52c89c3] Updated index.html
 1 file changed, 1 insertion(+), 1 deletion(-)
[sarah@ststor01 web]$ git push origin master
Username for 'http://git.stratos.xfusioncorp.com': sarah
Password for 'http://sarah@git.stratos.xfusioncorp.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 290 bytes | 290.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: . Processing 1 references
remote: Processed 1 references in total
To http://git.stratos.xfusioncorp.com/sarah/web.git
   45be2ea..52c89c3  master -> master
[sarah@ststor01 web]$ 
```
We just updated it

![image](https://github.com/user-attachments/assets/4dbb2afa-8cbc-46f6-bdd0-a1a58eb6494d)

After another run of Jenkins job the App will be updated

![image](https://github.com/user-attachments/assets/1e837944-1feb-476a-910d-975a6d89c1d7)









