# Instructions

The Nautilus team is planning to use Jenkins for some of their CI/CD pipelines. DevOps team has just installed a fresh Jenkins server and they are configuring it further to be available for use.

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

It has only a sample job for now. A new developer has joined the Nautilus application development team and they want this user to be added to the Jenkins server as per the details mentioned below:

1. Create a jenkins user named `james` with `BruCStnMT5` password, their full name should be `James` (it is case sensitive).

2. Using `Project-based Matrix Authorization Strategy` assign `overall read` permission to `james` user.

3. Remember to remove all permissions for `Anonymous` users (if given) and make sure `admin` user has overall `Administer` permissions.

4. There is one existing job, make sure `james` only has `read` permissions to that job (we are not worried about other permissions like Agent, SCM, etc.).

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, in case Jenkins UI gets stuck when Jenkins service restarts in the back end, please make sure to refresh the UI page.

2. Do not immediately click on `Finish` button if you have restarted the Jenkins service, please wait for Jenkins login page to come back before finishing your task.

3. For these kind of scenarios that required changes to be done from a web UI, please take screenshots of your work so that you can share the same with us for review purpose (in case your task is marked incomplete or failed). You may also consider using a screen recording software such as `loom.com` to record and share your work.

# Solution

Create the user

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/899f60d9-a508-4ac6-a93d-16fa2e64159c)

Check for Matrix Authorization Strategy in Plugins and install it

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f748b2e3-79f7-4b64-83ca-ab0b7240e6c9)

Changes permissions, at the end click on Apply/Save

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/03dacf5a-2885-43fc-8466-4f74fe444551)

Connect as James and check if you can see the job

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/59a4a39f-7fec-47ea-8a4c-16aed9780ca9)



