# Instructions

A python app needed to be Dockerized, and then it needs to be deployed on `App Server 1`. We have already copied a `requirements.txt` file (having the app dependencies) under `/python_app/src/` directory on `App Server 1`. Further complete this task as per details mentioned below:

1. Create a `Dockerfile` under `/python_app` directory:
    ◦ Use any `python` image as the base image.
    ◦ Install the dependencies using `requirements.txt` file.
    ◦ Expose the port `5001`.
    ◦ Run the `server.py` script using `CMD`.
2. Build an image named `nautilus/python-app` using this Dockerfile.
3. Once image is built, create a container named `pythonapp_nautilus`:
    ◦ Map port `5001` of the container to the host port `8094`.
4. Once deployed, you can test the app using `curl` command on `App Server 1`.
`curl http://localhost:8094/`

# Solution

`ssh tony@stapp01`

`cd python_app`

`vi Dockerfile`

```python
FROM python:latest

WORKDIR /usr/src/python_app

COPY ./src/requirements.txt ./

RUN pip install --no-cache-dir -r requirements.txt

COPY ./src .

EXPOSE 5001

CMD ["python", "server.py"]
```

`docker build -t nautilus/python-app .`

`docker images`

```bash
[root@stapp01 python_app]# docker build -t nautilus/python-app .
Sending build context to Docker daemon  4.608kB
Step 1/7 : FROM python:latest
latest: Pulling from library/python
c6cf28de8a06: Pull complete 
891494355808: Pull complete 
6582c62583ef: Pull complete 
bf2c3e352f3d: Pull complete 
a99509a32390: Pull complete 
d46a03def8d9: Pull complete 
4429b810e09e: Pull complete 
2a4ca5af09fa: Pull complete 
Digest: sha256:3966b81808d864099f802080d897cef36c01550472ab3955fdd716d1c665acd6
Status: Downloaded newer image for python:latest
 ---> 12e5ab9d51c8
Step 2/7 : WORKDIR /usr/src/python_app
 ---> Running in eb8a2e496c16
Removing intermediate container eb8a2e496c16
 ---> 7755f4aba465
Step 3/7 : COPY ./src/requirements.txt ./
 ---> ac8ae24af8a3
Step 4/7 : RUN pip install --no-cache-dir -r requirements.txt
 ---> Running in 046690db2f3f
Collecting flask (from -r requirements.txt (line 1))
  Downloading flask-3.0.3-py3-none-any.whl.metadata (3.2 kB)
Collecting Werkzeug>=3.0.0 (from flask->-r requirements.txt (line 1))
  Downloading werkzeug-3.0.3-py3-none-any.whl.metadata (3.7 kB)
Collecting Jinja2>=3.1.2 (from flask->-r requirements.txt (line 1))
  Downloading jinja2-3.1.4-py3-none-any.whl.metadata (2.6 kB)
Collecting itsdangerous>=2.1.2 (from flask->-r requirements.txt (line 1))
  Downloading itsdangerous-2.2.0-py3-none-any.whl.metadata (1.9 kB)
Collecting click>=8.1.3 (from flask->-r requirements.txt (line 1))
  Downloading click-8.1.7-py3-none-any.whl.metadata (3.0 kB)
Collecting blinker>=1.6.2 (from flask->-r requirements.txt (line 1))
  Downloading blinker-1.8.2-py3-none-any.whl.metadata (1.6 kB)
Collecting MarkupSafe>=2.0 (from Jinja2>=3.1.2->flask->-r requirements.txt (line 1))
  Downloading MarkupSafe-2.1.5-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (3.0 kB)
Downloading flask-3.0.3-py3-none-any.whl (101 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 101.7/101.7 kB 1.7 MB/s eta 0:00:00
Downloading blinker-1.8.2-py3-none-any.whl (9.5 kB)
Downloading click-8.1.7-py3-none-any.whl (97 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.9/97.9 kB 8.8 MB/s eta 0:00:00
Downloading itsdangerous-2.2.0-py3-none-any.whl (16 kB)
Downloading jinja2-3.1.4-py3-none-any.whl (133 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.3/133.3 kB 12.5 MB/s eta 0:00:00
Downloading werkzeug-3.0.3-py3-none-any.whl (227 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 227.3/227.3 kB 17.1 MB/s eta 0:00:00
Downloading MarkupSafe-2.1.5-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (28 kB)
Installing collected packages: MarkupSafe, itsdangerous, click, blinker, Werkzeug, Jinja2, flask
Successfully installed Jinja2-3.1.4 MarkupSafe-2.1.5 Werkzeug-3.0.3 blinker-1.8.2 click-8.1.7 flask-3.0.3 itsdangerous-2.2.0
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
Removing intermediate container 046690db2f3f
 ---> 523ca2817b76
Step 5/7 : COPY ./src .
 ---> 89096fde00f3
Step 6/7 : EXPOSE 5001
 ---> Running in 4e267da8d531
Removing intermediate container 4e267da8d531
 ---> b35f8dc73e34
Step 7/7 : CMD ["python", "server.py"]
 ---> Running in c7f6b8cdbc99
Removing intermediate container c7f6b8cdbc99
 ---> 87de05c52968
Successfully built 87de05c52968
Successfully tagged nautilus/python-app:latest
[root@stapp01 python_app]# docker images
REPOSITORY            TAG                 IMAGE ID            CREATED              SIZE
nautilus/python-app   latest              87de05c52968        About a minute ago   1.03GB
python                latest              12e5ab9d51c8        6 weeks ago          1.02GB
```

`docker run -p 8094:5001 --name pythonapp_nautilus -d nautilus/python-app`

`docker ps`

```bash
[root@stapp01 python_app]# docker ps
CONTAINER ID        IMAGE                 COMMAND              CREATED             STATUS              PORTS                    NAMES
7ef5cf43a083        nautilus/python-app   "python server.py"   3 minutes ago       Up 55 seconds       0.0.0.0:8094->5001/tcp   pythonapp_nautilus
```
`curl http://localhost:8094/`
