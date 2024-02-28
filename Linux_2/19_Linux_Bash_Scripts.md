# Instructions

The production support team of `xFusionCorp Industries`is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. 
They have a static website running on `App Server 2` in `Stratos Datacenter`, and they need to create a bash script named `ecommerce_backup.sh` which should accomplish the following tasks. (Also remember to place the script under `/scripts` directory on `App Server 2`).

a. Create a zip archive named `xfusioncorp_ecommerce.zip` of `/var/www/html/ecommerce` directory.

b. Save the archive in `/backup/` on `App Server 2`. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on `Nautilus Backup Server`.

c. Copy the created archive to `Nautilus Backup Server` server in `/backup/` location.

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, `tony` in case of `App Server 1`) must be able to run it.

# Solution

`ssh steve@stapp02`

`vi /scripts/ecommerce_backup.sh`

Add this script

```python
#!/bin/bash

zip -r /backup/xfusioncorp_ecommerce.zip /var/www/html/ecommerce

scp /backup/xfusioncorp_ecommerce.zip clint@stbkp01:/backup
```

`ssh-keygen`

`ssh-copy-id clint@stbkp01`

Try to connect: `ssh clint@stbkp01`

`logout`

`cd /scripts`

`sh ecommerce_backup.sh`

`cd /backup`

`ls`

`ssh clint@stbkp01`

`cd /backup`

`ls`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/04cdd1a9-ef86-4ca5-bdb8-c0cea3184d68)
