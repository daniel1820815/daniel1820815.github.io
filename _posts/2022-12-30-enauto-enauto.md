---
layout: post
title:  "Passed the Cisco ENAUTO exam"
date:   2021-12-30 11:33:00 +0100
categories: Automation
---

About four weeks ago I passed the Cisco ENAUTO (Automating and Programming Cisco Enterprise Solutions) exam. I have put together all materials which I used during my studies and would like to share them in a post together with some study tips. I appreciate any additional resources and would like to encourage you to add your recommendations in the comments.

As it is described on the [Cisco Learning Network](https://learningnetwork.cisco.com/s){:target="_blank"} platform: “The Automating Cisco Enterprise Solutions v1.0 (ENAUTO 300-435) exam is a 90-minute exam associated with the CCNP Enterprise, Cisco Certified DevNet Professional, and Cisco Certified DevNet Specialist – Enterprise Automation and Programmability certifications.” That means that it is an exam for double usage and therefore a win-win situation for us. The exam counts as concentration exam for the CCNP Enterprise and for the Cisco Certified DevNet Professional as well.

## Exam topics

As for any Cisco certification you will also find all [exam topics for ENAUTO](https://learningnetwork.cisco.com/s/enauto-exam-topics){:target="_blank"} on the Cisco Learning Network platform. This so called blueprint should always be your playbook during preparing for a Cisco exam. The exam topics are general guidelines for the content likely to be present on the exam. The exam topics are diverted into six sections and we will take a closer look at each one.

The first section is called “Network Programmability Foundation” and reflects 10% out of the overall topics. It is all about foundational knowledge of Git, API’s (REST & RPC), Python, and Ansible & Puppet for IOSXE platforms. You should really know version control operations with git, understand REST API’s, and know Python data types, functions, classes, conditions, and looping, and of course how to use it in practice.

The second section is called “Automate APIs and Protocols” and it also reflects with 10% only a small portion of the exam topics. This section is all about YANG data models with JSON/XML encodings and the functionalities and benefits of RESTCONF / NETCONF protocols. Make sure you know the different types of YANG models and how they are build with its components like leafs, leaf lists, lists, containers, etc.

“Network Device Programmability” is the third section and is the first one out of four sections with 20% exam topic coverage which means double attention compared to the first two sections. Make sure you know how to use Netmiko, Python request library using RESTCONF, ncclient with NETCONF, and basic Ansible operations, all on Cisco IOS XE devices. Check how to use model driven telemetry and be familiar with publication and subscription models. The different Day 0 provisioning methods should be know in theory.

The next section name “Cisco DNA Center” stands for itself. It is all about the the features and capabilities of Cisco DNA Center and its APIs. Make sure you know Network assurance and Intent APIs, and how to integrate 3rd party devices with Multivendor support (3rd party SDKs). Learn how events and notifications are used on Cisco DNA Center and know how to implement event outbound webhooks. Be familiar with API requests for network management tasks on the Intent API, Command Runner API, Site API, Network discovery and device API, and Template API. Also train yourself in troubleshooting Cisco DNA Center automation processes using the Intent API.

“Cisco SD-WAN” is the name of the fifth section and it is all about features and capabilities of Cisco SD-WAN vManage. You need to know how to construct and implement API requests with Python scripts for the Cisco SD-WAN vManage Device Inventory API, Administration API, Configuration API, and Monitoring API. Use these scripts to retrieve and display data or troubleshoot the Cisco SD-WAN deployment for example.

The last section number six is called “Cisco Meraki” and is all about the Cisco Meraki Dashboard API. Make sure you know the features and capabilities of Cisco Meraki with its Location Scanning API, MV Sense API, External Captive Portal API, and WebHook Alert API. You should be able to create and configure a network using Cisco Meraki APIs and how to implement Cisco Meraki Alert WebHooks using a Python script.

In summary, besides the theory parts of the exam topics there are really a lot of practical skills needed to master the exam. Let’s take a look on the study materials I used to prepare for it.

## Courses

My basic learning resource was the Implementing Automation for Cisco Enterprise Solutions (ENAUI) v1.2 course available on the Cisco Learning Network Store. It covers most of the exam topics and teaches you how to implement Cisco Enterprise automated solutions, including programming concepts, orchestration, telemetry, and automation tools. There are some good lab sections included especially for the Cisco product specific sections like Cisco DNA Center, Cisco SDWAN, and Cisco Meraki. As there is currently no book available for the exam preparation like the Official Certification Guide series on Cisco Press, I would highly recommend this course as a basic learning resource. You can use a free trial version to take a first look before purchasing.

On the Cisco Learning Network you will also find a section called ENAUTO Study Materials which is designed to help you quickly find what you are looking for by organizing the content according to the exam topics. There are only five out of six of the exam topic sections covered, the Cisco SD-WAN section is missing, but apart from that the study materials are linked to suitable Cisco DevNet learning labs and modules.

There are also a couple of courses available from several training providers. Personally I have no experiences with any of those courses, but I heard only good feedback from Nick Russo’s Cisco ENAUTO (300-435) course available on Pluralsight. There is also a study plan for ENAUTO and many more available on Nick’s homepage.

## Books

As stated before there is currently no specific book available for the exam preparation, but I would recommend to take a look at the [Book Recommendations in my previous post](https://blog.kuhlcloud.de/2021/11/all-hands-and-eyes-on-network-automation/){:target="_blank"}. I would say that every single book of those can provide something valuable to gain the knowledge needed to pass the exam. Please provide any additional book recommendations in the comment section below.

## Labs

After reviewing the exam topics we already came to the conclusion that this exam is very practice orientated. Therefore it is essential to do a lot of practice in labs and luckily there are a lot of free resources available on Cisco’s Developers Network platform.

The first and probably one of the most important lab resource is the IOS XE on CSR Latest Code with ZTP functionality sandbox. Together with the following three learning labs and the additional resources you have everything to dive into IOS XE programmability:

- Introduction to Model Driven Programmability (ex: NETCONF/YANG)” Learning Module
- Introduction to Guest Shell on IOS XE
- Introduction to Ansible for IOS XE Configuration Management
- IOS XE on DevNet
- Model Driven Programmability on DevNet
- Network Plug and Play on DevNet
- Zero Touch Provisioning on DevNet
- Python for Network Automation on DevNet

The next lab resource is the Model Driven Telemetry sandbox and the learning module IOS XE Model Driven Telemetry as add-on.

The Cisco DNA Center Always-On 2.2 sandbox is a great resource to learn the exam topics for the “Cisco DNA Center” section. There is no reservation needed and all additional information can be found on the Cisco DNA Center subpage.

For the “Cisco SD-WAN” section there is the Cisco SD-WAN 19.2 Always On sandbox available to dive into the topic. The SD-WAN API subpage provides more information and documentation. This lab requires also no reservation.

The Cisco Meraki subpage provides all information about the Meraki API. Using the Meraki Always-On sandbox together with the Getting Started With Meraki learning lab will help you to gain good hands-on experience.

## Closing notes

I did put many focus on lab practice for sections three to six because these sections cover about 80% of the exam topics. But don’t forget about understanding the theory first before start with the labs. A good foundational theory knowledge is the key to success and then evolve your knowledge through practice with labs. As I earned the Cisco DevNet Associate certification earlier this year I already had a good basic knowledge in many topics and it was easy to build on top of that and to evolve.

I hope you like all the information I put together and it helps further candidates to master the ENAUTO exam. Please leave comments or put additional study materials and resources to the comments. Thank you for reading!
