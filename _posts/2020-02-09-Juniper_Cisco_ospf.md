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
{: style="color:black; font-size: 80%;"}

* vmx-Juniper Router14.1 image
* c7200-adventerprisek9-mz.124-24.T5.image
* GNS3 Newtwork Emulation software
{: style="color:black; font-size: 80%;"}

## Key Notes
### Interface mappings
Ports on the Juniper VMX and GNS3 map like such:
{: style="color:black; font-size: 80%;"}
<pre>
ge-0/0/0 = Ethernet 2

ge-0/0/1 = Ethernet 3

ge-0/0/2 = Ethernet 4

ge-0/0/3 = Ethernet 5

etc...
</pre>
{: style="color:gray; font-size: 70%;"}

So, per the image below, <b>e3 = ge-0/0/1</b>
{: style="color:black; font-size: 80%;"}
{:refdef: style="text-align: left;"}
![My Image]({{ site.baseimg }}/images/jun_interface.PNG)
{: refdef}
### Process ID's
While Cisco uses a process ID when configuring OSPF; <b> router ospf PROCESS_ID </b>, Junpier does not. If you want to configure a different process ID for Juniper to partake in within a hybrid environment, you must configure a routing instance and configure OSPF within that instance. That is beyond the scope of this write up.
{: style="color:black; font-size: 80%;"}

## The Configuration
Below is the topology I used to connect my Juniper VMX device into my Cisco 7200 topology using OSPF 1 in area 0:
{: style="color:black; font-size: 80%;"}
{:refdef: style="text-align: left;"}
![My Image]({{ site.baseimg }}/images/top.PNG)
{: refdef}

OSPF configuration on Cisco is straightforward. Create the OSPF instance and advertise any networks you wish to propogate. For Juniper, after you have added your addresses to the interfaces, create an OSPF protocol instance, and assign the interfaces you want to propogate.
{: style="color:black; font-size: 80%;"}

### Cisco OSPF Configuration
<pre>
interface FastEthernet1/1
 ip address 8.8.12.1 255.255.255.0
 duplex auto
 speed auto

router ospf 1
 log-adjacency-changes
 network 3.3.3.3 0.0.0.0 area 0
 network 8.8.9.2 0.0.0.0 area 0
 network 8.8.12.1 0.0.0.0 area 0
 </pre>
 {: style="color:gray; font-size: 70%;"}

### Juniper OSPF configuration

 <pre>
 }
interfaces {
    ge-0/0/1 {
        unit 0 {
            family inet {
                address 8.8.12.2/24;
            }
        }
    }
    ge-0/0/2 {
        unit 0 {
            family inet {
                address 9.9.9.1/24;
            }
        }
    }
}
routing-options {
    static {
        route 0.0.0.0/0 next-hop 8.8.12.1;
    }
}
protocols {
    ospf {
        area 0.0.0.0 {
            interface ge-0/0/1.0;
            interface ge-0/0/2.0;
        }
    }
}
</pre>
{: style="color:gray; font-size: 70%;"}

I set up a 9.9.9.0/24 network on the Juniper and added it to the OSPF area 0 as well. You can see on the Cisco device's routing table I now have a learned route to the 9.9.9.0/24 network
{: style="color:black; font-size: 80%;"}
{:refdef: style="text-align: left;"}
![My Image]({{ site.baseimg }}/images/route_table.PNG)
{: refdef}