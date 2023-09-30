# Firewall 101

A Firewall Policy is a set of rules that define which traffic matches each rule and how the matching traffic is processed. The rules are processed from top to bottom and the traffic is processed by the first matching rule. If no match is found, the traffic is dropped by the "Implicit deny" rule.&#x20;

To see what matches a rule, the firewall looks at:

* **incoming interface**
* **outgoing interface**
* **source**
  * Address Object
  * User
  * ISDB
* **destination**
  * Address Object
  * ISDB
* **service**
* **schedule**

The action for each rule is either:

* **DENY**: session is dropped
* **ACCEPT**: allows the session and if configured enables additional policies to be processed (authentication, NAT, L7 filtering)

**Users** can authenticate&#x20;

* localy - authentication is performed on the FortiGate, user is presented with a user and password prompt, when supported.
* remotely (LDAP, RADIUS) - authentication is performed is verified on the authentication server.  User is presented with a user and password prompt when supported.
* remotely through FSSO (Fortinet Single Sign-On) where the access is granted based on the domain contgroller group information. User is not presented with a user and password prompt.

An **Address Object** can be:

* **IP Address**
* **Subnet**
* **IP Range**
* **FQDN**
* **Geography** - a list of IP Addresses, maintained by Fortiguard which are allocated to locations on the globe, based on Region, Country, City
* **ISDB** - is a list of IP Addresses, maintained by FortiGuard for well known internet services (AWS, Amazon, Google, Facebook)
  * **Geographic ISDB** is a subset of ISDB based on Region, Country, City
* **Dynamic** (Fabric Connector Address)
* **MAC Address Range** (for source only)

Schedule can be:

* recurring
* one-time: has a specified start and end date.

**Services** are Protocols (TCP, UDP, ICMP, other) and port combinations (when it applies).

A UUID is created for each firewall policy and firewall object at creation time for easier tracking.  Policy ID is a number value which is assigned at system creation. They are not displayed by default in GUI but it is the identifier used to modify the policy in the CLI. &#x20;

### Session timeout

[https://community.fortinet.com/t5/FortiGate/Technical-Tip-Session-timeout-settings/ta-p/191228](https://community.fortinet.com/t5/FortiGate/Technical-Tip-Session-timeout-settings/ta-p/191228)

### Logging

A log is created when a sesssion is closed. You can additional add a log message when a session starts.&#x20;

During a session, if a security profile detects a violation, fortigate will log it immediately.



