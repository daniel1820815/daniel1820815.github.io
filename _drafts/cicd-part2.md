---
layout: post
title: "CI/CD pipeline II - GitLab Docker containers"
date: 2023-09-14 12:00:00 +0200
categories: [Containers]
#comments_id: # CREATE AND ADD ISSUE NO.
---

#### CI/CD pipeline part 2 - GitLab Docker containers

{: style="text-align: justify" }

After the [introduction post]() of this blog post series I will start with the first and foremost component of the automated framework and CI/CD pipeline in part two of the series: **The GitLab Docker containers**. As described in the previous post the GitLab setup contains of two Docker containers: [GitLab CE (community edition)](https://gitlab.com/rluna-gitlab/gitlab-ce){:target="_blank"} and the GitLab runner. GitLab CE will be used as version control system for the code. The GitLab runner, which is registered to the GitLab CE container, is the agent that is responsible for all of the GitLab CI/CD features like running the pipeline jobs. I will use Docker Compose to create and run both containers.

ADD DIAGRAM

ADD THE INTRO FOR THE PYTHON SCRIPT HERE

Install git using ```sudo yum install git``` to clone the repository
screenshot

## Docker Compose overview

{: style="text-align: justify" }

Let me briefly describe Docker Compose first and explain some key feature without going to much into the details. Docker Compose let you **run multi-container Docker applications** using a YAML file which describes your configuration. You will be able to **manage the whole lifecycle of your application** like start, stop, and rebuild services. Furthermore you can view the status of running services, stream the log output of running services or run a one-off command on a specific service.

{: style="text-align: justify" }

Docker Compose has some key features I would like to highlight. One of them is the advantage to have **multiple isolated environments on a single host**. It means that Docker Compose uses a project name to isolate environments from each other. By default the project name is the name of the project directory, but you can also set a custom name as I will show you later.

{: style="text-align: justify" }

**Preserving the volume data of services** when containers are created is another feature which is very useful. If there is any container from previous runs, the volume data will be copied to the new container and no data is lost.

{: style="text-align: justify" }

Docker Compose caches the configuration used to create a container and when you restart a service on which are no changes, the cached configuration will be re-used. It will **only recreate containers that have changed** what saves time when there were no changes made.

{: style="text-align: justify" }

There are more Docker Compose key features and uses cases. If you would like to dive deeper into it, I highly recommend taking a look at the documentation pages which are very good written and designed in my opinion. Start with [Docker Compose overview](https://docs.docker.com/compose/){:target="_blank"} and take it from there. Let's take a look now at the Docker Compose file for the Gitlab setup.

explain docker compose file
version
services: gitlab and runner
volumes

within the services:
gitlab:
restart
image
hostname
volumes
ports
environment
runner:
explain the values used with the options described before
extra hosts

create and explain shell scripts
setup.sh
verifiy-runners.sh

create Python script out of the shell scripts

Maybe add Docker create image

## Links

- [Docker Compose overview](https://docs.docker.com/compose/){:target="_blank"}
