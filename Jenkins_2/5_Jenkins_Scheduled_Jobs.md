# Instructions

The devops team of xFusionCorp Industries is working on to setup centralised logging management system to maintain and analyse server logs easily. Since it will take some time to implement, they wanted to gather some server logs on a regular basis. At least one of the app servers is having issues with the Apache server. The team needs Apache logs so that they can identify and troubleshoot the issues easily if they arise. So they decided to create a Jenkins job to collect logs from the server. Please create/configure a Jenkins job as per details mentioned below:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`

1. Create a Jenkins jobs named `copy-logs`.

2. Configure it to periodically build `every 9 minutes` to copy the Apache logs (`both access_log and error_logs`) from `App Server 3` (`from default logs location`)  to location `/usr/src/security` on `Storage Server`.

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in 
the back end. In this case please make sure to refresh the UI page.

2. Please make sure to define you cron expression like this `*/10 * * * *` (this is just an example to run job every 10 minutes).

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

# Solution

Install these plugins

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/d1005c40-139b-48a0-8185-38c5c5ab45a1)

ssh to App Server 3 and check logs location

ssh banner@stapp03

sudo su

cd /var/

cd log

cd httpd

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/bcde16f8-d30d-4542-b17f-30ac6ad4ca11)

[banner@stapp03 ~]$ ssh-keygen -t rsa

cd .ssh/

ssh-copy-id natasha@ststor01

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f07f644e-ae1a-4a23-9605-d24bde867314)

Add credentials

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/ad8bc3bb-2c53-452b-acb6-e2e974f31f6d)

Configure ssh remote host

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/7ee421b2-879c-49fb-b5e5-659755f82093)

Test connection

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c45c3ade-7aaf-42f5-974b-94fe5049e50d)

Create a job

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/ef03d92d-dbc6-4c38-9a42-a6951df87cb3)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f3cbfbc3-22ac-4ddd-9181-77b06b8fe2f3)

Add scp /var/log/httpd/* natasha@ststor01:/usr/src/security

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c596b505-b6e9-44a9-ad33-2ab91620910a)

Click Build Now

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/65312980-e154-4702-b072-25d6d692ca94)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/de98a29e-266b-45c3-b6b0-533b5ef542d9)

Check cd /usr/src/security before and after executing the job

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a9ef1054-0b1f-458c-8bed-418dcd3b703b)







