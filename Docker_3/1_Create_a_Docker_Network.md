# Instructions

The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:

a. Create a docker network named as `official` on App Server `2` in `Stratos DC`.

b. Configure it to use `bridge` drivers.

c. Set it to use subnet `172.28.0.0/24`  and iprange `172.28.0.2/24`.

# Solution

`ssh steve@stapp02`

This command creates a new Docker network named "official”: `docker network create --driver bridge --subnet 172.28.0.0/24 --ip-range 172.28.0.2/24 official`

This command lists all the Docker networks available on your system: `docker network ls`

```bash
[steve@stapp02 ~]$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
57b58fb926da        bridge              bridge              local
e73bbf408cbe        host                host                local
da2adc277aba        none                null                local
9539a72b215d        official            bridge              local
```

This command retrieves detailed information about the Docker network named "official": `docker network inspect official`

```bash
[steve@stapp02 ~]$ docker network inspect official
[
    {
        "Name": "official",
        "Id": "9539a72b215d53bfefc8507b5fd9d70a23aa8b8e4c99def547c0a057af9c6afe",
        "Created": "2024-04-18T15:31:09.352329842Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.28.0.0/24",
                    "IPRange": "172.28.0.2/24"
                }
            ]
        },
        <...>
```
