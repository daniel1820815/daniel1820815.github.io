---
layout: post
title: "DevNet Expert series - Docker part 2"
date: 2022-12-17 12:00:00 +0200
categories: Containers
#comments_id: # CREATE AND ADD ISSUE NO.
---

This is part 2 of the DevNet Expert series about Containers using Docker and Kubernetes. It continues where we are at the end from the first part [DevNet Expert series - Docker part 1](https://blog.kuhlcloud.de/containers/2022/12/16/docker-part1.html){:target="_blank"}". If you do not like to go through the first part you can find the source files on the Github repository, clone it, and start through with the second part.

During the first part we focused on section 4.1 from the [exam blueprint](https://learningnetwork.cisco.com/s/devnet-expert-exam-topics-lab){:target="_blank"} which is about creating a Docker image using Dockerfile. We ignored Docker networking to keep it simple, but maybe it was not a good idea to put all Docker containers into the same network and also using the default network. We will take a closer look at Docker networking now according to section 4.2. which is called "Create, consume, and troubleshoot a Docker host and bridge-based networks and integrate them with external networks". Sounds interesting, right? I will show you how to expand our application framework example using the benefits of Docker networking.

I am still using a simple lab setup in [Cisco Modeling Labs (CML)](https://developer.cisco.com/modeling-labs/){:target="_blank"} with a Ubuntu 20.04 machine as devbox running Docker and external connectivity. My lab topology file is available [here](https://github.com/daniel1820815/devnet-expert-lab/blob/main/blog/docker/){:target="_blank"} for download and import into CML. You could also use the official Candidate Workstation available for download on the [Cisco Learning Network](https://learningnetwork.cisco.com/s/article/devnet-expert-equipment-and-software-list){:target="_blank"}.

Hope you are also excited about this second part of the Containers series like I am and you follow my journey towards the Cisco Certified DevNet Expert. Let's start!

### Part 2: Docker networking

We will slightly change our design and create a frontend and a backend network in Docker to separate the containers. It is meaningful to keep different parts of functions isolate from each other and also control external connectivity of the containers. In our case the frontend network needs to have external connectivity as we have it in the default network, but the backend network does not necessarily need to. The load balancer will be connected to both networks to serve the incoming requests and have connectivity to the application servers on the backend. The application servers will only have the backend network connected.

[<img src="/images/docker-app-networking-diagram.png" width="500"/>](/images/docker-app-networking-diagram.png)

As you can see from the diagram we will also take advantage of assigning IP addresses to our containers from the new networks to get a fixed IP setup. This will make the setup a little bit easier to handle because we do not need to start the containers in order to make sure they get a specific IP address as we did before in part 1.

To begin with we take a look at the Docker networks we have out of the box.

```docker network ls```

```bash

```

As you can see from the output there are only the default Docker networks on my machine. The Docker network mode *host* for a container means, that it is not isolated from the Docker host network stack and the container does not get its own IP address allocated. When you create a network without specifying any options, it creates a *bridge* network with non-overlapping subnetwork for the network by default.

docker inspect

```sh

```

As you can see from the output, Docker created a bridge network by default with a local subnet of 172.19.0.0/16 with gateway 172.19.0.1. No other specific settings were made and currently there are no containers attached to the network. For more information about Docker networking please look at the [Docker Networking](https://docs.docker.com/network/){:target="_blank"} documentation. Let's move on and create the Docker images from a Dockerfile.

#### Create new networks

create backend network without external connectivity and assign fixed IP addresses to APPs and LB

show external connectivity is not working

create frontend network with external connectivity and assign IP address to LB

LB connected to both networks
APP connected to backend only


### Links & References

#### Cisco Modeling Labs

- [Cisco Modeling Labs (CML)](https://developer.cisco.com/modeling-labs/){:target="_blank"}
- [CML Documentation](https://developer.cisco.com/docs/modeling-labs/){:target="_blank"}

#### Cisco Learning Network

- [Cisco Certified DevNet Expert Certification and Training](https://learningnetwork.cisco.com/s/devnet-expert){:target="_blank"}
- [DevNet Certifications Community](https://learningnetwork.cisco.com/s/topic/0TO3i0000008jY5GAI/devnet-certifications-community){:target="_blank"}
- [Cisco Certified DevNet Expert (v1.0) Equipment and Software List](https://learningnetwork.cisco.com/s/article/devnet-expert-equipment-and-software-list){:target="_blank"}

#### NGINX

- [nginx on Docker Hub](https://hub.docker.com/_/nginx){:target="_blank"}
- [nginx Documentation](https://nginx.org/en/docs/){:target="_blank"}

#### Docker

- [Docker Documentation](https://docs.docker.com){:target="_blank"}
- [Docker Hub](https://hub.docker.com){:target="_blank"}

#### Python

- [python on Docker Hub](https://hub.docker.com/_/python){:target="_blank"}
- [flask](https://pypi.org/project/Flask/){:target="_blank"}
- [socket](https://docs.python.org/3/library/socket.html){:target="_blank"}
