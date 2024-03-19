---
layout: post
title: "NETCONF XML Payload Validation"
date: 2024-02-21 10:00:00 +0200
categories: [Automation, Python]
comments_id: 29
---

{: style="text-align: justify" }
In my previous blog post [NETCONF XML Payload with YANG models](https://blog.kuhlcloud.de/automation/python/2024/02/22/netconf-xml.html){:target="_blank"} I explained how to create proper XML payload using the Native YANG model and the OpenConfig YANG model for IOS-XE and NX-OS to send configuration data to the devices with NETCONF. I wrote that the next steps could be to build another Python script using NETCONF with the ```<get>``` operation to validate the changes I made in a programmatic way. Here we are!

Validation both IOS-XE router configuration using the Native YANG models

Short recap about the BGP states

BGP uses the Finite State Machine (FSM) to maintain a table of all BGP peers and their operational status. The BGP session may report in the following states:

Idle
Connect
Active
OpenSent
OpenConfirm
Established

First show a get to the configuration data and show the state is missing

```xml
<filter>
    <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
    <router>
        <bgp xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-bgp">
            <id/>
            <neighbor/>
        </bgp>
    </router>
    </native>
</filter>
```

Screenshots from 2024-03-19 14:25

Looks good, right? The BGP neighbors are there. But wait, the output tells us that the configuration we pushed via NETCONF was applied to the device, but it does not tell us if it is also working and forming BGP neighbors. We used the get operation to request configuration data and not the operational data. This is similar to a *show run | section bgp* command. It returns the configuration data from the device.

```none
Router1# show run | section bgp
router bgp 65001
 bgp log-neighbor-changes
 neighbor 10.0.10.2 remote-as 65002
 neighbor 10.0.20.2 remote-as 65002
 neighbor 10.0.30.3 remote-as 65003
```

Would it make more sense to request the operational data for the BGP neighbors? Yes, indeed!

Switch the ops module 

then show the operational data and operational data with state established filter

```python
'''Python script to push XML configuration payload to devices via NETCONF'''
from ncclient import manager
from lxml import etree

# List of devices IOSXE devices
# iosxe_devices = ["192.168.255.51", "192.168.255.52", "192.168.255.53"]
iosxe_devices = ["192.168.255.51"]

iosxe_native_filter = '''
    <filter xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <bgp-state-data xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-bgp-oper">
            <neighbors>
            <neighbor>
                <neighbor-id/>
            </neighbor>
            </neighbors>
        </bgp-state-data>
    </filter>
'''

nxos_openconfig_filter = '''

'''

# Loop through the devices and connect to it
for device in iosxe_devices:
    router = manager.connect(
        host=device,
        username="expert",
        password="1234QWer!",
        hostkey_verify=False
        )

    # Choose which filter to use
    if device == "192.168.255.53":
        netconf_filter = nxos_openconfig_filter
    else:
        netconf_filter = iosxe_native_filter

    # Get BGP neighbors based on filter and print out
    response = router.get(filter=netconf_filter)
    xml_data = etree.tostring(
        response.data_ele,
        pretty_print=True
        ).decode()
    print(f"\nBGP neighbors for {device}:\n")
    print(xml_data)
    print("\n")
```

Thanks to Kirk Byers [IOS-XE and NETCONF Candidate Configuration Testing, Part1](https://pynet.twb-tech.com/blog/netconf/iosxe-candidate-cfg1.html){:target="_blank"} blog post, especially the section for *Grabbing the XML Configuration*
