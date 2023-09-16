---
layout: post
title: "CI/CD pipeline I - Intro"
date: 2023-09-14 12:00:00 +0200
categories: [Containers, Python]
#comments_id: # CREATE AND ADD ISSUE NO.
---

#### CI/CD pipeline part 1 - Introduction

{: style="text-align: justify" }

In this SEVEN part blog post series I will go through the process of building a GitLab CI/CD pipeline for automated network configuration changes including pre and post validation. The pipeline will consist of several components which will be prepared separately and be brought together in the end to a complete automated framework. During the first part I will go through all components and briefly explain what is their function in the CI/CD pipeline. Then I will show the preparation of a virtual Linux machine which will be used as basis for the work.

{: style="text-align: justify" }

The idea and inspiration for this blog post series comes from the videos from Adrian Iliesiu available on the YouTube and the corresponding Github repository. All my work is mostly based on his resources, but the implementation slightly differs based on my own ideas, lab setup, software versions, and other things. Nevertheless a big thank you to Adrian for this great work and inspiration.

## Components

{: style="text-align: justify" }

Here is an high-level overview of the CI/CD components used in this automated framework. More details on each components will come with each post addressing one of the components in detail and how to setup and build it. There is also my GitHub repository available to review and download all the files I have used during this process.

![CICD components](/images/cicd_components.png "CICD components")
*Diagram 1: CI/CD components overview*

### GitLab Docker container

{: style="text-align: justify" }

The basis of the automated framework is GitLab. It will run in Docker containers and it consists of two parts. First, there is GitLab CE (community edition) which will be used as version control system for the code and second there is a GitLab runner registered to GitLab. The GitLab runner is the agent that is responsible for all of the GitLab CI/CD features like running the pipeline jobs. In the second part of the blog post series I will setup the GitLab Docker containers using a Docker compose file.

### CML lab using Terraform

{: style="text-align: justify" }

Cisco Modeling Labs is the platform I am using for simulating the network devices. You can quickly and easily simulate Cisco and non-Cisco networks, using real Cisco images or third party images. It comes with an easy-to-use HTML5 UI and an API. Cisco Modeling Labs is the perfect tool to design, test, troubleshoot, and learn about Cisco networking, but not limited to Cisco products and technologies.

![Cisco Modeling Labs lab setup](/images/cicd_cml.png "Cisco Modeling Labs lab setup")
*Screenshot 1: Cisco Modeling Labs lab setup*

{: style="text-align: justify" }

In the third part of the blog post series I will show you how to spin up a virtual lab in Cisco Modeling Labs automatically using Terraform. The lab will have an external connector and an un-managed switch for out-of-band management of the devices. Then we will have two Cisco CAT8000V IOS-XE routers as core layer and two Cisco Nexus 9000v switches as distribution layer running NX-OS. Terraform will help to create the lab, the lab devices, all connections, and start the devices using an initial configuration.

### Docker image creation

{: style="text-align: justify" }

As described before, the GitLab runner is the agent which is responsible for running the pipeline jobs. The pipeline jobs will run in a Docker container using the default image or a specified image. In my CI/CD pipeline I will create my own Docker image and upload it to DockerHub. The image will contain all packages like Ansible and pyATS. The advantage of using your own image is that you have control of package and software versions. In the forth part of the blog post series I will go into the details of the Docker image creation.

### Ansible for configuration

{: style="text-align: justify" }

Ansible is the flexible automation solution tool for the network configuration management in the CI/CD pipeline. It supports various network hardware/software vendors and provides modules for example for Cisco IOS (usable for IOS-XE) and NX-OS software which I will use in my case. I will create an Ansible playbook which will make sure that the interfaces for OSPF (Open Shortest Path First) routing in my network will be configured based on YAML (Yet Another Markup Language) files containing the intended network configuration. I will explain the details of the pre and post network configuration in the fifth part of the blog post series.

### pyATS for validation

{: style="text-align: justify" }

Pre and post validation of network changes and the network state are key in an automated frameworks like CI/CD pipelines. pyATS (Python Automated Test Systems) is a Python based test automation infrastructure. In part six of the blog post series I will show you how to use pyATS ability to connect to the lab devices, defined in a testbed file, and use the build-in **parse** and **learn** functions to evaluate the network changes and network state prior and after the Ansible configuration playbook run. Then I will compare the outputs and print a diff into the logs.

### GitLab CI/CD pipeline

## Preparation

{: style="text-align: justify" }

After explaining all the components of the framework we need a machine to work on. For this I will use a virtual machine on ESXi (7.0 Update 3) with a [CentOS 9 Stream](https://www.centos.org/download/){:target="_blank"} installation. The virtual machine consists 4vCPUs, 16 GB RAM, and 64 GB disk space. It is placed on the same virtual network subnet as the Cisco Modeling Labs server.

![CentOS VM settings](/images/cicd_vm.png "CentOS VM settings")
*Screenshot 1: CentOS 9 VM settings*

packages to be installed:

- docker
- docker compose
- make

Docker installation

## Links
