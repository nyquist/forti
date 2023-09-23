# NAT Modes

**NAT** means **Network Address Translation** and it is an operation that "translates" the source or the destination address of an incoming packet to another address when the packet is forwarded on an outgoing interface.

From the standpoint of which address is translated, there are 2 types of NAT:

* SNAT (Source NAT) - it is configured using IP Pools
* DNAT (Destination NAT) - it is configured using VIPs

In FortiGate devices, there are 2 ways of configuring NAT:

* **Firewall Policy NAT**: NAT is configured in the firewall policy entries - in this case the translation applies to the traffic that matches the policy
* **Central NAT**: NAT is configured per VDOM and one Central NAT rule could apply to multiple firewall policies. Central NAT cannot be enabled if there are VIPs or pools used in the firewall policies. Central SNAT is mandtory for NGFW policy-based mode. Central NAT can be disabled, but then the firewall stops processing NAT rules. The NAT rules must be added to the firewall policy.



## ARP Reply for VIPs and IP Pools

ARP Reply is enabled by deafult when using VIPs or IP Pools and it is best practice to keep it enabled. It can help in situations where the upstream router is not aware of the exact setup used on the firewall. The uspstream router can be setup to consider the VIP IP to be directly connected so it would expect an ARP reply from the VIP IP to create the L2 frame. ARP Reply can be disabled only if the routing in the upstream device correctly points that the VIP is reachable via the firewalls IP on the connected interface.

##





