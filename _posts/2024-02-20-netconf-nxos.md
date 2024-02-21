---
layout: post
title: "NETCONF XML payload with YANG models"
date: 2024-02-16 09:00:00 +0200
categories: [Automation, Python]
comments_id: 21
---

{: style="text-align: justify" }
In this blog post I would like to explain how to create proper XML payload using the Native YANG model and the OpenConfig YANG model for IOS-XE and NX-OS to send configuration data to the devices with NETCONF. At this point you will ask yourself why should I use NETCONF with creepy XML payload and complicated YANG models to configure my network devices when there are many other easier ways to do it? You might be right but first of all, let's briefly describe NETCONF with the a little help from [RFC6241](https://datatracker.ietf.org/doc/html/rfc6241){:target="_blank"} and then take a look at the one big advantage.

{: style="text-align: justify" }
The Network Configuration Protocol (NETCONF) provides mechanisms to install, manipulate, and delete the configuration of network devices. It uses XML-based (Extensible Markup Language) data encoding for the configuration data and the protocol messages. A secure, connection-oriented session is established using remote procedure calls (RPC) between a client which is your workstation for example and a server which is the network device. The NETCONF protocol provides a set of operations to manage device configurations (get-config, edit-config, copy-config, delete-config) and retrieve device state data (get). As the CLI was made for humans interacting with the devices, NETCONF was made for machines interacting with machines. For more detailed information about NETCONF I encourage you to read through [RFC6241](https://datatracker.ietf.org/doc/html/rfc6241){:target="_blank"} which is well written with good examples.

{: style="text-align: justify" }
A big advantage of using NETCONF to manage your network device configuration is the transactional behavior. Let's assume you're going to configure a network device manually with a prepared configuration part in a text editor to be copied and pasted into the CLI like we all were doing in the past. When there is an error in your configuration while pasting it to the CLI of the device you will end up with a partial configuration which could case serious issues.

ADD SCREENSHOT of FAILURE

```cli
Nexus1# conf t
Enter configuration commands, one per line. End with CNTL/Z.
Nexus1(config)# int ethernet1/10
Nexus1(config-if)# ip address 10.0.50.3/24
                        ^
% Invalid command at '^' marker.
Nexus1(config-if)#
```

*Example 1: Pasting commands from a text editor with error*
{: style="text-align: justify" }
NETCONF's transactional behavior ensures that the configuration will only be applied when every bit of it is correct. As soon as there's a wrong bit which is not accepted by the device, the whole configuration you wanted to be applied is refused. This mechanism provides a robust and resilient way to configure devices.

{: style="text-align: justify" }
As described before, in this blog post I will focus only on a small part of NETCONF which is the creation of proper XML configuration data to be used with the *edit-config* operation to push the configuration to the device. I will take advantage of the Python library [ncclient](https://ncclient.readthedocs.io/en/latest/){:target="_blank"} for the NETCONF communication between the client (my workstation) and the server (the network devices). The following diagram shows the lab setup in Cisco Modeling Labs I'm using for the example.

![NETCONF Lab overview](/images/netconf_lab_overview.png "NETCONF Lab overview")

*Screenshot 1: NETCONF Lab overview in Cisco Modeling Labs*
{: style="text-align: justify" }
For the initial lab setup please refer to my [DevNet Expert Lab on Cisco Modeling Labs](https://github.com/daniel1820815/devnet-expert-cml-lab){:target="_blank"} GitHub repository. There are only the management interfaces and some basic features configured, everything else like the interfaces and BGP configuration will be done via NETCONF.

Before communicating with the devices using NETCONF, the NETCONF Agent must be enabled. The NETCONF Agent is enabled or disabled by entering the ```netconf-yang``` command on IOS-XE and ```feature netconf``` command on NX-OS. Additionally you need to enable OpenConfig on NX-OS using the ```feature openconfig``` command. Make also sure you have a user with appropriate privileges configured on your devices and connectivity from your workstation via SSHv2 on TCP port 830 is possible. NETCONF does not support SSH version 1. That's all part of the initial lab configuration in my example.

### Intro to Cisco YANG Suite

{: style="text-align: justify" }
The XML configuration data, also named payload, will be created with the help of [Cisco YANG Suite](https://developer.cisco.com/yangsuite/){:target="_blank"}. It provides a set of tools and plugins to learn, test, and adopt YANG programmable interfaces such as NETCONF, RESTCONF, gNMI and more. I am using the DevNet Expert Candidate Workstation VM on which Cisco YANG Suite is already installed and is available on <http://localhost:8480>. For installation option please refer to the [Cisco YANG Suite documentation](https://developer.cisco.com/docs/yangsuite/){:target="_blank"}.

If it is your first time running YANG Suite, you should start at the [Getting Started](https://developer.cisco.com/docs/yangsuite/#!welcome-to-cisco-yang-suite/getting-started){:target="_blank"} section and create Device profiles and download the supported YANG models from the devices or upload YANG model files from your workstation or from public [YANG Github repository](https://github.com/YangModels/yang){:target="_blank"}. After that you can create YANG module sets for IOS-XE and NX-OS with the supported modules you need.

![YANG Module Sets](/images/netconf_yang_module_sets.png "YANG Module Sets")

Now we can take a closer look how to create the XML payload and use these models to send NETCONF at “Protocols –> NETCONF” in YANG Suite.

### Create XML Payload with Cisco YANG Suite

text

### Pushing the configs using ncclient

Assume the folliwing configuration in place
features for bgp

- Preparing For Installation (Download / Upload)
- Installation

not need from version x.z on
