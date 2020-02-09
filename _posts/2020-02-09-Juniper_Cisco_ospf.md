---
title: "OSPF in GNS3 between Juniper-VMX & Cisco"
date: 2020-01-31
tags: [Networking, Cisco, Juniper, OSPF, routing, Information Technology]
#header:
#  image: "/images/Ansible_Cisco.png"
excerpt: "Networking, Cisco, Juniper, OSPF, routing, Information Technology"
---
# OSPF between Cisco and Juniper VMX device
After  hours of banging my head against the wall I finally worked out all the bugs. For this configuration I'm using the following:

* vmx-Juniper Router14.1 image
* c7200-adventerprisek9-mz.124-24.T5.image
* GNS3 Newtwork Emulation software

## Key Notes
### Interface mappings
The reason this took so long as ultimately due to the port mappings of the Juniper image and GNS3. The ports map like such:
<pre>
ge-0/0/0 = Ethernet 2

ge-0/0/1 = Ethernet 3

ge-0/0/2 = Ethernet 4

ge-0/0/3 = Ethernet 5

etc...
</pre>
So, per the image below, <b>e3 = ge-0/0/1</b>
{:refdef: style="text-align: left;"}
![My Image]({{ site.baseimg }}/images/jun_interface.png)
{: refdef}
### Process ID's
While Cisco uses a process ID when configuring OSPF; <b> router ospf PROCESS_ID </b>, Junpier does not. If you want to configure a different process ID for Juniper to partake in within a hybrid environment, you must configure a routing instance and configure OSPF within that instance. That is beyond the scope of this write up.

## The Configuration
Below is the topology I used to connect my Juniper VMX device into my Cisco 7200 topology using OSPF 1 in area 0:
{:refdef: style="text-align: left;"}
![My Image]({{ site.baseimg }}/images/top.png)
{: refdef}
