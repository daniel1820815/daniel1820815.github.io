---
layout: post
title: "Software Triple D - Episode I"
date: 2023-07-16 12:00:00 +0200
categories: Software
#comments_id: # CREATE AND ADD ISSUE NO.
---

#### DevNet Expert Series - Software Triple D - Episode I

{: style="text-align: justify" }

This blog post series is about the software triple D which is better know as **Software Design, Development, and Deployment** from the [Cisco DevNet Expert lab exam topics](https://learningnetwork.cisco.com/s/devnet-expert-exam-topics-lab){:target="_blank"}. This topic is already covered very well in the Cisco DevNet Professional core exam which is the [350-901 DEVCOR: Developing Applications Using Cisco Core Platforms and APIs](https://learningnetwork.cisco.com/s/devcor-exam-topics){:target="_blank"}. All parts of section 1.0 from the DEVCOR exam topics are about **Software Development and Design** and it is a really good foundation to build on top of it for the lab exam. The 350-901 DEVCOR exam is currently the prerequisites to book the lab exam. In this two part blog posts I will cover section 1.1 including its subsections:

{: style="text-align: justify" }

- 1.1 Design a solution based on an on-premises, hybrid, or public cloud deployment, considering these factors

  - 1.1.a Deployment: maintainability, modularity (e.g., containers, VM, orchestration, automation, components, and infrastructure requirements)

  - 1.1.b Reliability: high availability and resiliency

  - 1.1.c Performance: scalability, latency, and rate limiting

  - 1.1.d Infrastructure: monitoring, observability, and metrics (e.g., instrument placement and instrument deployment)

{: style="text-align: justify" }

Please keep in mind that this blog posts are just a summarization from the resources I am using for my DevNet Expert lab exam preparation including my personal view on the topics. It does not mean that the topics are fully covered or that there are no other good resources which could be taken into account. In the first part I will take a look at 1.1.a and 1.1.b including a general view on the topic. Let us start and tackle the major bullet point 1.1 which is about designing a solution.

## Designing a Solution

{: style="text-align: justify" }

When designing a software solution you have to consider different factors like maintainability, modularity, high availability, resiliency, scalability, latency, rate limiting, monitoring, observability, and various metrics. Based on these factors your design should fit into one of the deployment scenarios: on-premises, hybrid, or public cloud deployment. But let us first look at the factors themselves before looking at those deployment. The factors are the design requirements and constraints. I will use a simple example throughout this blog post series which is a small pizza ordering application described as follows:

- The user can access the application through a web GUI.
- The user can view the menu.
- The user can view reviews of previous orders.
- The user shall login/register before creating an order.
- The payment shall be handled by a provider (3rd party)

From a quick design point of view, the application should have six components:

![Pizza Ordering App](/images/pizza-app.png "Pizza Ordering App")

**Diagram 1:** *Pizza Ordering Application*

{: style="text-align: justify" }

Not really anything exciting or spectacularly new, but it should be easy as an example. What I have described above the diagram are so called ***functional requirements***, also well known as use cases or as user stories if you are familiar with the Agile development approach. They specify what the application should do and how it should work. Functional requirements can easily be measured, are they fulfilled or not?

{: style="text-align: justify" }

On the other hand there are ***nonfunctional requirements***, also called quality attributes which specify how the application should fulfill the functional requirements. As you can see from the list below, you will find some of the terms from exam topics:

- Performance
- Security
- Availability
- Resiliency
- Modifiability
- Reliability
- Usability
- And many more...

{: style="text-align: justify" }

Nonfunctional requirements are closely connected and affect each other, so it is a good idea to take them into consideration as a group and not as single requirements. If you need to decide which nonfunctional requirements need to be prioritized, it is always helpful to gather business, administrative, user, and system requirements (functional requirements) first. With the right nonfunctional requirements in mind, maintenance for example will be much much easier when prioritizing observability and modularity in the beginning of the application development. It is a good practice to avoid technical debt and the potential cost of reworking or rebuilding a system. This will happen if you have chosen the fast and easy way during designing without thinking about nonfunctional requirements. It will guide you to the dark side.

![Dark side](/images/dark-side.jpg "Dark side")

{: style="text-align: justify" }

Keep in mind that dependant on specific cases and functional requirements you have to focus on different nonfunctional requirements. For example for our small-scale app like the pizza ordering application without any plans to grow fast (for now), you do not need to take scalability and extensibility much into account, but put usability and performance on top of your prioritization list instead to have a good user experience in the beginning. Let us now dive into the subsections and explore the nonfunctional requirements in detail. We will start with subsection 1.1.a Deployment: maintainability, modularity.

## A Maintainable & Modular Deployment

{: style="text-align: justify" }

Why should we create our small-scale pizza ordering app maintainable? The answer is simple: Cost! It is about money and maintenance is cost. Imagine we need to implement missing functionalities like extending the menu but the code for that part is bad written and not documented at all, it would take hours or even days to add more dishes to the menu. Another thing is fixing bugs which could be even harder. It is all cost and maintainability saves you time and resources. Maintainability could be achieved using different best practices.

{: style="text-align: justify" }

Using **coding standards** increase consistency in your code and a consistent code within your implementation team can help a lot to avoid high maintenance times. Your team should use **common tool sets**. It means the usage of common development tools, programming languages and techniques which helps to achieve consistency. For example when using [Python](https://www.python.org) as programming language use linters to analyze your code and style guides to improve consistency and code quality. Try to avoid the DRY (Don't repeat yourself) principle and create reusable code. Here is a very good resource how to achieve [Python Code Quality](https://realpython.com/python-code-quality/) with tools and best practices.

{: style="text-align: justify" }

Another important thing are **naming conventions** in the code and in **technical documentation** to increase observability and standardize your application. A clean and structured documentation using a straight naming convention help additional developers to quickly jump into the project and have a smooth onboarding. The usage of a **software configuration management and version control** should be standard in the software development world. You need a tool to maintain your code, track changes and issues, and work collaborative on those topics.

{: style="text-align: justify" }

It is good to use an **object oriented design (OOD)** which makes it easier for others to follow. It brings again the benefit to easily add additional developers to your project. I would recommend to look at the SOLID design principle which main purpose is to make the object oriented design more understandable, maintainable, and extendable. The SOLID design principle consists five principles:

1. Single responsibility principle (SRP)
2. Open-closed principle (OCP)
3. Liskov's subsitution principle (LSP)
4. Interface segregation principle (ISP)
5. Dependency inversion principle (DIP)

{: style="text-align: justify" }

I will not go into the details for the SOLID design principle as it would go beyond the scope of this post. You can find a very good and detailed explanation including code examples in chapter 3 of the [Cisco Certified DevNet Professional (DEVCOR 350-901) Official Cert Guide](https://www.ciscopress.com/store/cisco-certified-devnet-professional-devcor-350-901-9780137370443){:target="_blank"}.

{: style="text-align: justify" }

Last but not least, maintainability goes hand in hand with modularity. A **Modular design**, makes it easier to make changes on a small parts of the application or even replace components without affecting the entire system. As we already saw on the diagram of our pizza ordering application, it is about dividing the application into several separate modules. Those smaller functional modules are easier to understand and take less effort to maintain which is our main goal here. As mentioned before it creates more flexibility and extendability. Your code becomes more reusable and it is easier to build new modules or replace modules.

{: style="text-align: justify" }

**Microservices** are the perfect solution for a modular design. They are loosely coupled and each microservice has a single functionality. The communication between the microservices through their specific interfaces is often realized by APIs. The concept of microservices helps to build reusable modules which are independent of the underlying hardware. It is possible to build and maintain microservices without taking care of other microservices which allows rapid and frequent deployments or releases.

{: style="text-align: justify" }

Bringing all of the mentioned best practices into our pizza ordering application will help us to achieve reduced maintenance times through a good design and implementation. We should be well prepared to go live or not? What happens if the server on which our application is running crashes? We have not thought about how to achieve a high available and resilient application. So let us take a look at section 1.1.b Reliability: high availability and resiliency.

## Built It High Available & Resilient

{: style="text-align: justify" }

What does it mean to build a high available and resilient pizza ordering application? It means that the application needs to be available for ordering pizza during the business hours and it should have no downtime until closing of business day. Pretty simple in our case, right? Of course there many other businesses which have higher requirements and need to be always online. Those business need to have much higher service level agreements (SLAs) than our pizza ordering application, but I want to keep it simple. You can find more about high availability requirements and service level agreements (SLAs) in chapter 2 of the [Cisco Certified DevNet Professional (DEVCOR 350-901) Official Cert Guide](https://www.ciscopress.com/store/cisco-certified-devnet-professional-devcor-350-901-9780137370443){:target="_blank"}.

{: style="text-align: justify" }

So what does it mean behind the scenes? It is again about money as you can imagine from terms like "available during business hours" or "no downtime during business hours". Time is money and therefore is about the time between failures and how many time does it take to repair these failures. There are three main topics you should address to build a high available and resilient application, because you want to detect any failures, prevent any failures, and recover from any failure as fast as possible.

{: style="text-align: justify" }

**Detection** is the one important thing you want to have in place. If there is no proper *monitoring*, *self-testing*, or *self-monitoring* in place you will not detect failures in time. We always need to know if our pizza ordering application is available and ready to take orders. Based on the systems you are running in your environment, there are also mechanisms like *heartbeat* or *hello packets* to quickly detect when a cluster member goes down or is not reachable anymore for any reason. *Simple ping* or *ICMP echo request/reply* checks are also common methods of monitoring systems to measure if a system is still alive.

![Force](/images/force.jpg "Force")

{: style="text-align: justify" }

Let us now assume there is a failure on a component in our pizza ordering application and a potential danger that the application goes down during business hours. Then the **recovery** plays the next import role for high availability and resiliency. To ensure a fast recovery from a failure it is always recommended to build a level of *redundancy* and remove any *single point of failure* in your application design. The best level is a redundant system in *active mode*, also known as *hot standby*, syncing the state and taking over in case of missing heartbeats. Then there is the *passive* or *warm standby* mode, which does not sync the state constantly. In case of a failure the passive or standby system takes over but it needs to learn the proper state before it is fully functional. A very basic redundancy method is the *spare* or *cold standby* mode which means it is not operational or not even powered on. This mode requires a manual process to bring the spare system online and is obviously the worst solution at least for our pizza ordering application when there is a potential danger to go down during business hours.

{: style="text-align: justify" }

Other types are *retries* and *timeouts* which are working together closely in redundancy methods for example when in comes to the point to declare a cluster member as down. If there is a software bug found in the software you are using which causes a failure there is also the option of an *software or system upgrade* to a stable version to recover your application. Same goes for a *rollback* from a change made which ended in a failure.

{: style="text-align: justify" }

There are a lot of things we could do to recover our systems from failure but would it be even better to prevent from failures or downtimes at all? So let us talk about the **prevention**. Sometimes it is helpful to isolate a system from the application in case of failure to get the opportunity for a deeper analysis and learn from the failure to build even better prevention or recovery. Unfortunately, based on your high availability deployment, it is not possible to remove a faulty system without an impact to the whole application functionality. As mentioned before, monitoring is a important part of detection, but there is also the *predictive analysis* using *historic data* like *log messages* or *telemetry data*. The data is the new gold and with that data you could determine the normal behavior of your systems and compare it with the states *before*, *during*, and *after a failure*. Out of it you are able to improve your mechanisms of detecting, recovering, and preventing furthermore. Needless to say that *automation* is a great way to achieve code consistency as mentioned before during design and deployment, and it can help to eliminate potential human errors as prevention method.

![Droids](/images/droids.jpg "Droids")

{: style="text-align: justify" }

When planning and creating a high availability design you need to consider three main aspects: **high availability**, **continuous operations**, and **disaster recovery**. All three aspects are part of the *business continuity plan (BCP)* and obviously it contains the *disaster recovery plan (DRP)*. It depends on the business requirements of the organization how those plans need to be designed. There are many dependencies based on the business operations and therefore how to run the IT operations including the application landscape. Based on those requirements and dependencies you can design a sufficient business continuity plan (BCP) including a appropriate disaster recovery plan (DRP).

{: style="text-align: justify" }

The decision which deployment model for our high available and resilient pizza ordering application is the right one depends on several factors as we learned. Clustering and server load balancing are important in a high availability deployment. You do not need to forget about data backup and replication, because high availability and redundancy can not help if data is lost or replications are not working properly. Imagine the order history or the user reviews are lost in our pizza ordering application. It is like you start from beginning without statistics and feedback from previous orders.

![High Availability Deployment Models](/images/ha-deployment-models.png "High Availability Deployment Models")

**Diagram 2:** *High Availability Deployment Models*

{: style="text-align: justify" }

No matter which design you create and which model would best suit the requirements and dependencies, in the end the decision is very dependent on the costs. This can then certainly weaken the previously declared requirements and, where possible, also lead to compromises in certain areas of the design. In the end, you have to decide for an **on-premises**, **cloud**, or **hybrid deployment model** having different advantages and disadvantages. The most complex and difficult model to manage and maintain is for sure the hybrid deployment. You choose the good and the bad from both worlds. Dependent on your on-premises deployment it could end up in a complex design and configuration. The on-premises model will give full control and the ability to customize your deployment on the one hand, but can be costly and not easily scalable on the other hand if growth is planned or quickly needed.

{: style="text-align: justify" }

In case of our pizza ordering application the on-premises deployment could be a good starting point if there is a proper infrastructure already available that meets the requirements in the beginning. That would be a cost efficient way to begin with. For further growth we should design and build the application to be portable to a cloud deployment model and take all the advantages. A cloud deployment can provide *scale and performance*, *low maintenance*, *low cost* (it depends), *mobility and easy access*, and *high availability* and *disaster recovery* if your application is designed to leverage those features. The bad part of it is the *limited functionality* by the cloud provider, *security* topics, and *technical support* if needed. As said often enough, it all depends on the application design and the dependencies and requirements and then plays a larger or smaller role in your deployment.

## Conclusion

{: style="text-align: justify" }

So far so good. Thank you very much for reading until here. I hope you liked it. In this posts I covered section *1.1 Design a solution based on an on-premises, hybrid, or public cloud deployment*, considering these factors from subsections *1.1.a Deployment: maintainability, modularity* and *1.1.b Reliability: high availability and resiliency*. We learned that designing an application is not pretty straight forward. You have to consider requirements and constraints which have connections in different subject areas and are partly dependent on each other or with each other. We will see that a high available and resilient solution has its trade-offs when I will continue with sections *1.1.c Performance: scalability, latency, and rate limiting* and *1.1.d Infrastructure: monitoring, observability, and metrics* in the second part. I hope you will be with me again then. Please leave a comment here or on one of Social Media posts about this post. Take care and may the force be with you!

### Links & Sources

- [Developing Applications Using Cisco Core Platforms and APIs (DEVCOR 1.0) on Cisco U.](https://u.cisco.com/path/14){:target="_blank"}

- [Cisco Certified DevNet Professional (DEVCOR 350-901) Official Cert Guide on Cisco Press](https://www.ciscopress.com/store/cisco-certified-devnet-professional-devcor-350-901-9780137370443){:target="_blank"}

- [DevNet Expert Training provided by Andreas Bækdahl](https://www.devnetexperttraining.com/masterclass){:target="_blank"}
