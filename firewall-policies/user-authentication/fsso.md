# FSSO

FSSO is a software agent that enables FortiGate to identify network users without asking for username and password.

When a user logs in a directory service, FSSO agent sends FortiGate the IP it connected from and the list of groups that the us4er belongs to.

FSSO works with Microsoft AD and Novel eDirectory.

## Microsoft AD

{% tabs %}
{% tab title="Domain Controller Agent Mode" %}
* most scalable mode
* Requires:
  * one DC Agent (dcagent.dll) installed on each Windows DC in the `Windows\system32` directory. The DC Agent monitors user login events and forwards them to the collector agents on UDP 8002 (default). It also handles DNS lookups for the client (by default, but can be disabled to avoid double DNS resolution)
  * one collector agent installed on a Windwos server that is a member of the domain. It consolidates events received from DC agents and sends them to the FortiGate. The collector agent also performs a DNS lookup to see if IP address changed. Collector sends username, hostname, IP Address and User group to the FortiGate using TCP 8000(default) The collector must be added to the fortigate in the Security Fabirc > External Connectors> FSSO Agent on Windows AD menu item
{% endtab %}

{% tab title="Collector Agent Polling mode" %}
In this mode, we only have a collector agent installed on a Windows server that is a member of the domain. No agent is needed on the DC. The collector agent uses SMB TCP port 445 to periodically request event logs from the Domain Controller. The information is parsed and sent to the FortiGate. It can also use TCP 135, TCP 139, UDP 137. There are 3 methods to collect data:

* **WMI** - DC returns all requested login every 3 secords (depends on number of servers and latency) - Reduced network load vs WinSec methods
* **WinSecLog** - Polls every 10 seconds and expects a fast performing network. It is slower, but sees all login events but parses known event IDs only
* **NetAPI** - Polls every 9 seconds for the Autehtnication session table in RAM (NetSessionEnum). It is faster but may miss some login events Collector Agent should be able to poll workstations to see if the user is still logged in. This is done using TCP port 445 or 139. The collector must be added to the fortigate in the Security Fabirc > External Connectors> FSSO Agent on Windows AD menu item
{% endtab %}

{% tab title="Agentless Polling mode" %}
* Similar to Collect Agent Polling mode but it is the FortiGate that polls instead. It supports only the WinSecLog method only. Workstation verification is not availalbe in this mode.
* This is added from the **Security Fabric > External Connectors > Poll AD Server**. FortiGate used LDAP to query AD to retrive user group information.
{% endtab %}
{% endtabs %}

There is also an option to have an agent in Terminal server/Citrix Agent Mode where multiple users share the same IP

## Novel eDirectory

eDirectory Agent Mode - uses either Novell API or LDAP

## Debugging:&#x20;

* `diagnose debug authd fsso list`  - disalplays currenlty logged in FSSO users
* `execute fsso refresh` - triggers a refresh fo user gorup information
* `diagnose debug authd fsso server-status` - shows status of fsso collector agent communication
