---
layout: post
title: "DevNet Expert series - Docker part 2"
date: 2022-12-09 18:00:00 +0200
categories: Containers
#comments_id: # CREATE AND ADD ISSUE NO.
---

Not a good idea to put all the Docker containers into the default network...

#### Part 2: Docker networks

Create frontend and backend networks

frontend with external connectivity
backend without external connectivity

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
