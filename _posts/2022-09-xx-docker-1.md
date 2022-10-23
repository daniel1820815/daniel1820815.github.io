---
layout: post
title: "DevNet Expert blog series - Docker part 1"
date: 2022-10-28 18:00:00 +0200
categories: Containers
comments_id: # CREATE AND ADD ISSUE NO.
---

As you might know I am currently studying for the [Cisco Certified DevNet Expert](https://learningnetwork.cisco.com/s/devnet-expert){:target="_blank"} lab exam.

- Create a Docker image using Dockerfile

- Package and deploy a solution by using Docker Compose

- Package and deploy a solution by using Kubernetes

## Intro

In the first part of this blog series I show you how to create Docker images using Dockerfile and then run containers using these images. In my example, I use three containers running as an application. The application will contain the following components: A [NGNIX](https://hub.docker.com/_/nginx){:target="_blank"} load balancer container on the frontend which balances the requests between two similar application containers named APP1 and APP2.

```mermaid
stateDiagram 
    direction LR
    [*] --> LOADBALANCER: request
    LOADBALANCER --> APP1
    LOADBALANCER --> APP2
```

The idea for this scenario came originally from the Cisco On Demand E-Learning course [Developing Applications using Cisco Core Platforms and APIs (DEVCOR) v1.0](https://learningnetworkstore.cisco.com/on-demand-e-learning/developing-applications-using-cisco-core-platforms-and-apis-devcor-v1.0/ELT-DEVCOR-V1-024035.html){:target="_blank"} available on the [Cisco Learning Network Store](https://learningnetworkstore.cisco.com){:target="_blank"}. There was a little more complex scenario used to demonstrate containerized applications using Docker. Additionally it contained a MYSQL database in the backend to store the data which was not a container. I want keep it simple here and focus on Docker containers. Nevertheless I can highly recommend this course, especially for the labs used to demonstrate the topics.

Before I begin, let's make sure that Docker is running on my machine. I am using a virtual machine running Ubuntu 20.04.

```sh
developer@devbox:~$ docker version
Client:
 Version:           20.10.12
 API version:       1.41
 Go version:        go1.16.2
 Git commit:        20.10.12-0ubuntu2~20.04.1
 Built:             Wed Apr  6 02:14:38 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.12
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.2
  Git commit:       20.10.12-0ubuntu2~20.04.1
  Built:            Thu Feb 10 15:03:35 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.5.9-0ubuntu1~20.04.4
  GitCommit:        
 runc:
  Version:          1.1.0-0ubuntu1~20.04.1
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        
```

> *If Docker is not running on your machine you can check the [Install Docker Engine documentation](https://docs.docker.com/engine/install/){:target="_blank"} to look for instructions for your machine. I would also recommend to go through the [Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/){:target="_blank"} after you installed it.*

Docker is already running on my machine. Now let's check if there are any containers running or if there are any images:

```sh
developer@devbox:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

developer@devbox:~$ docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
developer@devbox:~$
```

The `docker ps` command lists containers and with the option `-a` it shows all containers, because by default it shows only running containers. The `docker image`command manages images and with the option `ls` it lists all locally available images. Nothing there so far, we have a green field. Now we quickly try running an official Docker image called [hello-world](https://hub.docker.com/_/hello-world){:target="_blank"} to test the Docker setup. I will explain the official Docker images in a minute.

```sh
developer@devbox:~$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:18a657d0cc1c7d0678a3fbea8b7eb4918bba25968d3e1b0adebfa71caddbc346
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

As you can see from the output I started a container using the image *hello-world*. Docker could not find it locally and then the steps described above were done. If we list now the containers and images again we will see more than before:

```sh
developer@devbox:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
824970e1855e   hello-world   "/hello"   22 minutes ago   Exited (0) 3 minutes ago             objective_neumann

developer@devbox:~$ docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    feb5d9fea6a5   13 months ago   13.3kB
developer@devbox:~$
```

The container was created but stopped after it streamed the output to my terminal as you can see from the status. The *hello-world* image is now available locally. Now I will show you how we create your own images from a Dockerfile and create containers running for the scenario described before.

## Create new Docker images using Dockerfile

First let's create two folders *app* and *lb* to separate the Dockerfiles and container specific files. Then we start with creating the Dockerfile for the application image in the *app* directory:

```docker
FROM python:3.9

COPY . /app
WORKDIR /app

RUN pip install flask
EXPOSE tcp/5000

CMD ["python3", "main.py"]
```

We use the official Docker image *python* from [Docker Hub](https://hub.docker.com){:target="_blank"} in version 3.9. Official Docker images are designed for most common use cases. They have clear documentation and use Docker best practices. The *COPY* statement is used to copy local files to a directory on the container. Then set the working directory with *WORKDIR* for the app. After that the container needs to install the Python library *flask* which is a lightweight web application framework which is used in the *main.py* file. We will look at it in a minute. The app will start the app with the *CMD* statement. Before that the *EXPOSE* statement is used to enable the container listening on the specified port, in our case tcp port 5000. The statements in the Dockerfile are called stages. When we build the Docker image we will see the different stages:

```sh

```

You can see that four stages were completed. The fifth and sixth stages which are related to the *EXPOSE* and *CMD* statements will be executed during image run. Let's check if the image is there:

```sh

```

With ```docker image inspect my-app``` you could take a look into the details of the image. I did not add the output here to avoid overloading this post with information. Now let's take a look at the 

```sh

```

By default, new Docker containers will be added to the default *bridge* network and will be able to communicate with other containers on that network.

```sh

```

As you can see from the output there are only the default Docker networks on my machine. The Docker network mode *host* for a container means, that it is not isolated from the Docker host network stack and the container does not get its own IP address allocated. When you create a network without specifying any options, it creates a *bridge* network with non-overlapping subnetwork for the network by default.



Let's inspect the details of the bridge network.

```sh

```

As you can see from the output, Docker created a bridge network by default with a local subnet of 172.19.0.0/16 with gateway 172.19.0.1. No other specific settings were made and currently there are no containers attached to the network. For more information about Docker networking please look at the [Docker Networking](https://docs.docker.com/network/){:target="_blank"} documentation. Let's move on and create the Docker images from a Dockerfile.


## Run the containers

## Summary and Outlook

### Links & References

#### NGINX

- [NGINX on Docker Hub](https://hub.docker.com/_/nginx){:target="_blank"}

#### Docker

- [Docker Documentation](https://docs.docker.com){:target="_blank"}
- [Docker Networking Overview](https://docs.docker.com/network/){:target="_blank"}
- [Docker Hub](https://hub.docker.com){:target="_blank"}

#### Python

- [Flask](https://pypi.org/project/Flask/){:target="_blank"}
