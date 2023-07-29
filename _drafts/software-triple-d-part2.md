---
layout: post
title: "Software Triple D - Episode II"
date: 2023-07-16 12:00:00 +0200
categories: software
#comments_id: # CREATE AND ADD ISSUE NO.
---

#### DevNet Expert Series - Software Triple D - Episode II

{: style="text-align: justify" }

This blog post series is about the software triple D which is better know as **Software Design, Development, and Deployment** from the [Cisco DevNet Expert lab exam topics](https://learningnetwork.cisco.com/s/devnet-expert-exam-topics-lab){:target="_blank"}. This topic is already covered very well in the Cisco DevNet Professional core exam which is the [350-901 DEVCOR: Developing Applications Using Cisco Core Platforms and APIs](https://learningnetwork.cisco.com/s/devcor-exam-topics){:target="_blank"}. All parts of section 1.0 from the DEVCOR exam topics are about **Software Development and Design** and it is a really good foundation to build on top of it for the lab exam. The 350-901 DEVCOR exam is currently the prerequisites to book the lab exam. In this two part blog posts I will cover section 1.1 including its subsections:

{: style="text-align: justify" }

- 1.1 Design a solution based on an on-premises, hybrid, or public cloud deployment, considering these factors

  - 1.1.a Deployment: maintainability, modularity (e.g., containers, VM, orchestration, automation, components, and infrastructure requirements)

  - 1.1.b Reliability: high availability and resiliency

  - 1.1.c Performance: scalability, latency, and rate limiting

  - 1.1.d Infrastructure: monitoring, observability, and metrics (e.g., instrument placement and instrument deployment)

{: style="text-align: justify" }

This is the second part of the two part blog post series. The major bullet point 1.1 including the subsections 1.1.a and 1.1.b. was covered in the first part [Software Triple D - Episode I](ADD LINK){:target="_blank"}. Now I will continue with subsection 1.1.c which is how to ensure **performance** considering *scalability*, *latency*, and *rate limiting*. Then I will close section 1.1 with subsection 1.1.d which is about **infrastructure** and how to take advantage of using *monitoring*, *observability*, and *metrics*. So let us start!

{: style="text-align: justify" }

> Please keep in mind that this blog posts are just my personal summarization from the resources I am using for my DevNet Expert lab exam preparation including my personal view on the topics. It does not mean that the topics are fully covered or that there are no other good resources which could be taken into account.

## All About Performance

The first impression from an application for a user is the performance. In first place the user does not get any idea about how maintainable, high available, resilient, or modular the application is built. The performance plays a big role for the first user experience. Is there a bad performance on our pizza ordering application because of slow response times of the menu for example then the user experience is bad too.

**Scalability**, which is closely linked with modularity, is another hidden aspect but it directly affects performance. Scalability is about any sizing up the application and that's much easier when it was built modular. There are two types of scalability, **horizontal** and **vertical scalability**.

Horizontal scalability, also mentioned as *Scaling out*, means adding additional resources to an application and distributing the load. From our example above for high availability deployment models we are simply adding another application server to the cluster to scale out from 2 to 3. The database servers were removed for simplicity. Load balancing, server load balancing or clustering is usually used for horizontal scaling.

![Horizontal Scalability](/images/horizontal-scalability.png "Horizontal Scalability")

**Diagram 2:** *Horizontal Scalability*

Vertical scalability on the other hand is about adding additional resources to a single physical or virtual server. It means *scaling up* the server with memory, storage, or whatever resource needs more power. Whatever way of scaling you choose or you need to choose based on new requirements you will quickly find out that designing your application as modular as possible helps a lot and makes it much easier to scale out or up. Keep in mind that nonfunctional requirements need to be measurable and scalability can be measured to determine the **performance**.

There are some important performance indicators like **latency**, **throughput**, and **round-trip time (RTT)** which can define a good or bad user experience, depending on how well they are managed.

latency in more details and describe the differences in networking and software for latency

TCP and UDP differences on performance parameters

other factors can affect those performance parameters for example
network issues along the path routers, switches, firewalls and many more components
hardware failures or less resource availability during a period of time
software issues or bugs
wrong or suboptimal network configurations for example routing
geographically distributed systems or users

there are three architecture trade offs and it is not easy to build low latency and high performance applications while fulfilling the different requirements and constraints we talked about
from business point of view the most important things is money and therefore the cost
availability and performance: high availability systems through building redundant components often affects latency
scalability and performance: more scalability often means distributing resources among multiple devices. these devices need to communicate with each other which can affect performance

rate limiting
meraki dashboard API has a rate limiting of X requests per user per second to avoid overloeading
control the amount of users accessing the pizza ordering application at the same time, for example the webserver can handle 100 requests per second

image rate limiting

examples for rate limits
number of requests
user actions
server-bound traffic
concurrent connections for user experience and security

parallel processing

exponential backoff

## Monitor, Observe & Measure the Metrics

### Links & Sources

- [Developing Applications Using Cisco Core Platforms and APIs (DEVCOR 1.0) on CiscoU.](https://u.cisco.com/path/14){:target="_blank"}

- [Cisco Certified DevNet Professional (DEVCOR 350-901) Official Cert Guide](https://www.ciscopress.com/store/cisco-certified-devnet-professional-devcor-350-901-9780137370443){:target="_blank"}
