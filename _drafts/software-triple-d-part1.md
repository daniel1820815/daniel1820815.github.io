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

Please keep in mind that this blog post series is a summarization from the resources I am using for my DevNet Expert lab exam preparation including my personal view on the topics. It does not mean that the topics are fully covered or that there are no other good resources which could be taken into account. Let us start and tackle the major bullet point 1.1 which is about designing a solution.

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

### Maintainable & Modular

## High available & Resilient

## Performance

## Monitor, Observe & Measure the Metrics

## Maintainability through Design

saves you time and resources

Modular design, makes it easier to replace components
Naming conventions to increase observability and standardize
software configuration management (using Git to track and control changes)
Coding standards increased consistency
common toolset to enable more developers and technologies
object oriented design for looser coupling and independent classes

SOLID design principle

The main purpose of the SOLID design principle is to create maintainable and extendable software.

1. Single responsibility principle

    Goes along with modular design that you have a single functionality and responsibility for example in a class.

2. Open-closed principle

    Your components should be open for extensions but closed for modifications which means for example be open for other vendors within your code but closed for modifying out of standards for vendor specific modifications which could potentially break your code.

3. Liskov's subsitution principle

    If you extend your code you do not want to change the base types, it should return the same types

4. Interface segregation principle

    Your code should be written in a way that someone else could easily use it without to depend on interfaces they do not use

5. Dependency inversion principle

    always program towards an interface not an implementation. Implementation can come and go but interfaces stay

## Maintainability through Implementation

During the design phase, the requirements were gathered and major decisions should have been made. But as within every project the decisions can be revised based on observations during the implementation phase.

consistent code within your implementation team is important and helps a lot

use common tools, languages and techniques, to achieve consistency for example using linters to analyze your code and style guides, avoid DRY principle and create reusable code

technical documentation, version control, and be aware bus factor

All reduced maintenance times through a good implementation

## Modularity

divide an application int several separate modules

smaller modules are easier to understand and take less effort to fix

as mentioned before it creates more flexibility and extendability. your code becomes more reusable and it is easier to build new modules 

- Composability <-> Decomposability

  It is better to have High cohesity & loose coupling instead of low cohesity & tight coupling. reduce the amount of communication between modules
  keep the number of functionalities per module at a minimum it will also help to easier understand the modules which takes us to ...

- Understandability
- Continuity
- Protection

example of Moduluarity in microservers

example of pizza ordering application

- UI
- Auth
- Payment (3rd Party)
- orders
- menu
- reviews

each microservice has a single functionality
communication through API
reusable microservices

### Links & Sources

- [Developing Applications Using Cisco Core Platforms and APIs (DEVCOR 1.0) on CiscoU.](https://u.cisco.com/path/14){:target="_blank"}

- [Cisco Certified DevNet Professional (DEVCOR 350-901) Official Cert Guide](https://www.ciscopress.com/store/cisco-certified-devnet-professional-devcor-350-901-9780137370443){:target="_blank"}
