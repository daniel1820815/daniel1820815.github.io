---
layout: post
title: "DevNet Expert series - Docker part 4"
date: 2023-05-30 12:00:00 +0200
categories: Containers
#comments_id: # CREATE AND ADD ISSUE NO.
---

This is part four and therefore the final part of the DevNet Expert series about Containers using Docker and Kubernetes. It builds on top of the previous blog posts...

- [DevNet Expert series - Docker part 1 - Create a Docker image using Dockerfile](https://blog.kuhlcloud.de/containers/2022/12/16/docker-part1.html){:target="_blank"}
- [DevNet Expert series - Docker part 2 - Create, consume, and troubleshoot a Docker host and bridge-based networks and integrate them with external networks](https://blog.kuhlcloud.de/containers/2022/12/16/docker-part1.html){:target="_blank"}
- [DevNet Expert series - Docker part 3 - Package and deploy a solution by using Docker Compose](https://blog.kuhlcloud.de/containers/2023/02/xx/docker-part1.html){:target="_blank"}

You can find the source files for all previous blog posts on the [Github repository](https://github.com/daniel1820815/devnet-expert-lab/){:target="_blank"}. It is not a requirement to go through the previous posts necessarily.

During the first part we focused on section 4.1 from the [exam blueprint](https://learningnetwork.cisco.com/s/devnet-expert-exam-topics-lab){:target="_blank"} which is about creating a Docker image using Dockerfile. We ignored Docker networking during the first part and added that within the second part according to section 4.4. which is called "Create, consume, and troubleshoot a Docker host and bridge-based networks and integrate them with external networks". Now it is time to bring all together when using Docker Compose which covers section 4.2 "Package and deploy a solution by using Docker Compose" from the exam topics.

I am still using a simple lab setup in [Cisco Modeling Labs (CML)](https://developer.cisco.com/modeling-labs/){:target="_blank"} with a Ubuntu 20.04 machine as devbox running Docker and external connectivity. My lab topology file is available [here](https://github.com/daniel1820815/devnet-expert-lab/blob/main/blog/docker/){:target="_blank"} for download and import into CML. You could also use the official Candidate Workstation available for download on the [Cisco Learning Network](https://learningnetwork.cisco.com/s/article/devnet-expert-equipment-and-software-list){:target="_blank"}.

Hope you are still with me after two parts of the Containers series and follow my journey towards the Cisco Certified DevNet Expert. Let's start with Docker Compose!

### Part 4: Kubernetes
