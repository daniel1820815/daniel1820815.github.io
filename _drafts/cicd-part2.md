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

{: style="text-align: justify" }

You can follow along step-by-step the creating of the Docker containers in this post or you can clone my GitHub repository with all files used in this blog post series. When using the CentOS 9 Linux machine we created in the first blog post then you need to install git first using ```sudo yum install git```.

![Installation of Git](/images/cicd_git_install.png "Installation of Git")
*Screenshot 1: Installation of Git*

{: style="text-align: justify" }

It will install *git* with all its dependencies as you can see from the screenshot above. Then you will be able to clone my repository from GitHub. All the files used to create the Docker containers can be found under *create_env* and the its subfolder *gitlab*.

```bash
git clone https://github.com/daniel1820815/cicd-pipeline.git
```

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

## Docker Compose file creation

{: style="text-align: justify" }

The Docker Compose file is used to configure your Docker application's services, networks, volumes, and more. The default file name in your working directory is ```compose.yaml``` or ```compose.yml```, but also ```docker-compose.yaml``` and ```docker-compose.yml``` are supported for backwards compatibility of earlier versions. I will use ```compose.yml``` as file name and start with defining the **services** as top-level element. The services define the computing components of an application. In our case we have two services ```gitlab``` and ```runner``` to define as mentioned before. The keys of the top-level services element are the names of the services and the values are service definitions.

```yaml
services: # The keys of the services are the name
  gitlab: 
  runner:
```

In the next step we give the gitlab service container a hostname. CHECK WITH AND WITHOUT HOSTNAME

{: style="text-align: justify" }

We specify the **images** which will be used in each services containers. The image must follow the Open Container [addressable image format](https://github.com/opencontainers/org/blob/master/docs/docs/introduction/digests.md){:target="_blank"}. We use ```image: gitlab/gitlab-ce``` and ```image: gitlab/gitlab-runner``` respectively. The [GitLab CE](https://hub.docker.com/r/gitlab/gitlab-ce) and [GitLab Runner](https://hub.docker.com/r/gitlab/gitlab-runner){:target="_blank"} images are available on [Docker Hub](https://hub.docker.com){:target="_blank"} for your reference. The images will be pulled from Docker Hub during the first run if not present locally and stored, so that they do not need to be pulled every time. We add the definition ```restart: always``` to both services, because by default a container does not restart in case of a termination under any circumstances. The restart policy always restarts the container until its removal.

```yaml
services:
  gitlab:
    hostname: gitlab            # added a hostname to EXPLAIN
    image: gitlab/gitlab-ce     # using the GitLab CE image from Docker Hub
    restart: always             # restart the service in case of a termination
  runner:
    image: gitlab/gitlab-runner # using the GitLab runner image on Docker Hub
    restart: always             # restart the service in case of a termination
```

{: style="text-align: justify" }

The services communicate with each other through **networks**. They store and share persistent data into **volumes**. In our case, we do not need any specific networks because we are using the default Docker network for both services to ensure they can communicate. But we want to use defined volumes to store our GitLab data. Therefore we define the volumes as another top-level element with two entries used for GitLab configuration files as ```gitlab-config``` and GitLab data files as ```gitlab-data``` specifying to use the local volume driver of the system with ```driver: local```. Then the volumes need to be defined under the service using ```VOLUME:CONTAINER_PATH``` syntax. Additionally you could specify the access mode with ```VOLUME:CONTAINER_PATH:ACCESS_MODE``` using ```rw``` or ```ro``` for example, but the default value is read and write access which is fine for us.

```yaml
services:
  gitlab:
    hostname: gitlab
    image: gitlab/gitlab-ce
    restart: always
    volumes:                            # add the volumes specified as top-level element
      - gitlab-config:/etc/gitlab
      - gitlab-data:/var/opt/gitlab
  runner:
    image: gitlab/gitlab-runner
    restart: always
    volumes:                            # mount local Docker service into container
      - /var/run/docker.sock:/var/run/docker.sock
volumes:                                # top-level volumes using local volume driver
  gitlab-config:
    driver: local
  gitlab-data:
    driver: local
```

{: style="text-align: justify" }

Docker needs to be available in the context of the image and for this to work we need to mount ```/var/run/docker.sock``` into the launched runner container as you can see above. If I understood this process correctly, it makes sure that we can run Docker-in-Docker which we need later in our CI/CD pipeline to start a Docker container running in GitLab Docker container.

{: style="text-align: justify" }

Now we get to specify the ```ports``` and add the entry ```80:80``` which exposes TCP port 80 of the host to TCP port 80 of the container. You could add the protocol values ```tcp``` or ```udp``` using the ```[HOST:]CONTAINER[/PROTOCOL]``` syntax to restrict the ports. By default ```tcp``` is used and therefore fine for us. At the runner service container we define ```devbox-cicd:192.168.11.220``` as ```extra_hosts``` which adds the hostname mapping to the container ```/etc/hosts``` network configuration file. In this case we add our of the Cent OS Linux machine hostname and IP address as mapping.

```yaml
services:
  gitlab:
    hostname: gitlab
    image: gitlab/gitlab-ce
    restart: always
    volumes:
      - gitlab-config:/etc/gitlab
      - gitlab-data:/var/opt/gitlab
    ports:                              # expose TCP port 80 from host to container
      - "80:80"
  runner:
    image: gitlab/gitlab-runner
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    extra_hosts:                        # specify extra hosts for /etc/hosts file
      - "devbox-cicd:192.168.11.220"
volumes:
  gitlab-config:
    driver: local
  gitlab-data:
    driver: local
```



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
