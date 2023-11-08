# Instructions

One of the Nautilus project developers created a container on `App Server 3`. This container was created for testing only and now we need to delete it. Accomplish this task as per details given below:

Delete a container named `kke-container`, its running on `App Server 3` in Stratos DC.

# Solution

ssh into the App Server 3: `ssh banner@172.16.238.12`

Check containers: `docker ps`

Stop container: ``docker stop kke-container``

Remove container: `docker rm kke-container`
