# DNAT

To perfrom DNAT (Destination NAT), FortiGates use the concept of VIP (Virtual IP). For sessions where the VIP is the destination IP, DNAT will be performed. After VIP is defined, when it is used as the destination in the firewall policy, DNAT will be automatically used.

## Static NAT VIP

* This is a one-to-one mapping between a VIP and an internal IP
* FortiGate performs DNAT for incoming traffic matching the VIP
* For outgoing traffic, FortiGate performs SNAT with the VIP for traffic sourced by the mapped internal address - provided the matching policy has NAT enabled. This behavior can change if an IP Pool is used instead of the interface IP.

## FQDN VIP

allows the uses of FQDNs for both the external and internal IP addresses. This provides an automatic update of the addresses when the FQDN resolved address change.

## Port Forwarding

When this option is used, you can limit the ports for which DNAT is performed and you can also define multiple VIPs using the same IP as long as the forwarded ports don't overlap. This also means that it is possible to change the destination port of the traffic.

## Policy matching

In FortiOS, VIPs and address objects do not overalp. That means the address object "all" for example doesn't include any VIP address. When using DENY rules, you can deny traffic destined to a VIP by using the VIP in the destination field, or by enabling "match-vip" in the policy (available only for DENY rules). This instructs the firewall to also check the VIPs during policy evaluation.&#x20;

## Central DNAT

When Central NAT is used, the VIPs no longer reside on the firewall policies. Instead, the configuration moves to the DNAT and Virtual IPs page. A firewall rule must ALLOW the traffic before it can be processed by Centra DNAT, but the rule will use the mapped internal address as destination, not the external address. This is because, in the order of operations DNAT is performed before firewall policy lookups.

Once a VIP is created in Central NAT mode, DNAT is automatically performed for it through a special rule in the kernel enabled with Central NAT. You can set it's status to "disabled" to prevent this.
