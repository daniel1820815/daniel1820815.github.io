---
layout: post
title:  "vril2-client for Cisco Modeling Labs"
date:   2022-08-14 20:00:00 +0200
categories: Automation
---

If you are using CML (Cisco Modeling Labs) Personal or Enterprise to build labs for study or test scenarios there will come the time you think about automating tasks and using the CML API. I was recently playing around with CML creating a new lab for testing the [pyATS framework](https://developer.cisco.com/docs/pyats/#!introduction/cisco-pyats-network-test--automation-solution){:target="_blank"}. I came very quick to the point to use the official Python library [virl2_client](https://github.com/CiscoDevNet/virl2-client){:target="_blank"} for CML to create a pyATS testbed automatically. During this process I stumbled over an issue which had an obvious solution.

## Install pyATS and virl2-client library

As I got a new laptop a month ago I have not installed all the tools I used before and also did not pull all the repository I was working on. So I started to clone my private repository I worked on, created a virtual environment, and installed pyATS:

```none
(pyats-test) pip install pyats
```

Note: There are different options how to install pyATS described in the documentation.

After that I installed the virl2-client library:

```none
(pyats-test) pip install virl2-client
```

Now I thought I could quickly create my testbed and start with testing. I used a small Python script which I copied from blog post "[How can I automate device configurations using CML2?](https://blogs.cisco.com/developer/363-askhankcml2-01){:target="_blank"}" by Hank Preston which contains a very good introduction and explanation how to use CML and pyATS.

paste the script here

show output of error

install correct version

```none
(pyats-test) pip install virl2-client
```

Github issue: https://github.com/CiscoDevNet/virl2-client/issues/20


## Finally create your pyATS testbed

pyats validate testbed 

I can highly recommend the  how to use it.


## Another cool helper for building labs

point to your repository

