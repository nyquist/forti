# IPSec VPN

Also see [https://ccie.nyquist.eu/security/ipsec-vpn-101](https://ccie.nyquist.eu/security/ipsec-vpn-101)

Fortinet doesn't use AH because it doesn't offer encryption.

IPSec can be used to provide Remote Access when a VPN Client, such as FortiClient is used, or it can be used for site-to-site VPN.

ADVPN (Auto-Discovery VPN) allows spokes to dyncamicall negociate on-demand VPNs between spokes.

## Configuration

Adding a IPSEC VPN tunnel takes the GUI user to the IPSec Wizard. There are predefined templates (site-to-site, hub-and-spoke, remote-access) as well as custom.

### Phase 1

The initiator is the peer that starts the Phase 1 negotiation, while the reposnder is the peer that responds to the neogitation request.

The purpose of Phase 1 is to authenticate peers and to set up a secure channel for negotiating the Phase 2 SAs (IPSec SA) that are later used to encrypt the traffic between peers. The Phase 1 SA is called IKE SA.

For authentication, you can use pre-shared keys or digital signatures and you can also add an extra authentication methods (XAuth)

At the end of pahse 1, the DH keys used in Phase 2 are negociated.

#### Phase 1 - Network

In this section of Phase 1, you define&#x20;

* The **Remote Gateway**
  * Static IP - use the IP of the peer
  * Dialup User - used when the peer IP is unknown
  * Dynamic DNS - use the FQDN of the pper
* the **interface** that will terminate IPSec traffic
* the **Local Gateway** (used to select the IP when the interface has multiple addresses)
* **Mode Config** - when enabled, it pushes network config (IP, netmask, DNS) to the clients over IKE messages. Useful in dialup scenarios. The settings must match on both ends. It is enabled by default on FortiClient, but it needs to be enabled on a FortiGate
* **NAT Traversal**
  * when enabled, it detects if NAT devices exist on the path. If yes, then both ESP and IKE use UDP 4500
  * when set to Forced, ESP and IKE always use UDP 4500
* **Keepalive** frequency
* **Dead Peer Detection** settings
  * On Demand (default) - DPD probes are sent when there is no inbound traffic
  * On Idle - DPD probes are sent when there is no traffic
  * Disabled - DPD probes are not sent, but reply to them.
* **Forward Error Correction** (FEC) - useful on noisy links, but doesn't support hardware offloading
* **Advanced**
  * **Add Route** - disable it if a dynamic routing protocol is used on the tunnel
  * **Auto discovery sender** - useful to send ADVPN invite
  * **Auto discvovery receiver** - useful to accept an ADVPN invite
  * **Exchange Interface IP** - When enabled, a point-to-multipoint connection can be created
  * **Device creation** - creates an interface for each dial-up client.&#x20;
  * **Aggregate member** - allows you to aggregate multiple tunnles into a single interface.

#### Phase 1 - Authentication

* **Method**:
  * **PSK**
  * **Signature** - The digital signature of one peer is validated by the presence of the CA certificate installed on the other peer.
* **Version**: IKEv1 or IKEv2
* **Mode**:
  * **Aggresive**
    * Less secure, faster negotiation (3 packets), required when peer ID check is needed (when the IP alone cannot identify the peer)
  * **Main**
    * More secure, slower negotiation (6 packets)

#### Phase 1 - Proposal

* **Encryption** - selects the algorithm used for encryption
* **Authentication** - selectes the algorithm used to verify the integrity and authenticity of the data
* **Diffie-Hellman Groups** (DH): The higher the group, the more secure, but also with higher compute time. At least 1 value must be selected
* **Key Lifetime**
* **Local ID** - the ID of the local peer

#### Phase 1 - Extended Authentication (XAuth)

Sometimes called Phase 1.5, XAuth forces remote users to authenticate with their credentials (Username and Password)

* **Type**: **PAP**, **CHAP**, **Auto Server** (protocol for XAuth is automatically detected), Disabled
* **User Group**:
  * **Inherit from policy** - based on the matching IPSec policy in the Firewall Policy
  * **Choose** - you can choose a group, but if you need more groups, then a new VPN needs to be created

### Phase 2&#x20;

Phase 2 negotiates the IPSec SAs over the secure channel established during Phase 1. ESP uses IPsec SAs to encrypt the traffic.

Phase 2 periodically renegociates the IPSec SAs to maintain security. If you enable PFS (Perfect Forward Secrecy), each time Phase 2 expires, FortiGate uses DH to recalculate new secret keys.&#x20;

#### Phase 2 - Selectors

In Phase 2, you must define the encryption domain (interesting traffic) of the IPSec tunnel. When you specify the encryption domain by indicating the network parameters, use:

* **Local Address and Remote Address**&#x20;
* **Protocol Number**
* **Local Port and Remote Port**

For Point-to-Point VPNs selectors must match so Source on one device becomes Destination on the other.

Because firewall policy is applied before traffic goes through the tunnel, the selector is applied after the firewall policy.

#### Phase 2 - Proposal

For each selector you must configure a proposal. This includes settings for **Encryption**, **Authentication**, **DH Group.**

Set **Auto-Negotiate** to prevent dropping interesting traffic by negotiating a new key before the old one expires.

## Firewall Policies

You need one firewall policy to accept traffic and bring the IPsec tunnel up. To allow both ends to initiate the connection then you need one policy for incoming and one policy for outgoing traffic. The interface used with the virtual tunnel interface (phase 1 name)

## Redundant VPNs

* **Partially redundant**: One peer (hub) has 2 ISP connections for backup and each VPN terminates on different physical port. On the other peer (spoke) there is a single ISP connection and both VPNs terminate on the same physical port.
* **Fully redundant**: both peers terminate their VPNs on different physical ports.

1. Create a Phase1 config for each VPN (primary and backup). Also, enable DPD on both ends.
2. Create at least a Phase2 config for each Phase1
3. Add at least one static route for each VPN. Routes for the primary VPN must have a lower distance than the backup or use dynamic routing protocols.

## Troubleshooting

In the GUI dashbaord, the widget allows you to forcefully bring down individual VPNs, a particular phase2 selector or all pahse2 selectors.

IPsec routes appear in the routing table:

* after Phase1 comes up - if the remote gateway is set to static IP or Dynamic DNS
* after Phase2 comes up - if the remote gateway is set to dial-up user.

