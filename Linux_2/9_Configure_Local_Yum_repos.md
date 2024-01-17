# Instructions

The `Nautilus` production support team and security team had a meeting last month in which they decided to use local yum repositories for maintaing packages needed for their servers. For now they have decided to configure a local yum repo on `Nautilus Backup Server`. This is one of the pending items from last month, so please configure a local yum repository on `Nautilus Backup Server` as per details given below.

a. We have some packages already present at location `/packages/downloaded_rpms/` on `Nautilus Backup Server`.

b. Create a yum repo named `localyum` and make sure to set `Repository ID` to `localyum`.  Configure it to use package's location `/packages/downloaded_rpms/`.

c. Install package `wget` from this newly created repo.

# Solution

ssh clint@stbkp01

Open a terminal on the Nautilus Backup Server and run the following command to create a new repository configuration file: `sudo vi /etc/yum.repos.d/localyum.repo`

add

```YAML
[localyum]
name=localyum
baseurl=file:///packages/downloaded_rpms/
enabled=1
gpgcheck=0
```

To install the `wget` package from the newly created `localyum` repository, run the following command: `sudo yum install wget --disablerepo="*" --enablerepo="localyum”`

`yum list wget`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/49558563-55f3-446e-9cda-88233ee9ea9a)
