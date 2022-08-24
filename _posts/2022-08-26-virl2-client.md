---
layout: post
title:  "Python vril2_client for Cisco Modeling Labs"
date:   2022-08-26 13:00:00 +0200
categories: Automation
---

Are you are using [Cisco Modeling Labs Personal or Enterprise](https://www.cisco.com/c/en/us/products/cloud-systems-management/modeling-labs/index.html){:target="_blank"} to build labs for your learning and certification studies or even testing various scenarios from production environments in a safe place? Then you will probably come to the point that you think about automating tasks to build labs using the CML API. Automating repeating tasks is key in todays IT world. It will save you some time and help to focus on the topics you want to do in your lab rather than spending hours with the lab setup.

I was recently playing around with CML while creating a new lab for testing the [Cisco pyATS framework](https://developer.cisco.com/docs/pyats/#!introduction/cisco-pyats-network-test--automation-solution){:target="_blank"}. I came very quickly to the point to use the official Python library [virl2_client](https://github.com/CiscoDevNet/virl2-client){:target="_blank"} for CML which provides a Python package to programmatically create, edit, delete, and control your network simulations on a CML controller. 

In my case I wanted to create a pyATS testbed automatically from a lab on the CML controller rather than creating it manually. The automatic process will safe me some time in the future and I will be able to focus on creating my test cases using pyATS. During this process I stumbled over an issue which had an obvious solution. Let's take a look.

## Install pyATS and virl2_client library

As I got a new laptop at this time I had not installed all the tools I used before and also did not pull all the repository I was working on. So I started to clone my private repository I was previously working on, created a new virtual environment, and installed pyATS using pip, the package installer for Python:

```python
(pyats-test) pip install pyats
```

> **Note:** There are different options how to install pyATS described in the pyATS documentation.

After that I installed the virl2-client library:

```python
(pyats-test) pip install virl2-client
```

Now I thought I could quickly create my testbed and start with testing. I used a small Python script which I copied from the blog post "[How can I automate device configurations using CML2?](https://blogs.cisco.com/developer/363-askhankcml2-01){:target="_blank"}" by Hank Preston which contains a very good introduction and explanation how to use CML and pyATS.

```python
from virl2_client import ClientLibrary

# Create a client object for interacting with CML
client = ClientLibrary("https://<YOUR-CML-IP/URL>", "<CML-USER", "CML-PASSWORD", ssl_verify=False)

# Find your lab. Method returns a list, this assumes the first lab returned is what you want
lab = client.find_labs_by_title("Multi Platform Network")[0]

# Retrieve the testbed for the lab 
pyats_testbed = lab.get_pyats_testbed()

# Write the YAML testbed out to a file
with open("lab_testbed.yaml", "w") as f: 
    f.write(pyats_testbed)
```

You have to fill in your CML data or even better use environment variables. Add the name of your lab, and specify the output file for the testbed. The script does the rest for you and with quite good comments self-explanatory. But this is the step where I faced into the issue after I ran the Python script:

```python
(pyats-test) $ python create_testbed.py 
SSL Verification disabled
Traceback (most recent call last):
  File "/Users/danielkuhl/Coding/pyats-test/create_testbed.py", line 4, in <module>
    client = ClientLibrary("https://<YOUR-CML-IP/URL>", "<CML-USER", "CML-PASSWORD", ssl_verify=False)
  File "/Users/danielkuhl/Coding/pyats-test/lib/python3.9/site-packages/virl2_client/virl2_client.py", line 281, in __init__
    self.check_controller_version()
  File "/Users/danielkuhl/Coding/pyats-test/lib/python3.9/site-packages/virl2_client/virl2_client.py", line 429, in check_controller_version
    raise InitializationError(
virl2_client.virl2_client.InitializationError: Controller version 2.2.3+build63 is marked incompatible! List of versions marked explicitly as incompatible: [2.0.0, 2.0.1, 2.1.0, 2.1.1, 2.1.2, 2.2.1, 2.2.2, 2.2.3].
```

The check of the controller version failed. The error told me that the virl2_client version I was using is incompatible with the controller version. At this time I was confused and not aware about the fact that the virl2_client version need to match with the controller version. That's why I thought I stumbled over a bug and too quickly created my first [Github issue](https://github.com/CiscoDevNet/virl2-client/issues/20){:target="_blank"} ever. 

I might have checked the error message more carefully and also double check the documentation. As you can see from the Github issue the developers of virl2_client responded very fast and mentioned that the error was expected with my controller version. Another lessons learned on the journey.

## Install the compatible virl2_client version

First I checked the virl2_client version and by default it installs the latest version which is 2.4.0 according to the latest CML controller version 2.4.0 released. My CML controller version is the recommended version 2.2.3 as of time of writing this article:

```python
(pyats-test) $ pip list | grep virl2
virl2-client                 2.4.0
```

With that I confirmed the version mismatch. Then uninstall virl2_client:

```python
(pyats-test) $ pip uninstall virl2_client
Found existing installation: virl2-client 2.4.0
Uninstalling virl2-client-2.4.0:
  Would remove:
    /Users/danielkuhl/Coding/pyats-test/lib/python3.9/site-packages/examples/demo.ipynb
    /Users/danielkuhl/Coding/pyats-test/lib/python3.9/site-packages/examples/licensing.py
    /Users/danielkuhl/Coding/pyats-test/lib/python3.9/site-packages/examples/link_conditioning.py
    /Users/danielkuhl/Coding/pyats-test/lib/python3.9/site-packages/examples/sample.py
    /Users/danielkuhl/Coding/pyats-test/lib/python3.9/site-packages/virl2_client-2.4.0.dist-info/*
    /Users/danielkuhl/Coding/pyats-test/lib/python3.9/site-packages/virl2_client/*
Proceed (Y/n)? Y
  Successfully uninstalled virl2-client-2.4.0
```

And re-install with specifying the latest version less than 2.3.0 which is 2.2.1.post2 which I checked on the documentation after the virl2-client developer pointed me to that:

```python
(pyats-test) $ pip install "virl2-client<2.3.0"
Collecting virl2-client<2.3.0
  Using cached virl2_client-2.2.1.post2-py3-none-any.whl (52 kB)
Requirement already satisfied: requests<3,>=2 in ./lib/python3.9/site-packages (from virl2-client<2.3.0) (2.28.1)
Requirement already satisfied: requests-toolbelt<0.10.0,>=0.9.1 in ./lib/python3.9/site-packages (from virl2-client<2.3.0) (0.9.1)
Requirement already satisfied: charset-normalizer<3,>=2 in ./lib/python3.9/site-packages (from requests<3,>=2->virl2-client<2.3.0) (2.1.0)
Requirement already satisfied: idna<4,>=2.5 in ./lib/python3.9/site-packages (from requests<3,>=2->virl2-client<2.3.0) (3.3)
Requirement already satisfied: certifi>=2017.4.17 in ./lib/python3.9/site-packages (from requests<3,>=2->virl2-client<2.3.0) (2022.6.15)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in ./lib/python3.9/site-packages (from requests<3,>=2->virl2-client<2.3.0) (1.26.11)
Installing collected packages: virl2-client
Successfully installed virl2-client-2.2.1.post2
```

Now I was back on the right path. Let's check if creating of the testbed was working now.

## Finally create the pyATS testbed

Run the Python script again:

```python
(pyats-test) $ python create_testbed.py
SSL Verification disabled
```

Validate testbed


```bash
(pyats-test) $ pyats validate testbed lab_testbed.yaml 
Loading testbed file: lab_testbed.yaml
--------------------------------------------------------------------------------

Testbed Name:
    LAB-TEST

Testbed Devices:
|-- host-01 [linux/server]
|   |-- eth0 ----------> l14
|   `-- eth1 ----------> l16
|-- rtr-edge [ios/iosv]
|   |-- GigabitEthernet0/0 ----------> l1
|   |-- GigabitEthernet0/1 ----------> l7
|   |-- GigabitEthernet0/2 ----------> l8
|   |-- GigabitEthernet0/3
|   `-- Loopback0 ----------> rtr-edge:Loopback0
|-- server-01 [linux/server]
|   |-- eth0 ----------> l15
|   `-- eth1 ----------> l17
|-- sw-acc-01 [ios/iosv]
|   |-- GigabitEthernet0/0 ----------> l5
|   |-- GigabitEthernet0/1 ----------> l12
|   |-- GigabitEthernet0/2 ----------> l16
|   |-- GigabitEthernet0/3
|   `-- Loopback0 ----------> sw-acc-01:Loopback0
|-- sw-acc-02 [ios/iosv]
|   |-- GigabitEthernet0/0 ----------> l6
|   |-- GigabitEthernet0/1 ----------> l13
|   |-- GigabitEthernet0/2 ----------> l17
|   |-- GigabitEthernet0/3
|   `-- Loopback0 ----------> sw-acc-02:Loopback0
|-- sw-core [ios/iosv]
|   |-- GigabitEthernet0/0 ----------> l2
|   |-- GigabitEthernet0/1 ----------> l8
|   |-- GigabitEthernet0/2 ----------> l9
|   |-- GigabitEthernet0/3 ----------> l10
|   `-- Loopback0 ----------> sw-core:Loopback0
|-- sw-dist-01 [ios/iosv]
|   |-- GigabitEthernet0/0 ----------> l3
|   |-- GigabitEthernet0/1 ----------> l9
|   |-- GigabitEthernet0/2 ----------> l11
|   |-- GigabitEthernet0/3 ----------> l12
|   `-- Loopback0 ----------> sw-dist-01:Loopback0
|-- sw-dist-02 [ios/iosv]
|   |-- GigabitEthernet0/0 ----------> l4
|   |-- GigabitEthernet0/1 ----------> l10
|   |-- GigabitEthernet0/2 ----------> l11
|   |-- GigabitEthernet0/3 ----------> l13
|   `-- Loopback0 ----------> sw-dist-02:Loopback0
`-- terminal_server [linux/linux]

YAML Lint Messages
------------------

Warning Messages
----------------
 - Device 'terminal_server' has no interface definitions
```

Well, this output looked much better than before. As you can see from the output all devices from the lab are listed there including the link labels besides the interfaces. Each device interfaces which contain the same link labels are connected with each other. This information gives you some additional opportunities for automating things. You could use link labels for configurations on both ends of devices which are connected or even drawing a full network topology to mention only some first ideas coming to my mind.

## Lessons learned

Hope you liked this small journey about a really good lessons-learned for me in regards to software version compatibility checks or reading the documentation before creating a Github issue. As I wrote it was the first time I created a public Github issue and it was good way to learn. I had to read about how to create a Github issue and what information is useful to put in. I can highly recommend to read through the section [About Github issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues){:target="_blank"} on Github. 

Below you will find all the links used in this article. Thank you so much for reading. Please feel free to leave a comment or get in contact with me on Social Media if not happened yet.

### Links & References

#### Cisco Modeling Labs

- [Product page](https://www.cisco.com/c/en/us/products/cloud-systems-management/modeling-labs/index.html){:target="_blank"}
- [Documentation](https://developer.cisco.com/docs/modeling-labs/){:target="_blank"}
- [How can I automate device configurations using CML2?](https://blogs.cisco.com/developer/363-askhankcml2-01){:target="_blank"}"

#### Github

- [virl2_client](https://github.com/CiscoDevNet/virl2-client){:target="_blank"}
- [My Github issue](https://github.com/CiscoDevNet/virl2-client/issues/20){:target="_blank"}
- [About Github issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues){:target="_blank"}

#### pyATS
- [Cisco pyATS: Network Test & Automation Solution](https://developer.cisco.com/docs/pyats/#!introduction/cisco-pyats-network-test--automation-solution){:target="_blank"}
