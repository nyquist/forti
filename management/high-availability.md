# High Availability

Two to four FortiGate devices can operate in a cluster for redundancy and performance purposes.

A cluster includes one device that acts as the primary(active) FortiGate. The primary synchronizes to the secondary (standby) devices:

* configuration (some settings are not synchronized: hostname, HA priority, HA override settings)
* FIB entries
* DHCP leases
* ARP table
* FortiGuard definitions
* IPsec tunnel SAs
* Sessions (must be enabled)
* session information, FIB entries, FortiGuard definitions and other operation-related information&#x20;

The cluster share one or more heartbeat interfaces, also known as members for synchronizing data and monitoring the health of each member.



{% tabs %}
{% tab title="Active-Passive HA" %}
The primary FortiGate is the onlty FortiGate that actively processes traffic. Secondary FortiGate remains in passive mode, monitoring the status of the primary device.

If a problem is detected on the primary device, the secondary device takes over the primary role (HA failover)
{% endtab %}

{% tab title="Active-Active HA" %}
Operation is similar to the Active-Passive mode, with the main difference that all cluster members can process traffic. The primary fortigate can distribute sessions to secondary devices.
{% endtab %}
{% endtabs %}

## FGCP - FortiGate Clustering Protocol

FortiGate HA uses FGCP to discover members, elect the primary FortiGate, sync data between members and monitor their health.&#x20;

Each member broadcasts heartbeat packets over all configured heartbeat interfaces.

In NAT mode, hearbeat frames are type 8890. In transparent mode, heartbeat frames are type 8891. If the cluster operates in active-active mode, the first packet of a session distributed to the secondary is encapsulated in 8891 type Ethernet frames.

Data synchronization (TCP or UDP 703), local CLI management (SSH) and logging (TCP 700) is exchanged using frame type 8893.

Data synchronization includes:

* configuration (some settings are not synchronized)
* FIB entries
* ARP table
* FortiGuard&#x20;

### Cluster setup

When setting up an HA cluster, all members must have the same:

* firmware version
* model
* license (if different, the lowest level license is used)
* hard drive configuration
* operating mode (VDOMs)
* configuration for:
  * group ID
  * group name
  * password
  * heartbeat interface settings
* heartbeat interfaces are connected (in the same broadcast domain for more than 2 devices)

Asa a best practice, there should be at least 2 heartbeat interfaces for redundancy purposes

### Failover

Failover can be configured to take place when:

* Dead member
* failed link
* failed remote link (detected through health monitor)
* high memory usage
* failed SSD
* admin-triggered

On the primary, each interface is assigned a virtual MAC address (except heartbeat interfaces). Upon failover, the newly elected primary adopts the same virtual MAC address as the former primary. To avoid conflicts, the group ID is used to determine the MAC address.&#x20;

The new primary broadcasts Gratuitous ARP packets notifying the network that each virtual MAC address is now reachable on a different switch port.

#### in Active-Active mode

By default, only sessions that are subject to proxy inspection are distributed to the secondary HAs. This behavior can be changed in CLI. The traffic flow is different when only proxy inspected sessions are distributed vs when all sessions are distributed.

### Election

{% tabs %}
{% tab title="override disabled" %}
this is the deafult mode

1. Count of connected monitored ports. Greater wins
2. HA uptime. Greater wins
3. Prioirty. Greater wins
4. Serial Number. Greater wins

Since HA uptime is more imporant than priority, the HA uptime can be used to trigger a manual failver. This can be done with `diagnose sys ha reset-uptime` command which restes the HA uptime to 0.

The HA uptime is also reset if an interface fails or a member reboots. You can verify the HA uptime using `diagnose sys ha dump-by vcluster` command.
{% endtab %}

{% tab title="override enabled" %}
In this mode, the priority is checked before HA uptime

To enable override, use:

```
config system ha
  set override enable
end
```

This means that you can specify which device is preferred primary as long as it is up and running, by configuring it with the highest HA value.  The disadvantage is that a failover is triggered not only when the primary fails, but also when it is available again.

1. Count of connected monitored ports. Greater wins
2. HA uptime. Greater wins
3. Prioirty. Greater wins
4. Serial Number. Greater wins

To force a failover, change the priority.
{% endtab %}
{% endtabs %}

### Heartbeat interfaces

The cluster assigns addresses to the heartbeat interfaces based on the serial number of each member, starting from the highest to lowest in order from the range 169.254.0.1 - N

IP Address assignment may change only if a member leaves or joins the cluster, but not if thei role changes.

### Config sync

When a new member joins the cluster, the primary FortiGate compares its config checksum with the secondary's config checksum. If they don't match, the FortiGate uploads its complete configuration to the secondary FortiGate.

After this, changes to the primary Fortigate are propagated to the other devices using incremental synchronization.

Periodically, HA checks for synchronization and if the checksum doesn't match after 5 attempts, the secondary downloads all config from the primary.

### Session sync

```
config system has
  set session-pickup enable
  ! for non proxied TCP
  set session-pickup-connectionless enable
  ! for UDP and ICMP
  set multicast-ttl <5-3600>
  ! time multicast routes remain in multicast forwarding table after failover. Default 600 sec
```

TCP firewall sessions are synchronized by default, except those that are subject to proxy inspection! (except for SIP sessions inspected by SIP ALG, which are synchronized)

Synchronization of UDP and ICMP sessions can also be enabled.&#x20;

Multicast sessions are not synchronized, only multicast routes.

Local-in and local-out sessions are not synchronized. For this reason, BGP peerings, OSPF adjacencies and SSH/HTTP sessions must be restarted fater a failover.

IPSec IKE and IPSec SAs are synchronized (tunnels continues to be up after failover)

SSLVPN web mode authenitcation info is synchronized (users don't have to re-authenticate but must restart connections)

Tunnel mode SSL VPN is not synchronized and SSL VPN tunnels must be restarted after failover

## Virtual Clustering

Virtual clusters allow you to have one device acting as the primary for one VDOM, and as the secondary for another VDOM. With Virtual clustering, only 2 devices of the cluster can process traffic.

## Full Mesh HA

For improved availability, you can use MCLAG(Multi Chassis Link Aggregation) switches to connect each member of the HA to each switch.

## Troubleshooting

```
get system ha status
diagnose sys ha checksum show
diagnose sys ha checksum cluster
```

Using the CLI you can connect to any CLI member

```
execute ha manage MEMBER_ID ADMIN_USER
```

To force a permanent secondary role on the primary, use:

```
execute ha failover {set|unset}
execute ha failover status
```

### Firmware Upgrade

In an HA cluster, uniterruptible upgrade mode is enabled by default. The upgrade process is as follows:

1. The primary sends the firmware to all secondary members using the heartbeat interface.
2. The secondary devices upgrade their firmware first
3. The first secondary that finishes the upgrade takes over the cluster
4. The former primary becomes secondary and upgrades the firmware

Unitterruptible upgrade can be disabled to speed up the process but this will have impact on the traffic.
