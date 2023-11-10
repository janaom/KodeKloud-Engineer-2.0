# Instructions

There is a static website running within a container named `nautilus`, this container is running on `App Server 1`. Suddenly, we started facing some issues with the static website on `App Server 1`. Look into the issue to fix the same, you can find more details below:

1. Container's volume `/usr/local/apache2/htdocs` is mapped with the host's volume `/var/www/html`.
2. The website should run on host port `8080` on `App Server 1` i.e command `curl http://localhost:8080/` should work on `App Server 1`.

# Solution

ssh into the App Server 1: `ssh [tony@172.16.238.10](mailto:tony@172.16.238.10)`

Check all containers: `docker ps -a`

Restart container: `docker restart nautilus`

Check the result: `curl http://localhost:8080/`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/89dd0954-f847-4ad0-b765-baa4c7c6413a)
