# Instructions 

Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

Create a Jenkins  job named `install-packages` and configure it to accomplish below given tasks.

- Add a string parameter named `PACKAGE`.
- Configure it to install a package on the `storage server` in `Stratos Datacenter` provided to the `$PACKAGE` parameter.

`Note`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`.
 Also some times Jenkins UI gets stuck when Jenkins service restarts in 
the back end so in such case please make sure to refresh the UI page.

2. Make sure Jenkins job passes even on repetitive runs as validation may try to build the job multiple times.

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

# Solution


Connect to Jenkins, find and install ssh plugins

Create ssh credentials for the storage server user

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/15729f94-f1f7-44b6-af25-007c846065a3)

Add ssh host in Configure System, check connection, apply and save

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/26c33c70-eb38-4745-90ec-f3ccb7bb20cc)

Create a job with these parameters

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/4c914f76-1a31-44b6-b2e9-24fcb9ea448a)

Configure Build Steps, the command: echo 'Bl@kW' | sudo -S yum install -y $PACKAGE

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/79bdbbed-d139-4d3a-a9a5-ee14b49d0ea1)

Run the job

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c4703e7b-9450-4ffc-a49f-42deb250319d)




