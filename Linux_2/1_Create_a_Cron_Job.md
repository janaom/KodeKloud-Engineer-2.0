# Instructions

The `Nautilus` system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in `Stratos DC` on a set schedule. Before that they need to test similar functionality with a sample cron job. 
Therefore, perform the steps below:

a. Install `cronie` package on all `Nautilus` app servers and start `crond` service.

b. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.

# Solution

ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03

Run the appropriate command based on the package manager used by your servers:`sudo yum install cronie`

After installing the `cronie` package, start the `crond` service: `sudo systemctl start crond`

To check if the `crond` service is running on a Nautilus app server, you can use the following command: `sudo systemctl status crond`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/8b575d63-9335-4f4e-b8da-0653d9ba8f5f)

Open the crontab editor for the root user by executing the following command: `sudo crontab -e -u root`

In the crontab editor, add the following line: `*/5 * * * * echo hello > /tmp/cron_text`

Then open /tmp/ after 5min the file will appear there

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/38646582-e68a-4359-913b-2e7492804871)

