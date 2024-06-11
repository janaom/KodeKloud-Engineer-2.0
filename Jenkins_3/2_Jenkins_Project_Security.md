# Instructions

The xFusionCorp Industries has recruited some new developers. There are already some existing jobs on Jenkins and two of these new developers need permissions to access those jobs. The development team has already shared those requirements with the DevOps team, so as per details mentioned below grant required permissions to the developers.

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

1. There is an existing Jenkins job named `Packages`, there are also two existing Jenkins users named `sam` with password `sam@pass12345` and `rohan` with password `rohan@pass12345`.
2. Grant permissions to these users to access `Packages` job as per details mentioned below:
    
    a.) Make sure to select `Inherit permissions from parent ACL` under `inheritance strategy` for granting permissions to these users.
    
    b.) Grant mentioned permissions to `sam` user : `build`, `configure` and `read`.
    
    c.) Grant mentioned permissions to `rohan` user : `build`, `cancel`, `configure`, `read`, `update` and `tag`.
    

`Note:`

1. Please do not modify/alter any other existing job configuration.
2. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.
3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

# Solution

Dashboard → Manage Jenkins → Plugins → find and install Matrix Authorization Strategy

Follow these screenshots

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/cb09857b-723c-4b20-968a-fa98c74ea276)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f90261f5-6390-45fa-8e0d-15b8e3451717)

