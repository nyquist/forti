# ZTNA

Modes of operation

* **ZTNA Access Proxy** allows users to securily access resources through SSL-encrypted access proxy
* **IP/MAC fitering** uses ZTNA tags to provide identification factors and security posture to implement role-based zero-trust access.

ZTNA works in conjunction with **FortiClient EMS** (Enterprise Management System) which shares tag information with FortiGate using Security Fabric. FortiGate then uses ZTNA tags to enforce access control rules.

ForitClient provides endpoint infromation and obtains client certificate from FortiClient EMS FortiClient EMS issues and signs client ceritificates and shares it's ZTNA CA certificate with FortiGate so that FortiGate can use it to authentication the clients. It uses tagging rules to tag the clients FortiGate syncs with FortiClient EMS and the processes ZTNA traffic.&#x20;

The client certificate is used when accessing resources protected by the access proxy. Currently ZTNA support in the client works only in Microsoft Edge and Google Chrome browsers.

The access proxy works as a reverse proxy for HTTP servers. The ZTNA server defines the access proxy VIP and the real servers that clients connect to.&#x20;

A ZTNA rule is a proxy policy used to enforce access control using ZTNA tags or tag groups.&#x20;

ZTNA supports basic HTTP and SAML authtentication methods.&#x20;

You can also configure TCP Forwarding Access Proxy (TFAP) to forward TCP traffic to protected resources or SSH Access proxy IP/MAC access control can be used for clients connected to the corporate network that do not have FortiClient. Whenver the tags of a device change, FortiGate is update by EMS and then it revealuates the access.
