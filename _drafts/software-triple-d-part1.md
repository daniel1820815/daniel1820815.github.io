---
layout: post
title: "DevNet Expert series - Software triple D part 1"
date: 2023-07-16 12:00:00 +0200
categories: software
#comments_id: # CREATE AND ADD ISSUE NO.
---

I am talking about Software Design, Development, and Deployment

In this first blog post I will try to cover...

- 1.1 Design a solution based on an on-premises, hybrid, or public cloud deployment, considering these factors

  - 1.1.a Deployment: maintainability, modularity (e.g., containers, VM, orchestration, automation, components, and infrastructure requirements)

  - 1.1.b Reliability: high availability and resiliency

  - 1.1.c Performance: scalability, latency, and rate limiting

  - 1.1.d Infrastructure: monitoring, observability, and metrics (e.g., instrument placement and instrument deployment)

Functional requirements: What the software should do!

Nonfunctional requirements: How a system should perform the functional requirements

NOTE: Add Examples

Importance of non-functional requirements

make maintenance much easier when prioritizing observability and modularity in the beginning of the application development

avoid technical debt and the potential cost of reworking or rebuilding a system. This will happen if you have chosen the fast and easy way without thinking about nonfunctional requirements. It will guide you to the dark side.

Dependant on specific cases you have to focus on the right nonfunctional requirements. For example for small-scale apps which are expected to grow quickly, you need to take scalability and extensibility more into account and put on top of your prioritization list than other requirements.

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
