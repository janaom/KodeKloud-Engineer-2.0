# Instructions

Last week the Nautilus DevOps team met with the application development team and decided to containerize several of their applications. The DevOps team wants to do some testing per the following:

1. Install `docker-ce` and `docker-compose` packages on `App Server 1`.
2. Start `docker` service.

# Solution

(Infra details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

ssh into the App Server 1: ssh tony@172.16.238.10
![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f4583deb-5142-4ffc-a8de-dde39e52da39)

`sudo su -`

`curl -L "https://github.com/docker/compose/releases/download/1.28.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/24fb5fc9-1064-4c6d-b278-307d1e53b128)

`ls -la /usr/local/bin/docker-compose`
```
-rw-r--r-- 1 root root 12212176 Nov  6 18:25 /usr/local/bin/docker-compose
```

`chmod +x /usr/local/bin/docker-compose`

`ls -la /usr/local/bin/docker-compose`
```
-rwxr-xr-x 1 root root 12212176 Nov  6 18:25 /usr/local/bin/docker-compose
```

`docker-compose --version`
```
docker-compose version 1.28.6, build 5db8d86f
```

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/0eda4e1a-c4a8-4238-80ec-53f901304630)

`yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`

`yum install docker-ce docker-ce-cli containerd.io`

`rpm -qa |grep docker`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/48c7908f-ba1f-4d8f-b36c-3b1ab051a3e8)

`systemctl enable docker`

`systemctl start docker`

`systemctl status docker`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/8c354e22-2f87-45de-861d-69151b553418)

`docker --version`

`docker ps`

`docker-compose --version`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/319de39b-acc2-47d4-ac1b-8cbc527533b9)








