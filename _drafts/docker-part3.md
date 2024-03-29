---
layout: post
title: "DevNet Expert series - Docker part 3"
date: 2023-01-17 12:00:00 +0200
categories: Containers
#comments_id: # CREATE AND ADD ISSUE NO.
---

This is part three out of four of the DevNet Expert series about Containers using Docker and Kubernetes. It builds on top of the previous blog posts [DevNet Expert series - Docker part 1](https://blog.kuhlcloud.de/containers/2022/12/16/docker-part1.html){:target="_blank"} and [DevNet Expert series - Docker part 2](https://blog.kuhlcloud.de/containers/2022/12/16/docker-part1.html){:target="_blank"}. If you do not like to go through the first and second part you can find the source files on the [Github repository](https://github.com/daniel1820815/devnet-expert-lab/){:target="_blank"}. It is not a requirement to go through the previous posts necessarily.

During the first part we focused on section 4.1 from the [exam blueprint](https://learningnetwork.cisco.com/s/devnet-expert-exam-topics-lab){:target="_blank"} which is about creating a Docker image using Dockerfile. We ignored Docker networking during the first part and added that within the second part according to section 4.4. which is called "Create, consume, and troubleshoot a Docker host and bridge-based networks and integrate them with external networks". Now it is time to bring all together when using Docker Compose which covers section 4.2 "Package and deploy a solution by using Docker Compose" from the exam topics.

I am still using a simple lab setup in [Cisco Modeling Labs (CML)](https://developer.cisco.com/modeling-labs/){:target="_blank"} with a Ubuntu 20.04 machine as devbox running Docker and external connectivity. My lab topology file is available [here](https://github.com/daniel1820815/devnet-expert-lab/blob/main/blog/docker/){:target="_blank"} for download and import into CML. You could also use the official Candidate Workstation available for download on the [Cisco Learning Network](https://learningnetwork.cisco.com/s/article/devnet-expert-equipment-and-software-list){:target="_blank"}.

Hope you are still with me after two parts of the Containers series and follow my journey towards the Cisco Certified DevNet Expert. Let's start with Docker Compose!

### Part 3: Docker compose

Before we start we need to verify if Docker Compose is already installed. If you are using Docker Desktop/Toolbox on a Windows or Mac it should be already installed, but if you are on a Linux machine like I am it could be the case that you need to install it. If you are using my setup from the [Github repository](_blank"} with a Ubuntu 20.04 machine as devbox running Docker and external connectivity you should be fine. My lab topology file is available [here](https://github.com/daniel1820815/devnet-expert-lab/blob/main/blog/docker/){:target="_blank"}, please be aware that I made an update to the packages for the Devbox. You can either use the updated version from my repository or simply follow the [Docker installation](https://docs.docker.com/engine/install/){:target="_blank"} and then you should be at the same status.

Whatever way you chose in the end you need to get a similar output when using the ```docker compose version``` command.

```bash
developer@devbox:~$ docker compose version
Docker Compose version v2.15.1
```

We will use the same setup we used in the previous post but now 

Intro using Docker compose to build the app and lb containers including the networks we defined...

create docker-compose.yaml file

```bash
docker run -itd \
--network=backend-net --ip=172.21.0.101 \
--name myapp1 myapp
```

```bash
docker run -itd \
--network=backend-net --ip=172.21.0.102 \
--name myapp2 myapp
```

define the services

define the networks

run and test everything
