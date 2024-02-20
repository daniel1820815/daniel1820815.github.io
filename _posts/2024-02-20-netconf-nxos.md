---
layout: post
title: "NETCONF XML payload with YANG models"
date: 2024-02-16 09:00:00 +0200
categories: [Automation, Python]
comments_id: 21
---

{: style="text-align: justify" }
In this blog post I would like to explain how to create proper XML payload using the native YANG models for IOS-XE and the OpenConfig YANG models for NX-OS to send configuration data to network devices with NETCONF. At this point you will ask yourself why should I use NETCONF with creepy XML payload and complicated YANG models to configure my network devices when there are many other easier ways to do it? You might be right but first of all, let's briefly describe NETCONF with the a little help from [RFC6241](https://datatracker.ietf.org/doc/html/rfc6241){:target="_blank"} and then take a look at the one big advantage.

{: style="text-align: justify" }
The Network Configuration Protocol (NETCONF) provides mechanisms to install, manipulate, and delete the configuration of network devices. It uses XML-based (Extensible Markup Language) data encoding for the configuration data and the protocol messages. A secure, connection-oriented session is established using remote procedure calls (RPC) between a client which is your workstation for example and a server which is the network device. The NETCONF protocol provides a set of operations to manage device configurations (get-config, edit-config, copy-config, delete-config) and retrieve device state data (get). As the CLI was made for humans interacting with the devices, NETCONF was made for machines interacting with machines. For more detailed information about NETCONF I encourage you to read through [RFC6241](https://datatracker.ietf.org/doc/html/rfc6241){:target="_blank"} which is well written with good examples.

{: style="text-align: justify" }
A big advantage of using NETCONF to manage your network device configuration is the transactional behavior. Let's assume you're going to configure a network device manually with a prepared configuration part in a text editor to be copied and pasted into the CLI like we all were doing in the past. When there is an error in your configuration while pasting it to the CLI of the device you will end up with a partial configuration which could case serious issues. NETCONF's transactional behavior ensures that the configuration will only be applied when every bit of it is correct. As soon as there's a wrong bit which is not accepted by the device, the whole configuration you wanted to be applied is refused. This mechanism provides a robust and resilient way to configure devices.

ADD SCREENSHOT of FAILURE

{: style="text-align: justify" }
As described before, in this blog post I will focus only on a small part of NETCONF which is the creation of proper XML configuration data to be used with the *edit-config* operation to push the configuration to the device. I will take advantage of the [Python library ncclient](https://ncclient.readthedocs.io/en/latest/){:target="_blank"} for the NETCONF communication between the client (my workstation) and the server (the network devices). The following diagram shows the lab setup in Cisco Modeling Labs I'm using for the example.

ADD LINK TO LAB

ADD DIAGRAM

The XML configuration data, also named payload, will be created with the help of [YANG suite](https://developer.cisco.com/yangsuite/){:target="_blank"}. It provides a set of tools and plugins to learn, test, and adopt YANG programmable interfaces such as NETCONF, RESTCONF, gNMI and more. Let's take a closer look how to create the XML payload in YANG suite.

### YANG suite

- Prepare Yang suite
- Build payload with Yang suite
- Use payload with ncclient

### XML config for IOS-XE with native YANG model

### XML config for NX-OS with OpenConfig YANG model

### Pushing the configs using ncclient

Assume the folliwing configuration in place
features for bgp

- Preparing For Installation (Download / Upload)
- Installation

not need from version x.z on
