# Instructions

There is a requirement to Dockerize a Node app and to deploy the same on `App Server 2`. Under `/node_app` directory on `App Server 2`, we have already placed a `package.json` file that describes the app dependencies and `server.js` file that defines a web app framework.

1. Create a `Dockerfile` (name is case sensitive) under `/node_app` directory:
    - Use any `node` image as the base image.
    - Install the dependencies using `package.json` file.
    - Use `server.js` in the `CMD`.
    - Expose port `5003`.
2. The build image should be named as `nautilus/node-web-app`.
3. Now run a container named `nodeapp_nautilus` using this image.
    - Map the container port `5003` with the host port `8094`.

. Once deployed, you can test the app using a `curl` command on `App Server 2`:

`curl http://localhost:8094`

# Solution

`ssh steve@stapp02`

`sudo su -`

`cd /node_app`

`vi Dockerfile`

```yaml
FROM node:latest

WORKDIR /app

COPY package.json ./

RUN npm install

COPY . .

EXPOSE 5003

CMD ["node", "server.js"]
```

`docker build -t nautilus/node-web-app .`

`docker images`

```bash
[root@stapp02 node_app]# docker build -t nautilus/node-web-app .
Sending build context to Docker daemon  4.096kB
Step 1/7 : FROM node:latest
latest: Pulling from library/node
c6cf28de8a06: Pull complete 
891494355808: Pull complete 
6582c62583ef: Pull complete 
bf2c3e352f3d: Pull complete 
2b542796ccab: Pull complete 
e785d3fb8448: Pull complete 
8cd9c90782e6: Pull complete 
3b4414c82700: Pull complete 
Digest: sha256:a8ba58f54e770a0f910ec36d25f8a4f1670e741a58c2e6358b2c30b575c84263
Status: Downloaded newer image for node:latest
 ---> 3d4b037e6712
Step 2/7 : WORKDIR /app
 ---> Running in 75ab14a1f2f3
Removing intermediate container 75ab14a1f2f3
 ---> 1357d7e2129b
Step 3/7 : COPY package.json ./
 ---> 8b0c15050e87
Step 4/7 : RUN npm install
 ---> Running in 7f0bacb78071

added 64 packages, and audited 65 packages in 3s

12 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice
npm notice New minor version of npm available! 10.7.0 -> 10.8.0
npm notice Changelog: https://github.com/npm/cli/releases/tag/v10.8.0
npm notice To update run: npm install -g npm@10.8.0
npm notice
Removing intermediate container 7f0bacb78071
 ---> 726f25d1d8d4
Step 5/7 : COPY . .
 ---> 773375b1a622
Step 6/7 : EXPOSE 5003
 ---> Running in f4aab2c52bda
Removing intermediate container f4aab2c52bda
 ---> 732152f14ba4
Step 7/7 : CMD ["node", "server.js"]
 ---> Running in 4643f57fa4df
Removing intermediate container 4643f57fa4df
 ---> 66aaaaa3d0cb
Successfully built 66aaaaa3d0cb
Successfully tagged nautilus/node-web-app:latest
[root@stapp02 node_app]# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED              SIZE
nautilus/node-web-app   latest              66aaaaa3d0cb        About a minute ago   1.12GB
node                    latest              3d4b037e6712        8 days ago           1.11GB
```

`docker run -p 8094:5003 --name nodeapp_nautilus -d nautilus/node-web-app`

`docker ps`

```bash
[root@stapp02 node_app]# docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                    NAMES
8aaccf143dd5        nautilus/node-web-app   "docker-entrypoint.s…"   2 minutes ago       Up About a minute   0.0.0.0:8094->5003/tcp   nodeapp_nautilus
```

`curl http://localhost:8094`
