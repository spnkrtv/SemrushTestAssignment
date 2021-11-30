# How to make an app image and deploy it

Goal is to run a container with the ExampleApp in the [Kubernetes](https://kubernetes.io/docs/home/) cluster.

## Step 1. Get the Docker

[Download](https://www.docker.com/products/docker-desktop) the latest Docker release and install it

Have a look at step-by-step [tutorial](https://www.docker.com/docker-desktop/getting-started-for-mac) to customise it.

## Step 2. Make a directory

Every project needs a structure.

First, create a root directory ***quickstart_docker***. 

Next, subdirectories ***application*** and ***docker*** inside of it. Put ***application*** in the ***docker*** folder. 

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

## Step 3. Deploy an app

Get the app to start deploying.

Place ***application.py*** file into ***quickstart_docker/application*** catalog

```
import http.server
import socketserver
PORT = 8000
Handler = http.server.SimpleHTTPRequestHandler
httpd = socketserver.TCPServer(("", PORT), Handler)
print("serving at port", PORT)
httpd.serve_forever()
```
The app needs its environment, so it needs Python. And Python needs operational system. Dockerhub has all of these, so use it.

First, place Dockerfile file into ***quickstart_docker/docker/application*** directory with following containment:

```
# Use base image from the registry
FROM python:3.5
# Set the working directory to /app
WORKDIR /app
# Copy the 'application' directory contents into the container at /app
COPY ./application /app
# Make port 8000 available to the world outside this container
EXPOSE 8000
# Execute 'python /app/application.py' when container launches
CMD ["python", "/app/application.py"]
```

Next, to make a Dockerfile and ExampleApp easy for search, type at the terminal:

```
docker build . -f-docker/application/Dockerfile -t exampleapp
```

To learn more about Docker imaging [follow here](https://docs.docker.com/engine/reference/builder/).

Check docker images list:

| REPOSITORY | TAG    | IMAGE ID     | CREATED       | SIZE   |
|------------|--------|--------------|---------------|--------|
| exampleapp | latest | 83wse0edc28a | 2 seconds ago | 153 MB |
| python     | 3.6    | 05sob8636w3f | 6 weeks ago   | 153 MB |


Docker image should be placed in repository
