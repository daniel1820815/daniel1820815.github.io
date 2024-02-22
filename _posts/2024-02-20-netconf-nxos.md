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

```cli
Nexus1# conf t
Enter configuration commands, one per line. End with CNTL/Z.
Nexus1(config)# int ethernet1/10
Nexus1(config-if)# ip address 10.0.50.3/24
                        ^
% Invalid command at '^' marker.
Nexus1(config-if)#
```

*Example 1: Pasting commands from a text editor with error.*

{: style="text-align: justify" }
NETCONF's transactional behavior ensures that the configuration will only be applied when every bit of it is correct. As soon as there's a wrong bit which is not accepted by the device, the whole configuration you wanted to be applied is refused. This mechanism provides a robust and resilient way to configure devices.

{: style="text-align: justify" }
As described before, in this blog post I will focus only on a small part of NETCONF which is the creation of proper XML configuration data to be used with the *edit-config* operation to push the configuration to the device. I will show you the following three different options for the XML payload:

✅ IOS-XE config with Native YANG

✅ IOS-XE config with OpenConfig YANG

✅ NX-OS config with Native + OpenConfig YANG

{: style="text-align: justify" }
For the NETCONF communication between the client (my workstation) and the server (the network devices) I will take advantage of the Python library [ncclient](https://ncclient.readthedocs.io/en/latest/){:target="_blank"}. The following diagram shows the lab setup in Cisco Modeling Labs I am using for the example.

![NETCONF Lab overview](/images/netconf_lab_overview.png "NETCONF Lab overview")
*Screenshot 1: NETCONF Lab overview in Cisco Modeling Labs.*

{: style="text-align: justify" }
For the initial lab setup please refer to my [DevNet Expert Lab on Cisco Modeling Labs](https://github.com/daniel1820815/devnet-expert-cml-lab){:target="_blank"} GitHub repository. There are only the management interfaces and some basic features configured, everything else like the interfaces with IP addressing and BGP configuration will be done via NETCONF.

{: style="text-align: justify" }
Before communicating with the devices using NETCONF, the NETCONF Agent must be enabled. The NETCONF Agent is enabled or disabled by entering the ```netconf-yang``` command on IOS-XE and ```feature netconf``` command on NX-OS. Additionally you need to enable OpenConfig on NX-OS using the ```feature openconfig``` command. Make also sure you have users with appropriate privileges configured on your devices that is *privilege level 15 on IOS-XE* and *network-admin or dev-ops role on NX-OS*. Last but not least you need connectivity from your workstation via SSHv2 on TCP port 830. NETCONF does not support SSH version 1. That's all part of the initial lab configuration in my example.

### Intro to Cisco YANG Suite

{: style="text-align: justify" }
The XML configuration data, also named payload, will be created with the help of [Cisco YANG Suite](https://developer.cisco.com/yangsuite/){:target="_blank"}. It provides a set of tools and plugins to learn, test, and adopt YANG programmable interfaces such as NETCONF, RESTCONF, gNMI and more. I am using the DevNet Expert Candidate Workstation VM on which Cisco YANG Suite is already installed and is available on <http://localhost:8480>. For installation option please refer to the [Cisco YANG Suite documentation](https://developer.cisco.com/docs/yangsuite/){:target="_blank"}.

{: style="text-align: justify" }
If it is your first time running YANG Suite, you should start at the [Getting Started](https://developer.cisco.com/docs/yangsuite/#!welcome-to-cisco-yang-suite/getting-started){:target="_blank"} section from the documentation and create Device profiles and download the supported YANG models from the devices or upload YANG model files from your workstation or from public [YANG Github repository](https://github.com/YangModels/yang){:target="_blank"} at the Setup menu on the right. After that you can create YANG module sets for IOS-XE and NX-OS with the supported modules you need.

![YANG Module Sets](/images/netconf_yang_module_sets.png "YANG Module Sets")
*Screenshot 2: Module Sets in YANG Suite.*

{: style="text-align: justify" }
Now we can take a closer look how to create the XML payload and use these models to send NETCONF at **Protocols –> NETCONF** in YANG Suite.

### Create XML Payloads with Cisco YANG Suite

#### Router1 - XML config payload using the Native YANG model for IOS-XE

{: style="text-align: justify" }
Let us start with creating the XML configuration payload for the IOS-XE Router1 using the Native YANG model. Go to **Protocols –> NETCONF** in YANG Suite, select *IOS-XE* on the **YANG Set** from the Dropdown menu, search and select the *Cisco-IOS-XE-native* module and load the modules. Then choose the NETCONF operation, ```<edit-config>``` in our case, and select **Router1** as device. Before we start browsing through the YANG tree, click on the "YANG Tree -> Options" and choose *NETCONF XML (RPC parameters only)* from **Display as RPC(s) as** to show only the configuration payload parameters.

![YANG Tree Settings](/images/netconf_yang_tree_settings.png "YANG Tree Settings")
*Screenshot 3: YANG Tree Settings in YANG Suite.*

{: style="text-align: justify" }
Now we can start browsing the YANG tree and look for the **interfaces** container. Unfortunately there is no option to sort the YANG tree in alphabetical order, therefore I recommend using the search option from the browser. Expand the **interfaces** container, look for the **GigabitEthernet** list element and expand it. The small key symbol at the **name** leaf tells you that this leaf is the key of the list item and mandatory. Add the interface number *2* in the value field of the **name**. Another important setting is the **shutdown** leaf which has only a checkbox. Check the box in the **Value** column and add the *remove* operation in the **Operation** column. This setting enables the interface and is the equivalent to the *no shutdown* CLI command.

![Native YANG Interfaces](/images/netconf_native_interfaces1.png "Native YANG Interfaces")
*Screenshot 4: GigabitEthernet list from interface container of the Cisco-IOS-XE-native module.*

{: style="text-align: justify" }
Scroll further down to the **ip** container, expand it, and then choose **address-choice -> address -> address -> address-choice -> fixed case -> primary**. Add the IP address *10.0.10.1* at the **address** leaf and the subnet mask *255.255.255.0* at the **mask** leaf. After that click **Build RPC** and the XML config payload is generated on the right side.

![Native YANG Interfaces](/images/netconf_native_interfaces2.png "Native YANG Interfaces")
*Screenshot 5: First XML config payload for interfaces from the Cisco-IOS-XE-native module.*

{: style="text-align: justify" }
You could use YANG Suite now to do a RPC to **Router1** in a new browser tab and send the payload when clicking on **Run RPC(s)**. But for now we move on to the BGP configuration part which can be found at the **router** container and the **ios-bgp:bgp** list element. The leaf **ios-bgp:id** is the key and it represents the AS number *65001* in our case. Expand the list element **ios-bgp:neighbor**, add the neighbor ID *10.0.10.2* as value of the **ios-bgp:ip** leaf item, and add *65002* to the **ios-bgp:remote-as** leaf item. Click the red **Clear RPC(s)** button to clear the XML payload section before clicking **Build RPC(s)** again otherwise the new added XML payload data will be added to the existing data instead of re-creating the whole thing.

![Native YANG BGP](/images/netconf_native_bgp.png "Native YANG BGP")
*Screenshot 6: All XML config payload parts from the Cisco-IOS-XE-native module.*

{: style="text-align: justify" }
Now we have all XML payload configuration parts ready. Then we can replicate what we have put together so far for the other interfaces and BGP neighbors to create the full XML configuration payload for **Router1**:

```xml
<config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
    <interface>
      <GigabitEthernet>
        <name>2</name>
        <shutdown xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="remove"/>
        <ip>
          <address>
            <primary>
              <address>10.0.10.1</address>
              <mask>255.255.255.0</mask>
            </primary>
          </address>
        </ip>
      </GigabitEthernet>
      <GigabitEthernet>
        <name>3</name>
        <shutdown xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="remove"/>
        <ip>
          <address>
            <primary>
              <address>10.0.20.1</address>
              <mask>255.255.255.0</mask>
            </primary>
          </address>
        </ip>
      </GigabitEthernet>
      <GigabitEthernet>
        <name>4</name>
        <shutdown xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="remove"/>
        <ip>
          <address>
            <primary>
              <address>10.0.30.1</address>
              <mask>255.255.255.0</mask>
            </primary>
          </address>
        </ip>
      </GigabitEthernet>
    </interface>
    <router>
      <bgp xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-bgp">
        <id>65001</id>
        <neighbor>
          <id>10.0.10.2</id>
          <remote-as>65002</remote-as>
        </neighbor>
        <neighbor>
          <id>10.0.20.2</id>
          <remote-as>65002</remote-as>
        </neighbor>
        <neighbor>
          <id>10.0.30.3</id>
          <remote-as>65003</remote-as>
        </neighbor>
      </bgp>
    </router>
  </native>
</config>
```

*Example 2: Complete XML config payload for Router1 from IOS-XE native model.*

{: style="text-align: justify" }
So far so good. The complete XML config payload for Router1 from IOS-XE native models is ready. Now let us move on to the XML configuration payload for **Router2**, but this time we will use the OpenConfig YANG model.

#### Router2 - XML config payload using the OpenConfig YANG model for IOS-XE

{: style="text-align: justify" }
For the usage of OpenConfig YANG modules you need to choose all the single modules you need to build XML payload for and not only the single native module like we did for Router1 before. We select *IOS-XE* on the **YANG Set** from the Dropdown menu, but this time we search and select the following OpenConfig YANG modules:

- *openconfig-interfaces*
- *openconfig-if-ip*
- *openconfig-network-instance*
- *openconfig-policy-types*

{: style="text-align: justify" }
You can find out how the various modules are linked together by using right click on the mouse while on a module in the YANG tree and choose **properties**. There is a lot of useful information about the modules and which other submodules linked to it. The same option is available from left menu at **Explore -> YANG** where you can load the individual modules and explore the details.

![OpenConfig YANG Module properties](/images/netconf_openconfig_properties.png "OpenConfig YANG Module properties")
*Screenshot 5: OpenConfig YANG Module properties*

{: style="text-align: justify" }
Load the modules and make sure you selected the NETCONF operation ```<edit-config>```. You do not need to select a device as we will not do NETCONF RPC from YANG Suite for now. Expand the **openconfig-interface** module as well as the **interfaces** container and the **interface** list element. Add the **name** *GigabitEthernet2* which is the key of the list element like it was for the native model. Expand the **config** container, add gain the interface **name**, choose *ianaift:ethernetCsmacd* for the **type** leaf, and select *true* for the **enabled** leaf element.

![OpenConfig YANG Interfaces](/images/netconf_openconfig_interfaces1.png "OpenConfig YANG Interfaces")
*Screenshot 6: OpenConfig YANG Interfaces*

{: style="text-align: justify" }
The IP address configuration is a little bit hided in the OpenConfig YANG models. Scroll down to the **subinterfaces** container, expand it, then expand the **subinterface** list element. The key **index** needs to be *0* in the case that you do not have a subinterface which is some kind of confusing but the IP addresses need to be set there. Move on expand **oc-ip:ipv4 -> oc-ip:addresses -> oc-ip:address** and add *10.0.10.2* as **oc-ip:ip**. Expand the **oc-ip:config** container, add the IP address at **oc-ip:ip** again, and add *24* as **oc-ip:prefix-length**. Make sure that you have cleaned the XML payload section before you hit **Build RPC(s)** to take a look what we have so far.

![OpenConfig YANG Interfaces IP settings](/images/netconf_openconfig_interfaces2.png "OpenConfig YANG Interfaces IP settings")
*Screenshot 7: OpenConfig YANG Interfaces IP settings*

{: style="text-align: justify" }
Let us move to the **openconfig-network-instance** model to create the configuration for BGP on Router2. Expand the **network-instances** container and the **network-instance** list element. The network instances are the layer-2, layer-3, or layer-2+layer-3 forwarding instances on a NX-OS device. In our case we are using the *default* network instance as **name** which is the key of the list. As the **default** network instance is already present on the device we do not need to use the **config** container in the **network-instance** and can move on to the **protocols** container.

![OpenConfig YANG Network Instances](/images/netconf_openconfig_network_instances.png "OpenConfig YANG Network Instances")
*Screenshot 8: OpenConfig YANG Network Instances*

{: style="text-align: justify" }
Expand the **protocols** container and the **protocol** list element. Now it is getting a little bit more complicated. The **identifier** of the protocol needs to be added as *oc-pol-types:INSTALL_PROTOCOL_TYPE* which is in our case *oc-pol-types:BGP*. Click into the value field before and then you see that there is a reference down to the **config** container, expand it, and you have a drop down where you can see the valid values to choose from.

![OpenConfig YANG Protocols in Network Instances](/images/netconf_openconfig_protocols1.png "OpenConfig YANG Protocols in Network Instances")
*Screenshot 9: OpenConfig YANG Protocols in Network Instances*

{: style="text-align: justify" }
As mentioned before we use *oc-pol-types:BGP* as **identifier** in the **protocol** list and in the **config** container. We do the same with *65002* for the **name** which is a unique name for the protocol instance. You can add whatever string you want but for better correlation I decided to use the BGP AS number.

![OpenConfig YANG BGP Protocol in Network Instances](/images/netconf_openconfig_protocols2.png "OpenConfig YANG BGP Protocol in Network Instances")
*Screenshot 10: OpenConfig YANG BGP Protocol in Network Instances*

{: style="text-align: justify" }
Scroll further down to the **bgp** container and expand it. Expand the **global** container as well and add the **as** number *65002* and the **router-id** of *2.2.2.2*. The move on to the **neighbor** container, expand it, and under the **neighbor** list element add the **neighbor-address** of Router1 which is *10.0.10.1*. Add the same value under the **config** container at the **neighbor-address** and use *65001* as **peer-as** which is the equivalent of the *remote-as* on the neighbor statement on the CLI.

![OpenConfig YANG BGP Config](/images/netconf_openconfig_bgp.png "OpenConfig YANG BGP Config")
*Screenshot 11: OpenConfig YANG BGP Config*

{: style="text-align: justify" }
Remember to clear the XML payload section using the **Clear RPC(s)** button and then hit **Build RPC(s)**. Similar as we did for Router1, we are able to replicate the XML payload for the other interfaces and BGP neighbors to complete the full XML payload for Router2. It should then look like example 3 below. Now there is only the NX-OS device **Nexus1** left.

```xml
<config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <interfaces xmlns="http://openconfig.net/yang/interfaces">
    <interface>
      <name>GigabitEthernet2</name>
      <config>
        <name>GigabitEthernet2</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
      </config>
      <subinterfaces>
        <subinterface>
          <index>0</index>
          <ipv4 xmlns="http://openconfig.net/yang/interfaces/ip">
            <addresses>
              <address>
                <ip>10.0.10.2</ip>
                <config>
                  <ip>10.0.10.2</ip>
                  <prefix-length>24</prefix-length>
                </config>
              </address>
            </addresses>
          </ipv4>
        </subinterface>
      </subinterfaces>
    </interface>
    <interface>
      <name>GigabitEthernet3</name>
      <config>
        <name>GigabitEthernet3</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
      </config>
      <subinterfaces>
        <subinterface>
          <index>0</index>
          <ipv4 xmlns="http://openconfig.net/yang/interfaces/ip">
            <addresses>
              <address>
                <ip>10.0.20.2</ip>
                <config>
                  <ip>10.0.20.2</ip>
                  <prefix-length>24</prefix-length>
                </config>
              </address>
            </addresses>
          </ipv4>
        </subinterface>
      </subinterfaces>
    </interface>
    <interface>
      <name>GigabitEthernet4</name>
      <config>
        <name>GigabitEthernet4</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
      </config>
      <subinterfaces>
        <subinterface>
          <index>0</index>
          <ipv4 xmlns="http://openconfig.net/yang/interfaces/ip">
            <addresses>
              <address>
                <ip>10.0.40.2</ip>
                <config>
                  <ip>10.0.40.2</ip>
                  <prefix-length>24</prefix-length>
                </config>
              </address>
            </addresses>
          </ipv4>
        </subinterface>
      </subinterfaces>
    </interface>
  </interfaces>
  <network-instances xmlns="http://openconfig.net/yang/network-instance">
    <network-instance>
      <name>default</name>
      <protocols>
        <protocol>
          <identifier xmlns:oc-pol-types="http://openconfig.net/yang/policy-types">oc-pol-types:BGP</identifier>
          <name>65002</name>
          <config>
            <identifier xmlns:oc-pol-types="http://openconfig.net/yang/policy-types">oc-pol-types:BGP</identifier>
            <name>65002</name>
          </config>
          <bgp>
            <global>
              <config>
                <as>65002</as>
                <router-id>2.2.2.2</router-id>
              </config>
            </global>
            <neighbors>
              <neighbor>
                <neighbor-address>10.0.10.1</neighbor-address>
                <config>
                  <neighbor-address>10.0.10.1</neighbor-address>
                  <peer-as>65001</peer-as>
                </config>
              </neighbor>
              <neighbor>
                <neighbor-address>10.0.20.1</neighbor-address>
                <config>
                  <neighbor-address>10.0.20.1</neighbor-address>
                  <peer-as>65001</peer-as>
                </config>
              </neighbor>
              <neighbor>
                <neighbor-address>10.0.40.3</neighbor-address>
                <config>
                  <neighbor-address>10.0.40.3</neighbor-address>
                  <peer-as>65003</peer-as>
                </config>
              </neighbor>
            </neighbors>
          </bgp>
        </protocol>
      </protocols>
    </network-instance>
  </network-instances>
</config>
```

*Example 3: Complete XML config payload for Router2 from OpenConfig YANG models.*

#### Nexus1 - XML config payload using the Native + OpenConfig YANG model for NX-OS

{: style="text-align: justify" }
For the NX-OS device I had several challenges during testing. Initially I wanted to use only the OpenConfig YANG modules to create the XML payload for the configuration. It quickly turned out that not all functions of the CLI were implemented in the OpenConfig YANG model for NX-OS, for example it is not possible to change an interface from a layer-2 to a layer-3 interface. On the CLI you would simply enter *no switchport* at the interface configuration and that's it. So I decided to use a combination of both YANG modules, the Native Cisco-NX-OS-device module and the OpenConfig modules. Furthermore I thought it would be a good idea to keep the configuration parts separated in to single files and send it later via NETCONF sequentially to the NX-OS device.

First interfaces to layer3

```xml
<config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <System xmlns="http://cisco.com/ns/yang/cisco-nx-os-device">
    <intf-items>
      <phys-items>
        <PhysIf-list>
          <id>eth1/1</id>
          <layer>Layer3</layer>
        </PhysIf-list>
        <PhysIf-list>
          <id>eth1/2</id>
          <layer>Layer3</layer>
        </PhysIf-list>
      </phys-items>
    </intf-items>
  </System>
</config>
```

*Example 4: Physical interface XML config payload for Nexus1 from .*

then ip addressing on interfaces...

```xml
<config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <interfaces xmlns="http://openconfig.net/yang/interfaces">
    <interface>
      <name>eth1/1</name>
      <config>
        <name>eth1/1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
      </config>
      <subinterfaces>
        <subinterface>
          <index>0</index>
          <ipv4 xmlns="http://openconfig.net/yang/interfaces/ip">
            <addresses>
              <address>
                <ip>10.0.30.3</ip>
                <config>
                  <ip>10.0.30.3</ip>
                  <prefix-length>24</prefix-length>
                </config>
              </address>
            </addresses>
          </ipv4>
        </subinterface>
      </subinterfaces>
    </interface>
    <interface>
      <name>eth1/2</name>
      <config>
        <name>eth1/2</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
      </config>
      <subinterfaces>
        <subinterface>
          <index>0</index>
          <ipv4 xmlns="http://openconfig.net/yang/interfaces/ip">
            <addresses>
              <address>
                <ip>10.0.40.3</ip>
                <config>
                  <ip>10.0.40.3</ip>
                  <prefix-length>24</prefix-length>
                </config>
              </address>
            </addresses>
          </ipv4>
        </subinterface>
      </subinterfaces>
    </interface>
  </interfaces>
</config>
```

last but not least the bgp configuration including neighbor statements

```xml
<config>
  <network-instances xmlns="http://openconfig.net/yang/network-instance">
    <network-instance>
      <name>default</name>
      <config>
        <type xmlns:oc-ni-types="http://openconfig.net/yang/network-instance-types">oc-ni-types:DEFAULT_INSTANCE</type>
        <enabled-address-families xmlns:oc-types="http://openconfig.net/yang/openconfig-types">oc-types:IPV4</enabled-address-families>
      </config>
      <protocols>
        <protocol>
          <name>bgp</name>
          <identifier xmlns:oc-pol-types="http://openconfig.net/yang/policy-types">oc-pol-types:BGP</identifier>
          <config>
            <identifier xmlns:oc-pol-types="http://openconfig.net/yang/policy-types">oc-pol-types:BGP</identifier>
            <name>bgp</name>
          </config>
          <bgp>
            <global>
              <config>
                <as>65003</as>
                <router-id>3.3.3.3</router-id>
              </config>
            </global>
            <neighbors>
              <neighbor>
                <neighbor-address>10.0.30.1</neighbor-address>
                <config>
                  <neighbor-address>10.0.30.1</neighbor-address>
                  <peer-as>65001</peer-as>
                </config>
                <afi-safis>
                  <afi-safi>
                    <afi-safi-name xmlns:oc-bgp-types="http://openconfig.net/yang/bgp-types">oc-bgp-types:IPV4_UNICAST</afi-safi-name>
                    <ipv4-unicast>
                    <config>
                      <send-default-route>false</send-default-route>
                    </config>
                    </ipv4-unicast>
                    <config>
                      <afi-safi-name xmlns:oc-bgp-types="http://openconfig.net/yang/bgp-types">oc-bgp-types:IPV4_UNICAST</afi-safi-name>
                    </config>
                  </afi-safi>
                </afi-safis>
              </neighbor>
              <neighbor>
                <neighbor-address>10.0.40.2</neighbor-address>
                <config>
                  <neighbor-address>10.0.40.2</neighbor-address>
                  <peer-as>65002</peer-as>
                </config>
                <afi-safis>
                  <afi-safi>
                    <afi-safi-name xmlns:oc-bgp-types="http://openconfig.net/yang/bgp-types">oc-bgp-types:IPV4_UNICAST</afi-safi-name>
                    <ipv4-unicast>
                    <config>
                      <send-default-route>false</send-default-route>
                    </config>
                    </ipv4-unicast>
                    <config>
                      <afi-safi-name xmlns:oc-bgp-types="http://openconfig.net/yang/bgp-types">oc-bgp-types:IPV4_UNICAST</afi-safi-name>
                    </config>
                  </afi-safi>
                </afi-safis>
              </neighbor>
            </neighbors>
          </bgp>
        </protocol>
      </protocols>
    </network-instance>
  </network-instances>
</config>
```


### Pushing the configs using ncclient

```python
'''Python script to push XML configuration payload to devices via NETCONF'''
import logging
from ncclient import manager

logging.basicConfig(level=logging.DEBUG)

# List of devices IOSXE devices
iosxe_devices = ["192.168.255.51", "192.168.255.52", "192.168.255.53"]

# List of NX-OS device configurations
nxos_configs = ["native_interfaces", "openconfig_interfaces", "openconfig_bgp"]

# Loop through the devices and connect to it
for device in iosxe_devices:
    router = manager.connect(
        host=device,
        username="expert",
        password="1234QWer!",
        hostkey_verify=False
        )

    # For the NX-OS device
    if device == "192.168.255.53":
        # Loop through the NXOS configs and apply it
        for config in nxos_configs:
            with open(f"nxos_{config}.xml", encoding="utf-8") as file:
                payload = file.read()
            router.edit_config(payload, target="running")
    # For all other devices
    else:
        with open(f"{device}.xml", encoding="utf-8") as file:
            payload = file.read()
        router.edit_config(payload, target="running")
```

### Verification & Validation

show example of an error message

in our case everything went well and we got an ```<ok/>``` message back from the devices...

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rpc-reply xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="urn:uuid:42aeb554-42a7-4dc6-abb3-47776dc9e635">
    <ok/>
</rpc-reply>
```

Verification

manually

But we could also use YANG Suite to vaidate our changes...

The next steps could be to build another Python script using NETCONF with ```<get>``` operation to validate the changes but this is something for the another blog post.
