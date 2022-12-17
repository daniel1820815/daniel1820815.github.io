---
layout: post
title: "DevNet Expert series - Docker part 2"
date: 2022-12-17 12:00:00 +0200
categories: Containers
#comments_id: # CREATE AND ADD ISSUE NO.
---

This is part 2 of the DevNet Expert series about Containers using Docker and Kubernetes. It continues where we are at the end from the first part [DevNet Expert series - Docker part 1](https://blog.kuhlcloud.de/containers/2022/12/16/docker-part1.html){:target="_blank"}". If you do not like to go through the first part you can find the source files on the Github repository, clone it, and start through with the second part.

During the first part we focused on section 4.1 from the [exam blueprint](https://learningnetwork.cisco.com/s/devnet-expert-exam-topics-lab){:target="_blank"} which is about creating a Docker image using Dockerfile. We ignored Docker networking to keep it simple, but maybe it was not a good idea to put all Docker containers into the same network and also using the default network. We will take a closer look at Docker networking now according to section 4.2. which is called "Create, consume, and troubleshoot a Docker host and bridge-based networks and integrate them with external networks". Sounds interesting, right? I will show you how to expand our application framework example using the benefits of Docker networking.

Hope you are also excited about this second part of the Containers series like I am and you follow my journey towards the Cisco Certified DevNet Expert. Let's start!

#### Part 2: Docker networking

In the second part of this blog series 

Show frontend and backend networks

[<img src="/images/docker-app-networking-diagram.png" width="500"/>](/images/docker-app-networking-diagram.png)

create backend network without external connectivity and assign fixed IP addresses to APPs and LB

show external connectivity is not working

create frontend network with external connectivity and assign IP address to LB

LB connected to both networks
APP connected to backend only

```sh

```

As you can see from the output there are only the default Docker networks on my machine. The Docker network mode *host* for a container means, that it is not isolated from the Docker host network stack and the container does not get its own IP address allocated. When you create a network without specifying any options, it creates a *bridge* network with non-overlapping subnetwork for the network by default.

Let's inspect the details of the bridge network.

```sh

```

As you can see from the output, Docker created a bridge network by default with a local subnet of 172.19.0.0/16 with gateway 172.19.0.1. No other specific settings were made and currently there are no containers attached to the network. For more information about Docker networking please look at the [Docker Networking](https://docs.docker.com/network/){:target="_blank"} documentation. Let's move on and create the Docker images from a Dockerfile.

#### Docker

- [Docker Documentation](https://docs.docker.com){:target="_blank"}
- [Docker Networking Overview](https://docs.docker.com/network/){:target="_blank"}
