Thanks to Kirk Byers [IOS-XE and NETCONF Candidate Configuration Testing, Part1](https://pynet.twb-tech.com/blog/netconf/iosxe-candidate-cfg1.html){:target="_blank"} blog post, especially the section for *Grabbing the XML Configuration*

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
