# Instructions

Some developers are working on a common repository where they are testing some features for an application. They are having three branches (excluding the master branch) in this repository where they are adding changes related to these different features. They want to test these changes on Stratos DC app servers so they need a Jenkins job using which they can deploy these different branches as per requirements. Configure a Jenkins job accordingly.

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on `Gitea` button to access the Gitea page. Login to `Gitea` server using username `sarah` and password `Sarah_pass123`.

1. There is a Git repository named `web_app` on Gitea where developers are pushing their changes. It has three branches `version1`, `version2` and `version3` (excluding the `master` branch). You need not to make any changes in the repository.
2. Create a Jenkins job named `app-job`.
3. Configure this job to have a choice parameter named `Branch` with choices as given below:

`version1`

`version2`

`version3`

1. Configure the job to fetch changes from above mentioned Git repository and make sure it should fetches the changes from the respective branch which you are passing as a choice in the choice parameter while building the job. For example if you choose `version1` then it must fetch and deploy the changes from branch `version1`.
2. Configure this job to use custom workspace rather than a default workspace and custom workspace directory should be created under `/var/lib/jenkins` (for example `/var/lib/jenkins/version1`) location rather than under any sub-directory etc.  The job  should use a workspace as per the value you will pass for `Branch` parameter while building the job. For example if you choose `version1` while building the job then it should create a workspace directory called `version1` and should fetch Git repository etc within that directory only.
3. Configure the job to deploy code (fetched from Git repository) on storage server (in Stratos DC) under `/var/www/html` directory. Since its a shared volume.
4. You can access the website by clicking on `App` button.

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.
2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

# Solution

This is the only solution that worked for me

Install Gitea and Publish Over SSH plugins

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/735647b3-2ca9-45a6-9e24-e70912641e7c)

Then just follow the screenshots

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/86024b40-88bf-495b-ab13-2ecaec821607)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a9ab7228-8776-43fa-afdb-dc62a021a855)

Add under Publish over SSH

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b4432734-1e14-4248-aaae-82d5dadba284)

Test configuration

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/64bcc801-15d1-4881-b520-c3dc1ff74cb9)

ssh into the Storage Server: `ssh natasha@172.16.238.15`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/53d59a02-165a-42e3-b4ac-ba9e736d73bf)

Create a job

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a47a69c7-dfbd-4e29-a9e4-6fd1469ee51f)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/05f94c8b-c824-469e-a779-73345714b084)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/ab8f481e-2858-49f4-8b5c-1c9c793ccafd)

Copy the link

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/da7e7b84-56c5-4b24-b438-07db65c6b4b9)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/8a22e6ff-bce9-45ba-9115-30caf296f110)

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/d12b1817-b2f4-456a-abb3-e11bbfc5b7ec)

Run the job, select 'version 1'. You should see `SSH: Transferred 1 file(s)`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/3d2b0f2f-eb08-47d9-a2af-46004f64cb12)

Open the App link

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/fce5539a-da09-4439-9a8e-c941906e4e9f)











