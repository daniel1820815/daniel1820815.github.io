---
layout: post
title: "DevNet Expert series - Docker part 2"
#date: 2022-12-27 12:00:00 +0200
date: 2022-12-17 12:00:00 +0200
categories: Containers
#comments_id: # CREATE AND ADD ISSUE NO.
---

This is part two of the DevNet Expert series about Containers using Docker and Kubernetes. It continues where we are at the end from the first part [DevNet Expert series - Docker part 1](https://blog.kuhlcloud.de/containers/2022/12/16/docker-part1.html){:target="_blank"}. If you do not like to go through the first part you can find the source files on the [Github repository](https://github.com/daniel1820815/devnet-expert-lab/tree/main/blog/docker){:target="_blank"}. You can simply clone it and start through with the second part.

During the first part we focused on section 4.1 from the [exam blueprint](https://learningnetwork.cisco.com/s/devnet-expert-exam-topics-lab){:target="_blank"} which is about creating a Docker image using Dockerfile. We ignored Docker networking to keep it simple, but maybe it was not a good idea to put all Docker containers into the same network and also using the default network. We will take a closer look at Docker networking now according to section 4.2. which is called "Create, consume, and troubleshoot a Docker host and bridge-based networks and integrate them with external networks". Sounds interesting, right? I will show you how to expand our application framework example using the benefits of Docker networking.

I am still using a simple lab setup in [Cisco Modeling Labs (CML)](https://developer.cisco.com/modeling-labs/){:target="_blank"} with a Ubuntu 20.04 machine as devbox running Docker and external connectivity. My lab topology file is available [here](https://github.com/daniel1820815/devnet-expert-lab/blob/main/blog/docker/){:target="_blank"} for download and import into CML. You could also use the official Candidate Workstation available for download on the [Cisco Learning Network](https://learningnetwork.cisco.com/s/article/devnet-expert-equipment-and-software-list){:target="_blank"}.

Hope you are also excited about this second part of the Containers series like I am and you follow my journey towards the Cisco Certified DevNet Expert. Let's start!

### Part 2: Docker networking

We will slightly change our design and create a frontend and a backend network in Docker to separate the containers. It is meaningful to keep different parts of functions isolated from each other and also control external connectivity of the containers. In our case the frontend network needs to have external connectivity as we had it in the default network before, but the backend network does not necessarily need to. The load balancer will be connected to both networks to serve the incoming requests and have connectivity to the application servers on the backend. The application servers will only have the backend network connected.

[<img src="/images/docker-app-networking-diagram.png" width="500"/>](/images/docker-app-networking-diagram.png)

As you can see from the diagram we will also take advantage of assigning IP addresses to our containers from the new networks to get a fixed IP setup. This will make the whole setup a little bit easier to manage because we do not need to start the containers in any specific order to make sure they get a specific IP address as we did before in part one.

To begin with we take a look at the Docker networks we have out of the box using the ```docker network ls``` command.

```bash
developer@devbox:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
6ba106d77aaa   bridge    bridge    local
0d30fa1bbb5e   host      host      local
91482370b08a   none      null      local
```

As you can see from the output there are only the default Docker networks on my machine. The Docker network mode *host* for a container means, that it is not isolated from the Docker host network stack and the container does not get its own IP address allocated. The *none* network has all networking disabled. When you create a network without specifying any options, it creates a *bridge* network with non-overlapping subnetwork by default. Bridge networks are usually used for applications running in standalone containers that need to communicate.

Let's take a closer look at the default bridge network *bridge* using ```docker network inspect bridge``` command. You can either use the network name or the network id to inspect the network.

```bash
developer@devbox:~$ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "6ba106d77aaa988cf9f1f7a776d859057e87c97de23e0d5b8c35009982a80dd1",
        "Created": "2022-12-10T11:58:16.95713737Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
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
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

As you can see from the output the Docker default bridge network has a local subnet of 172.17.0.0/16 with gateway 172.17.0.1. Let's focus on two other important settings for us. To provide external connectivity from the Docker containers attached to the bridge network the option *com.docker.network.bridge.enable_ip_masquerade* needs to be set to *true*. This option enables IP masquerading which is another wording for NAT (Network Address Translation). Then we have the option *com.docker.network.bridge.enable_icc* which enables inter container connectivity. That means containers attached to the same network are able to communicate with each other. We will see the differences in a minute in practice.

At this point I want to highlight some (but not all) differences between the Docker default bridge and user-defined bridges:

- **User-defined bridges provide provide better isolation**

    This is one of the key differences why we create user-defined networks in our example because we want to isolate the frontend and backend. By default all containers are attached to the default bridge unless specifying another network using the ```--network``` option.

- **Containers can be attached and detached from user-defined networks on the fly**

    By default you can't remove a container from the default bridge without stopping it while you can connect or disconnect it from user-defined networks. This adds more flexibility to the container management.

- **User-defined bridges provide automatic DNS resolution between containers**

    The last difference I want to highlight is the automatic DNS resolution between containers which allows us to use the names of the containers instead of IP addresses to communicate with them.

For more information about [Docker Networking](https://docs.docker.com/network/){:target="_blank"} and especially the [Differences between user-defined bridges and the default bridge](https://docs.docker.com/network/bridge/#differences-between-user-defined-bridges-and-the-default-bridge){:target="_blank"} please look at the documentation links. Let's move on and create our own Docker bridge networks.

#### Create new bridge networks

First we create the backend network named *backend-net* without external connectivity. We use the ```docker network create -d bridge``` command to create our new bridge network. We specify a subnet 172.21.0.0/16 and a gateway 172.21.0.1 using the corresponding ```--subnet``` and ```--gateway``` command options. In our example we want our backend application containers to not have connectivity to any external destinations therefore we will disable the IP masquerade option we talked about earlier. As we need connectivity between the containers on the backend, in our case between the load balancer and the app containers, we will enable the inter container connectivity.

```bash
docker network create -d bridge \
--subnet=172.21.0.0/16 \
--gateway=172.21.0.1 \
-o "com.docker.network.bridge.enable_ip_masquerade"="false" \
-o "com.docker.network.bridge.enable_icc"="true" \
backend-net
```

Test with alpine:

docker run -itd --rm --network=backend-net --ip=172.21.0.10 --name test1 alpine
docker run -it --rm --network=backend-net --ip=172.21.0.11 --name test2 alpine

```bash
# ping 172.21.0.1
PING 172.21.0.1 (172.21.0.1): 56 data bytes
64 bytes from 172.21.0.1: seq=0 ttl=64 time=0.230 ms
64 bytes from 172.21.0.1: seq=1 ttl=64 time=0.171 ms
64 bytes from 172.21.0.1: seq=2 ttl=64 time=0.095 ms
64 bytes from 172.21.0.1: seq=3 ttl=64 time=0.096 ms
^C
--- 172.21.0.1 ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 0.095/0.148/0.230 ms

# ping 172.21.0.10
PING 172.21.0.10 (172.21.0.10): 56 data bytes
64 bytes from 172.21.0.10: seq=0 ttl=64 time=0.314 ms
64 bytes from 172.21.0.10: seq=1 ttl=64 time=0.112 ms
64 bytes from 172.21.0.10: seq=2 ttl=64 time=0.110 ms
64 bytes from 172.21.0.10: seq=3 ttl=64 time=0.111 ms
^C
--- 172.21.0.10 ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 0.110/0.161/0.314 ms

# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
^C
--- 8.8.8.8 ping statistics ---
5 packets transmitted, 0 packets received, 100% packet loss
```

From the output...

docker kill test*

create frontend network with external connectivity but without inter container connectivity

```bash
docker network rm fronend-net
docker network create -d bridge \
--subnet=172.20.0.0/16 \
--gateway=172.20.0.1 \
-o "com.docker.network.bridge.enable_ip_masquerade"="true" \
-o "com.docker.network.bridge.enable_icc"="false" \
frontend-net
```

docker run -itd --rm --network=frontend-net --ip=172.20.0.10 --name test1 alpine
docker run -it --rm --network=frontend-net --ip=172.20.0.11 --name test2 alpine

```bash
# ping 172.20.0.1
PING 172.20.0.1 (172.20.0.1): 56 data bytes
64 bytes from 172.20.0.1: seq=0 ttl=64 time=0.227 ms
64 bytes from 172.20.0.1: seq=1 ttl=64 time=0.114 ms
64 bytes from 172.20.0.1: seq=2 ttl=64 time=0.116 ms
64 bytes from 172.20.0.1: seq=3 ttl=64 time=0.099 ms
^C
--- 172.20.0.1 ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 0.099/0.139/0.227 ms

# ping 172.20.0.10
PING 172.20.0.10 (172.20.0.10): 56 data bytes
^C
--- 172.20.0.10 ping statistics ---
4 packets transmitted, 0 packets received, 100% packet loss

# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=118 time=15.636 ms
64 bytes from 8.8.8.8: seq=1 ttl=118 time=15.050 ms
64 bytes from 8.8.8.8: seq=2 ttl=118 time=14.739 ms
64 bytes from 8.8.8.8: seq=3 ttl=118 time=14.807 ms
^C
--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 14.739/15.058/15.636 ms
```

From the output

LB connected to both networks
APP connected to backend only

#### Bring all containers up again

assign fixed IP addresses to APPs

show external connectivity is not working

docker run -itd --network=backend-net --ip=172.21.0.101 myapp

start

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
