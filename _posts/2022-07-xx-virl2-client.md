---
layout: post
title:  "vril2_client for Cisco Modeling Labs"
date:   2022-08-14 20:00:00 +0200
categories: Automation
---

If you are using CML (Cisco Modeling Labs) Personal or Enterprise to build labs for study or test scenarios there will come the time you think about automating tasks and using the CML API. I was recently playing around with CML creating a new lab for testing the [pyATS framework](https://developer.cisco.com/docs/pyats/#!introduction/cisco-pyats-network-test--automation-solution){:target="_blank"}. I came very quick to the point to use the official Python library [virl2_client](https://github.com/CiscoDevNet/virl2-client){:target="_blank"} for CML to create a pyATS testbed automatically. During this process I stumbled over an issue which had an obvious solution.

## Install pyATS and virl2-client library

As I got a new laptop a month ago I have not installed all the tools I used before and also did not pull all the repository I was working on. So I started to clone my private repository I worked on, created a virtual environment, and installed pyATS:

```bash
(pyats-test) pip install pyats
```

Note: There are different options how to install pyATS described in the documentation.

After that I installed the virl2-client library:

```bash
(pyats-test) pip install virl2-client
```

Now I thought I could quickly create my testbed and start with testing. I used a small Python script which I copied from blog post "[How can I automate device configurations using CML2?](https://blogs.cisco.com/developer/363-askhankcml2-01){:target="_blank"}" by Hank Preston which contains a very good introduction and explanation how to use CML and pyATS.

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

You have to fill in your CML data, add the name of your lab, and specify the output file for the testbed. The script does the rest for you. This is the step where I ran into the issue.

```bash
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

The check of the controller version failed. The error told me that the virl2_client version I used was incompatible with the controller version. At this time I was confused and not aware about the fact that the virl2_client version need to match with the controller version.

install correct version

```none
(pyats-test) pip install virl2-client
```

Github issue: https://github.com/CiscoDevNet/virl2-client/issues/20


## Finally create your pyATS testbed

pyats validate testbed 

I can highly recommend the  how to use it.


## A repository to grow

point to your repository

Links:

- Github issue: https://github.com/CiscoDevNet/virl2-client/issues/20
- my repository
- pyATS documentation
- Cisco Modeling Labs
- [virl2_client](https://github.com/CiscoDevNet/virl2-client){:target="_blank"}
- [How can I automate device configurations using CML2?](https://blogs.cisco.com/developer/363-askhankcml2-01){:target="_blank"}"
