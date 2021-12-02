# How to make and run an app container using Docker

For a perspective of Nordlys infrastructure migration to Kubernetes present guide gives a short overview of the technology and gives an example of running an application in Docker ecosystem.

## What is Docker?

Docker is a platform for developing, running and organizing applications. The core advantage of Docker is reducing time between program development and delivery. By using Docker a developer is able to ship, test and delivery code quickly without using external infrastructure. 

Docker provides the ability to isolate an application in an environment called a container. Container used along with virtual machines which is and abstraction of actual hardware turning one server into many servers. Each virtual machine provides an operating system and necessary libraries.

![docker and vms](https://www.docker.com/sites/default/files/d8/2018-11/docker-containerized-and-vm-transparent-bg.png)

## What is Kubernetes?

Kubernetes is an open-source platform for managing and configuring containers. Kubernetes brings a framework to run distributed systems and helps with scaling and failover for an application, provides deployment patterns and so on.

To start a Kubernetes single-node cluster check the box **Enable Kubernetes** in Settings of Docker.

To learn more about Kubernetes [follow here](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/).

## Step 1. Get the Docker

[Download](https://www.docker.com/products/docker-desktop) the latest Docker release and install it.

## Step 2. Make a project structure

First, create a root directory **quickstart_docker**. 

Next, create subdirectories **application** and **docker** inside of it. Put **application** in the **docker** folder. 

To do this, type at the command line:
```
mkdir quickstart_docker
mkdir quickstart_docker/application
mkdir quickstart_docker/docker
mkdir quickstart_docker/docker/application
```

The project structure become as follows:
```
├── quickstart_docker
    ├── application
    ├── docker
      └── application
```

## Step 3. Compose an app

Prepare an app to operate with. This example app is a simple web server written in Python and it will be executable in future container.

Put the following code in **quickstart_docker/application/application.py** file:

```
import http.server
import socketserver
PORT = 8000
Handler = http.server.SimpleHTTPRequestHandler
httpd = socketserver.TCPServer(("", PORT), Handler)
print("serving at port", PORT)
httpd.serve_forever()
```

## Step 4. Create a Dockerfile

Dockerfile is a set of instructions for building a Docker image. A Docker image is a file that allow to execute code in a Docker container. 

Put the following code in Dockerfile into **quickstart_docker/docker/application/Dockerfile**:

```
FROM python:3.5                         #1 
WORKDIR /app                            #2 
COPY ./application /app                 #3
EXPOSE 8000                             #4
CMD ["python", "/app/application.py"]   #5
```

1. Use Python from a public [repository](https://docs.docker.com/docker-hub/repos/)
2. Set the working directory to /app
3. Copy the 'application' directory contents into the container at /app
4. Make port 8000 available to the world outside this container
5. Execute 'python /app/application.py' in the working directory when container launches


## Step 5. Make a Docker image

Next step is to create a Docker image by using **docker build** command.

To do this, type at the command line:

```
docker build . -f docker/application/Dockerfile -t exampleapp
```

Where **.** is a working directory, **-f docker/application/Dockerfile** is the Dockerfile itself,  **-t exampleapp** is the image name.

Check created Docker images list by executing the command:

```
docker images
```

| REPOSITORY | TAG    | IMAGE ID     | CREATED       | SIZE   |
|------------|--------|--------------|---------------|--------|
| exampleapp | latest | 83wse0edc28a | 2 seconds ago | 153 MB |
| python     | 3.6    | 05sob8636w3f | 6 weeks ago   | 153 MB |

To learn more about Docker imaging [follow here](https://docs.docker.com/engine/reference/builder/).

## Step 6. Run the container

Final step is to check running container.

Run the Exampleapp container by typing at the command line:

```
docker run exampleapp
```

[Explore](https://docs.docker.com/) some more features of Docker if necessary. 
