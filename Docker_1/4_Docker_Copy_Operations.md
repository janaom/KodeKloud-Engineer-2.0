# Instructions

The Nautilus DevOps team has some confidential data present on `App Server 3` in `Stratos Datacenter`. There is a container `ubuntu_latest`
 running on the same server. We received a request to copy some of the data from the docker host to the container. Below are more details about the task:

On `App Server 3` in `Stratos Datacenter` copy an encrypted file `/tmp/nautilus.txt.gpg` from docker host to `ubuntu_latest` container (running on same server) in `/home/` location. Please do not try to modify this file in any way.

# Solution

ssh into the App Server 3: `ssh banner@172.16.238.12`

Check containers: `docker ps`

Copy the file: `docker cp /tmp/nautilus.txt.gpg bed04cbb3c78:/home/` (docker cp /tmp/nautilus.txt.gpg <container_id>:/home/)

Check the result: `docker exec -it bed04cbb3c78 bash` (`docker exec -it <container_id> bash`)

And the: `cd /home/`
