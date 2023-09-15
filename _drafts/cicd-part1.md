---
layout: post
title: "CI/CD pipeline I - Intro"
date: 2023-09-14 12:00:00 +0200
categories: [Containers, Python]
#comments_id: # CREATE AND ADD ISSUE NO.
---

#### CI/CD pipeline part 1 - Introduction

{: style="text-align: justify" }

In this SEVEN part blog post series I will go through the process of building a GitLab CI/CD pipeline for automated network configuration changes including pre and post validation. The pipeline will consist of several components which will be prepared separately and be brought together in the end to a complete automated framework.

{: style="text-align: justify" }

The idea and inspiration for this blog post series comes from the videos from Adrian Iliesiu available on the YouTube and the corresponding Github repository. All my work is mostly based on his resources, but the implementation slightly differs based on my own ideas, lab setup, software versions, and other things. Nevertheless a big thank you to Adrian for this great work and inspiration.

## Preparation

CentOS 9 Stream

Docker installation

## Components

### GitLab Docker container

The basis of the automated framework is GitLab. It will run in Docker containers and it consists of two parts. First, there is GitLab CE (community edition) which will be used as version control system for the code and second there is a GitLab runner registered to GitLab. The GitLab runner is the agent that is responsible for all of the GitLab CI/CD features like running the pipeline jobs.

### CML lab using Terraform

### Docker image creation

### Ansible for configuration

### pyATS for validation

### GitLab CI/CD pipeline

## Links
