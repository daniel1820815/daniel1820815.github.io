---
layout: post
title: "NETCONF XML payload with YANG models"
date: 2024-02-16 09:00:00 +0200
categories: [Automation, Python]
comments_id: 21
---

{: style="text-align: justify" }
In this blog post I would like to explain how to create proper XML payload using the native YANG models for IOS-XE and the OpenConfig YANG models for NX-OS to send configuration data to network devices with NETCONF. At this point you will ask yourself why should I use NETCONF with creepy XML payload and complicated YANG models to configure my network devices when there are many other easier ways to do it? You might be right but first of all, let's briefly describe NETCONF with the a little help from [RFC6241](https://datatracker.ietf.org/doc/html/rfc6241){:target="_blank"} and then take a look at the advantages.

The Network Configuration Protocol (NETCONF) provides mechanisms to install, manipulate, and delete the configuration of network devices. It uses XML-based (Extensible Markup Language) data encoding for the configuration data and the protocol messages.

remote procedure call (RPC) between a client which is your workstation for example and a server which is the network device. a secure, connection-oriented session



In the example I describe 

- Prepare Yang suite
- Build payload with Yang suite
- Use payload with ncclient

OpenConfig on NX-OS

- Preparing For Installation (Download / Upload)
- Installation

not need from version x.z on
