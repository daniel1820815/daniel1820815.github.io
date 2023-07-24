---
layout: post
title: "DevNet Expert series - Software triple D part 1"
date: 2023-07-16 12:00:00 +0200
categories: Software
#comments_id: # CREATE AND ADD ISSUE NO.
---

This blog post series is about the software triple D which is better know as **Software Design, Development, and Deployment** from the [Cisco DevNet Expert lab exam topics](https://learningnetwork.cisco.com/s/devnet-expert-exam-topics-lab){:target="_blank"}. This topic is already covered very well on the Cisco DevNet Professional core exam which is the [350-901 DEVCOR: Developing Applications Using Cisco Core Platforms and APIs](https://learningnetwork.cisco.com/s/devcor-exam-topics){:target="_blank"}. All parts of section 1.0 from the DEVCOR exam topics are about **Software Development and Design** and it is a really a good foundation to build on top of it for the lab exam. The 350-901 DEVCOR exam is currently the prerequisites to book the lab exam. In this first blog post I will cover section 1.1 including its subsections:

- 1.1 Design a solution based on an on-premises, hybrid, or public cloud deployment, considering these factors

  - 1.1.a Deployment: maintainability, modularity (e.g., containers, VM, orchestration, automation, components, and infrastructure requirements)

  - 1.1.b Reliability: high availability and resiliency

  - 1.1.c Performance: scalability, latency, and rate limiting

  - 1.1.d Infrastructure: monitoring, observability, and metrics (e.g., instrument placement and instrument deployment)

Please keep in mind that this blog post series is just a summarization from the resources I am using for my DevNet Expert lab exam preparation including my personal view on the topics. It does not mean that the topics are fully covered or that there are no other good resources which could be taken into account. Let us start and tackle the major bullet point 1.1 which is about designing a solution.

## Designing a solution

When designing a software solution you have to consider different factors like maintainability, modularity, high availability, resiliency, scalability, latency, rate limiting, monitoring, observability, and various metrics. Based on these factors your design should fit into one of the deployment scenarios: on-premises, hybrid, or public cloud deployment. But let us first look at the factors themselves before looking at those deployment. The factors are the design requirements and constraints. I will use a simple example throughout this blog post series which is a small pizza ordering application described as follows:

- The user can access the application through a web GUI.
- The user can view the menu.
- The user can view reviews of previous orders.
- The user shall login/register before creating an order.
- The payment shall be handled by a provider (3rd party)

From a quick design point of view, the application should have six components:

![Pizza Ordering App](/images/pizza-app.png "Pizza Ordering App")

Not really anything exciting or spectacularly new, but it should be easy as an example. What I have described above the diagram are so called ***functional requirements***, also well known as use cases or as user stories if you are familiar with the Agile development approach. They specify what the application should do and how it should work. Functional requirements can easily be measured, are they fulfilled or not?

On the other hand there are ***nonfunctional requirements***, also called quality attributes which specify how the application should fulfill the functional requirements. As you can see from the list below, you will find some of the terms from exam topics:

- Performance
- Security
- Availability
- Resiliency
- Modifiability
- Reliability
- Usability
- And many more...

Nonfunctional requirements are closely connected and affect each other, so it is a good idea to take them into consideration as a group and not as single requirements. If you need to decide which nonfunctional requirements need to be prioritized, it is always helpful to gather business, administrative, user, and system requirements (functional requirements) first. With the right nonfunctional requirements in mind, maintenance for example will be much much easier when prioritizing observability and modularity in the beginning of the application development. It is a good practice to avoid technical debt and the potential cost of reworking or rebuilding a system. This will happen if you have chosen the fast and easy way during designing without thinking about nonfunctional requirements. It will guide you to the dark side.

![Dark side](/images/dark-side.jpg "Dark side")

Keep in mind that dependant on specific cases and functional requirements you have to focus on different nonfunctional requirements. For example for our small-scale app like the pizza ordering application without any plans to grow fast (for now), you do not need to take scalability and extensibility much into account, but put usability and performance on top of your prioritization list instead to have a good user experience in the beginning. Let us now dive into the subsections and explore the nonfunctional requirements in detail.

## A Maintainable & Modular Deployment

Why should we create our small-scale pizza ordering app maintainable? The answer is simple: Cost! It is about money and maintenance is cost. Imagine we need to implement missing functionalities like extending the menu but the code for that part is bad written and not documented at all, it would take hours or even days to add more dishes to the menu. Another thing is fixing bugs which could be even harder. It is all cost and maintainability saves you time and resources. Maintainability could be achieved using different best practices.

Using **coding standards** increase consistency in your code and a consistent code within your implementation team can help a lot to avoid high maintenance times. Your team should use **common tool sets**. It means the usage of common development tools, programming languages and techniques which helps to achieve consistency. For example when using [Python](https://www.python.org) as programming language use linters to analyze your code and style guides to improve consistency and code quality. Try to avoid the DRY (Don't repeat yourself) principle and create reusable code. Here is a very good resource how to achieve [Python Code Quality](https://realpython.com/python-code-quality/) with tools and best practices.

Another important thing are **naming conventions** in the code and in **technical documentation** to increase observability and standardize your application. A clean and structured documentation using a straight naming convention help additional developers to quickly jump into the project and have a smooth onboarding.

The usage of a **software configuration management and version control** should be standard in the software development world. You need a tool to maintain your code, track changes and issues, and work collaborative on those topics.

It is good to use an **object oriented design (OOD)** which makes it easier for others to follow. It brings again the benefit to easily add additional developers to your project. I would recommend to look at the SOLID design principle which main purpose is to make the object oriented design more understandable, maintainable, and extendable. The SOLID design principle consists five principles:

1. Single responsibility principle (SRP)
2. Open-closed principle (OCP)
3. Liskov's subsitution principle (LSP)
4. Interface segregation principle (ISP)
5. Dependency inversion principle (DIP)

I will not go into the details for the SOLID design principle as it would go beyond the scope of this post. You can find a very good and detailed explanation including code examples in the [Cisco Certified DevNet Professional (DEVCOR 350-901) Official Cert Guide](https://www.ciscopress.com/store/cisco-certified-devnet-professional-devcor-350-901-9780137370443){:target="_blank"} in chapter 3.

Last but not least, maintainability goes hand in hand with modularity. A **Modular design**, makes it easier to make changes on a small parts of the application or even replace components without affecting the entire system. As we already saw on the diagram of our pizza ordering application, it is about dividing the application into several separate modules. Those smaller functional modules are easier to understand and take less effort to maintain which is our main goal here. As mentioned before it creates more flexibility and extendability. Your code becomes more reusable and it is easier to build new modules or replace modules.

**Microservices** are the perfect solution for a modular design. They are loosely coupled and each microservice has a single functionality. The communication between the microservices through their specific interfaces is often realized by APIs. The concept of microservices helps to build reusable modules which are independent of the underlying hardware. It is possible to build and maintain microservices without taking care of other microservices which allows rapid and frequent deployments or releases.

Bringing all of the mentioned best practices into our pizza ordering application will help us to achieve reduced maintenance times through a good design and implementation. We should be well prepared to go live or not? What happens if the server on which our application is running crashes? We have not thought about how to achieve a high available and resilient application. So let us take a look at section 1.1.b Reliability: high availability and resiliency.

### High available & Resilient

how many time between failures and how many time to repair these failures
detect prevent and recover

detection
monitoring
self-testing or self-monitoring
heartbeat or 'hello packets'
simple ping or ICMP echo request/reply
sanity checks

recovery
redundancy to remove the single point of failure and ensure fast recovery
redundant system in active redundancy mode, also known as hot standby, syncing the state and taking over in case of missing heartbeats or in passive (warm standby) or spare (cold standby)
retries
timeouts
upgrade
rollback

prevention
isolation of system
predictive analysis, historic data available, telemetry, logs and monitoring, normal behavior, before, during, after a failure
automation is a great way to achieve code consistency as mentioned before during design and deployment, and can help to eliminate potential hunan errors

planning high availability
to create a high availability design you need to consider three main aspects: High availability, continuous operations, and disaster recovery. All three aspects are part of the the business continuity plan (BCP) and obviously it contains the disaster recovery plan (DRP). It depends on the business requirements of the organization how those plans need to be designed. There are many dependencies based on the business operations and therefore how to run the IT operations including the application landscape. Based on those requirements and dependencies you can design a sufficient business continuity plan (BCP) including a disaster recovery plan (DRP).

Deployment models for high availability

![High Availability Deployment Models](/images/ha-deployment-models.png "High Availability Deployment Models")

### Performance

### Monitor, Observe & Measure the Metrics

### Links & Sources

- [Developing Applications Using Cisco Core Platforms and APIs (DEVCOR 1.0) on CiscoU.](https://u.cisco.com/path/14){:target="_blank"}

- [Cisco Certified DevNet Professional (DEVCOR 350-901) Official Cert Guide](https://www.ciscopress.com/store/cisco-certified-devnet-professional-devcor-350-901-9780137370443){:target="_blank"}
