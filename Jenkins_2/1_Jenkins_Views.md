# Instructions

The DevOps team of xFusionCorp Industries is planning to create a number of Jenkins jobs for different tasks. So to easily manage the jobs within Jenkins UI they decided to create different views for all Jenkins jobs based on usage/nature of these jobs, - for example `nautilus-crons` view for all cron jobs. Based on the requirements shared below please perform the below mentioned task:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

1. Create a Jenkins job named `devops-test-job`.

2. Configure this job to run a simple bash command i.e `echo "hello world!!"`.

3. Create a view named `devops-crons` (it must be a `global` view of type `List View`) and make sure `devops-test-job` and  `devops-cron-job` (which is already present on Jenkins) jobs are listed under this new view.

4. Schedule this newly created job to build periodically at `every minute` i.e `* * * * *` (`please make sure to use the cron expression exactly same how it is mentioned here`)

5. Make sure the job builds successfully.

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

# Solution

Create a new job

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/8dde9b49-c134-4150-9b24-70195aaf1d0c)

Build Steps → Execute shell

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/43022b36-23ed-45fa-ad42-1112d020ca20)

Build Triggers → Build periodically

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/9fa0db8d-3ed4-4ed5-9202-50f89826f9bf)

Check Build #1 result

Check the Console Output

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/38f8ec3f-7cb9-41ce-a0b7-1db9246812dd)

Click New View (! Do not use My Views, more read [here](https://kodekloud.com/community/t/jenkins-views-error-in-validation/352413/10))

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/0dcc8c05-cbdb-4798-8af7-84da7cc93922)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/66ad85b8-1e21-477c-8d6e-8ad39f4d40ed)

Make sure you see this view, without admin

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b2e9c0cf-9369-41da-9004-838bd62aa62a)

This one is incorrect, you will get the error, e.g.  `- 'nautilus-crons' view doesn't exit or job 'nautilus-test-job' is not listed under it`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/daf8c572-4660-47b5-b406-a4424e96c6d9)





