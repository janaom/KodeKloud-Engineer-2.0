# Instructions

A new DevOps Engineer has joined the team and he will be assigned some Jenkins related tasks. Before that, the team wanted to test a simple parameterized job to understand basic functionality of parameterized builds. He is given a simple parameterized job to build in Jenkins. Please find more details below:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

1. Create a `parameterized` job which should be named as `parameterized-job`

2. Add  a `string` parameter named `Stage`; its default value should be `Build`.

3. Add a `choice` parameter named `env`; its choices should be `Development`, `Staging` and `Production`.

4. Configure job to execute a shell command, which should echo both parameter values (you are passing in the job).

5. Build the Jenkins job at least once with choice parameter value `Production` to make sure it passes.

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in 
the back end. In this case, please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

# Solution

Create a new job

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/5bb248fe-2fe4-4346-90ed-90d0cfe9b8e9)

add String Parameter

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/58674ac0-440d-4760-b483-6289291fedde)

And Choice Parameter

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/8c6b601d-3c08-4c71-b375-d6feddc8a196)

And Build steps

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/685693ea-1fb9-4f6c-9b12-877c5d058d56)

Apply → Save

Open Build with Parameters and select Production, click Build

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/c056de4e-663e-46dc-adb8-27ae5e9d1d12)

Check the result

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/352be05f-8de9-469d-8cca-3e01ef1d4c4c)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/e5889faa-936f-4738-87a4-1cb6bfbdfc47)





