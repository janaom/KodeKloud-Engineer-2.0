# Instructions

There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

1. Create a Jenkins job named `database-backup`.
2. Configure it to take a database dump of the `kodekloud_db01` database present on the `Database server` in `Stratos Datacenter`, the database user is `kodekloud_roy` and password is `asdfgdsd`.
3. The dump should be named in `db_$(date +%F).sql` format, where `date +%F` is the current date.
4. Copy the `db_$(date +%F).sql` dump to the `Backup Server` under location `/home/clint/db_backups`.
5. Further, schedule this job to run periodically at `*/10 * * * *` (please use this exact schedule format).

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.
2. Please make sure to define you cron expression like this `*/10 * * * *` (this is just an example to run job every 10 minutes).
3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

 # Solution

Install these plugins

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/4750be22-ebd7-49aa-b3a8-3d2e95d9a9dc)

Follow the screenshots

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/27730b2c-311f-46b6-8219-2f911b61c6d1)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f51031f5-6202-4868-a2f0-40038cf3b6d7)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/ee9179c6-503a-4450-ae6e-1bab0827a4e8)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a83a0272-3eea-42c0-8a48-29eec0454910)

Create a new job

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/1524cbde-51bf-412e-917f-8055f6e7333c)

Enter this command: 

mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > db_$(date +%F).sql

scp -o StrictHostKeyChecking=no db_$(date +%F).sql clint@stbkp01:/home/clint/db_backups

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/fbd0bcc8-f8c6-4e4c-8bfc-6ce729456d8a)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/0ce7f992-5e61-4196-be1d-21101beb7a52)

ssh peter@172.16.239.10

ssh-keygen -t rsa

ssh-copy-id clint@stbkp01

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b6a90033-d9d5-48e1-a094-f715deb45134)

ssh clint@stbkp01

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/7d814f22-0607-4853-9003-31c5c065c63f)

Click on Build Now

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/e42c85ce-060b-406c-8c81-891ac227b48b)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b623d0c4-7961-491c-b77b-b07c25e5e663)

Check the results again

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/22ab811b-7007-4b37-8a3f-76f76d5d1c71)









