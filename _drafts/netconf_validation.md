---
layout: post
title: "Validating NETCONF XML Configuration"
date: 2024-03-20 10:00:00 +0200
categories: [Automation, Python]
comments_id: 29
---

{: style="text-align: justify" }
In my previous blog post [NETCONF XML Payload with YANG models](https://blog.kuhlcloud.de/automation/python/2024/02/22/netconf-xml.html){:target="_blank"} I explained how to create proper XML payload using the Native and the OpenConfig YANG models for IOS-XE and NX-OS to send configuration data to the devices with NETCONF. I wrote that the next steps could be building another Python script using NETCONF with the ```<get>``` operation to validate the changes I made in a programmatic way. Here we are!

{: style="text-align: justify" }
I will show you the following two different options for the config validation:

✅ IOS-XE config validation with Native YANG model

✅ NX-OS config validation with OpenConfig YANG model

{: style="text-align: justify" }
Let's recap first what I did in the previous blog post. For the initial lab setup please refer to my [DevNet Expert Lab on Cisco Modeling Labs](https://github.com/daniel1820815/devnet-expert-cml-lab){:target="_blank"} GitHub repository. Then I did the configuration changes described at the previous blog post [NETCONF XML Payload with YANG models](https://blog.kuhlcloud.de/automation/python/2024/02/22/netconf-xml.html){:target="_blank"}. In short I configured the interfaces with IP addressing and the BGP configuration described on the diagram below.

![NETCONF Lab overview](/images/netconf_lab_overview.png "NETCONF Lab overview")
*Screenshot 1: NETCONF Lab overview in Cisco Modeling Labs.*

{: style="text-align: justify" }
Each of the three devices which are *Router1*, *Router2*, and *Nexus1*, have their own BGP AS and neighbor configurations on all interfaces except the management interfaces. In my previous blog post I validated the changes using Cisco YANG Suite with the build-in function to run RPCs to the devices in the web browser. This is not a very efficient way to validate the configuration changes and BGP neighbors. Now let's continue where I ended and validate that the configuration changes were applied in a programmatic way.

### Validating IOS-XE config using the Native YANG model

{: style="text-align: justify" }
Let's do a short recap about the BGP neighbor states that we can understand the returned data before we move on. BGP uses the Finite State Machine (FSM) to maintain a table of all BGP peers and their operational status. The BGP session may report in the following states:

- Idle
- Connect
- Active
- OpenSent
- OpenConfirm
- Established

{: style="text-align: justify" }
We want to see the *Established* state, because in this state, the BGP session is established and BGP neighbors exchange routes via Update messages. For more details take a look at the sample chapter [BGP Fundamentals](https://www.ciscopress.com/articles/article.asp?p=2756480&seqNum=4) from the book [Troubleshooting BGP: A Practical Guide to Understanding and Troubleshooting BGP](https://www.ciscopress.com/store/troubleshooting-bgp-a-practical-guide-to-understanding-9781587144646?w_ptgrevartcl=BGP+Fundamentals_2756480) available at Cisco Press.

{: style="text-align: justify" }
Now we will use a RPC *get* request to check the BGP neighbors. We use [Cisco YANG Suite](https://developer.cisco.com/yangsuite/){:target="_blank"} to create the filters we will then use within our Python script, but let's first check using the built-in RPCs function. As mentioned in the previous post, YANG Suite provides a set of tools and plugins to learn, test, and adopt YANG programmable interfaces such as NETCONF, RESTCONF, gNMI and more. I am using the DevNet Expert Candidate Workstation VM on which Cisco YANG Suite is already installed and is available on <http://localhost:8480>. The YANG module sets for IOS-XE and NX-OS we will use now are already configured. For installation option please refer to the [Cisco YANG Suite documentation](https://developer.cisco.com/docs/yangsuite/){:target="_blank"}.

{: style="text-align: justify" }
In YANG Suite go to **Protocols –> NETCONF**, select *IOS-XE* on the **YANG Set** from the Dropdown menu, search and select the *Cisco-IOS-XE-native* module and load the modules. Then choose the NETCONF operation, ```<get>``` in this case, and select **Router1** as device. Before browsing through the YANG tree, click on the "YANG Tree -> Options" and choose *NETCONF XML (RPC parameters only)* from **Display as RPC(s) as** to show only the configuration payload parameters. We don't need the other XML overhead.

{: style="text-align: justify" }
Browse the YANG tree and look for the **router** container. Expand it and move on to the **ios-bgp:bgp** list element which also needs to be expanded. Then you will find the **ios-bgp:neighbor** list element. Click the check button in the value column and then generate the filter using the **Build RPC** button. The filter should look like screenshot 2 below.

![Validating BGP Neighbors](/images/netconf_native_bgp_neighbor_config.png "Validating BGP Neighbors")
*Screenshot 2: Validating BGP Neighbors.*

{: style="text-align: justify" }
Next click the **Run RPC(s)** button to use the filter. A new browser tab will be opened where you can follow the complete RPC with all details including the output data of the BGP neighbors.

![BGP Neighbors reply](/images/netconf_native_bgp_neighbor_config_reply.png "BGP Neighbors reply")
*Screenshot 3: BGP Neighbors reply.*

Looks good, right? The BGP neighbors are there. But wait, the output tells us that the configuration we pushed via NETCONF was applied to the device, but it does not tell us if it is also working and forming BGP neighbors. We used the get operation to request configuration data and not the operational data. This is similar to a ```show run | section bgp``` command. It returns the configuration data from the device.

```none
Router1# show run | section bgp
router bgp 65001
 bgp log-neighbor-changes
 neighbor 10.0.10.2 remote-as 65002
 neighbor 10.0.20.2 remote-as 65002
 neighbor 10.0.30.3 remote-as 65003
```

*Example 1: BGP configuration on a IOS-XE device.*

Would it make more sense to request the operational data for the BGP neighbors? Yes, indeed! Let's quickly switch to the operational YANG module which is called *Cisco-IOS-XE-bgp-oper* and load the module. Clear the RPC filter data using the red **Clear RPC(s)** button before creating a new filter.

![BGP ops neighbors filter all](/images/netconf_native_bgp_neighbor_ops_filter_all.png "BGP ops neighbors filter all")
*Screenshot 4: BGP operational neighbors filter.*

![BGP ops neighbors full](/images/netconf_native_bgp_neighbor_ops_all_reply.png "BGP ops neighbors full")
*Screenshot 5: BGP operational neighbors full reply.*

![BGP ops neighbors filter](/images/netconf_native_bgp_neighbor_ops_filter.png "BGP ops neighbors filter")
*Screenshot 6: BGP operational neighbors filter on neighbor id and session-state.*

![BGP ops neighbors reply](/images/netconf_native_bgp_neighbor_ops_reply.png "BGP ops neighbors reply")
*Screenshot 7: BGP operational neighbors reply with neighbor id and session-state.*

```xml
<filter xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <bgp-state-data xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-bgp-oper">
    <neighbors>
      <neighbor>
        <neighbor-id/>
        <session-state/>
      </neighbor>
    </neighbors>
  </bgp-state-data>
</filter>
```

*Example 2: BGP neighbor filter on neighbor id and session-state.*

Thanks to Kirk Byers [IOS-XE and NETCONF Candidate Configuration Testing, Part1](https://pynet.twb-tech.com/blog/netconf/iosxe-candidate-cfg1.html){:target="_blank"} blog post, especially the section for *Grabbing the XML Configuration* was very helpful to get an idea to convert the XML reply data.
