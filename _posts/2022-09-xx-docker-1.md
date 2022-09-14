---
layout: post
title: "DevNet Expert blog series - Docker part 1"
date: 2022-09-16 18:00:00 +0200
categories: Containers
comments_id: # CREATE AND ADD ISSUE NO.
---

As you might know I am currently studying for the [Cisco Certified DevNet Expert certification](https://learningnetwork.cisco.com/s/devnet-expert){:target="_blank"}.

- Create a Docker image using Dockerfile

- Package and deploy a solution by using Docker Compose

- Package and deploy a solution by using Kubernetes

## Intro

## Create a new docker network

Let's start with creating a new Docker network to isolate the new deployment from other existing containers. By default, new docker containers will be added to the default *bridge* network and will be able to communicate with other containers on that network. Before creating a new network check for existing container networks:

```zsh
[expert@devbox ~]$ docker network ls
NETWORK ID     NAME             DRIVER    SCOPE
33b17295e975   bridge           bridge    local
43b6478812cd   gitlab_default   bridge    local
42563378bde0   host             host      local
eed3f5c39cf7   none             null      local
```

As you can see from the output there are already some Docker networks on my devbox. The Docker network mode *host* for a container means, that it is not isolated from the Docker host network stack and the container does not get its own IP address allocated. When you create a network without specifying any options, it creates a *bridge* network with non-overlapping subnetwork for the network by default. That is what we want to create.

```zsh
[expert@devbox ~]$ docker network create my-network
ade9ab9d04c4ced907112faa81acf7d1f6b6faa1dfc174bb0d1d4ac9e482b970
```

Let's inspect the details of the bridge network.

```zsh
[expert@devbox ~]$ docker network inspect my-network
[
    {
        "Name": "my-network",
        "Id": "ade9ab9d04c4ced907112faa81acf7d1f6b6faa1dfc174bb0d1d4ac9e482b970",
        "Created": "2022-09-13T22:26:48.22250377+02:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.20.0.0/16",
                    "Gateway": "172.20.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

Docker automatically created a bridge network with a /16 subnet mask and the first IP address as gateway. The scope is local, no IPv6 enabled, and currently there are no containers attached to the network. So far so good for our case. For more information how to configure Docker networks please take a look at the [Docker documentation](https://docs.docker.com){:target="_blank"}. Now we continue with building the images from Dockerfiles.

## Build the images from Dockerfiles

## Run the containers

## Summary and Outlook

### Links & References

- [Docker Documentations](https://docs.docker.com){:target="_blank"}
