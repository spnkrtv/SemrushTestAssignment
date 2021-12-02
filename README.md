# How to make an app image and deploy it

Goal is to learn how to prepare [Docker](https://www.docker.com/) environment for interacting with apps and run a container with the ExampleApp using [Kubernetes](https://kubernetes.io) cluster. 

## What is Docker?

Docker is an open platform for developing, running and organize applications. The core advantage of Docker is reducing time between program developing and running it in production. By using Docker developer has a possibility for shipping, testing, and deploying code quickly without using external infrastructure.

Docker provides the ability to packing, testing, deploying and running an application in a loosely isolated environment called a container. Container used along with virtual machines which is and abstraction of actual hardware turning one server into many servers. Each virtual machine provides an operating system and necessary libraries.

![docker and vms](https://www.docker.com/sites/default/files/d8/2018-11/docker-containerized-and-vm-transparent-bg.png)

## What is Kubernetes?

Kubernetes is an open-source platform for managing and configuring containers. Kubernetes brings a framework to run distributed systems and helps with scaling and failover for an application, provides deployment patterns and so on. To learn more about Kubernetes [follow here](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/).

## Step 1. Get the Docker

[Download](https://www.docker.com/products/docker-desktop) the latest Docker release and install it

Have a look at step-by-step [tutorial](https://www.docker.com/docker-desktop/getting-started-for-mac) to customise it.

## Step 2. Make a directory

Every project needs a structure.

First, create a root directory **quickstart_docker**. 

Next, subdirectories **application** and **docker** inside of it. Put **application** in the **docker** folder. 

To do this, type at the command line:
```
mkdir quickstart_docker
mkdir quickstart_docker/application
mkdir quickstart_docker/docker
mkdir quickstart_docker/docker/application
```

Now the tree looks like this:
```
├── quickstart_docker
    ├── application
    ├── docker
      └── application
```

## Step 3. Compose an app

Write down the following code in any text editor and name it application.py
```
import http.server
import socketserver
PORT = 8000
Handler = http.server.SimpleHTTPRequestHandler
httpd = socketserver.TCPServer(("", PORT), Handler)
print("serving at port", PORT)
httpd.serve_forever()
```

Place ***application.py*** file into ***quickstart_docker/application*** folder

The app needs its environment, so it needs Python. And Python needs operational system. Dockerhub has all of these, so use it.

First, place Dockerfile file into ***quickstart_docker/docker/application*** directory with following containment:

```

FROM python:3.5                         #1 
WORKDIR /app                            #2 
COPY ./application /app                 #3
EXPOSE 8000                             #4
CMD ["python", "/app/application.py"]   #5

#1 Use base image from the registry
#2 Set the working directory to /app
#3 Copy the 'application' directory contents into the container at /app
#4 Make port 8000 available to the world outside this container
#5 Execute 'python /app/application.py' when container launches

```

Next, to make a Dockerfile and ExampleApp easy for search, type at the terminal:

```
docker build . -f docker/application/Dockerfile -t exampleapp -

```

To learn more about Docker imaging [follow here](https://docs.docker.com/engine/reference/builder/).

Check docker images list:

| REPOSITORY | TAG    | IMAGE ID     | CREATED       | SIZE   |
|------------|--------|--------------|---------------|--------|
| exampleapp | latest | 83wse0edc28a | 2 seconds ago | 153 MB |
| python     | 3.6    | 05sob8636w3f | 6 weeks ago   | 153 MB |


Docker image should be placed in repository
