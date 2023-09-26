# Routing

## Route table

FortiGate maintains a RIB and a FIB.

| RIB                                 | FIB                      |
| ----------------------------------- | ------------------------ |
| visible in the GUI and the CLI      | only visible in the CLI  |
| `get router info routing-table all` | `get router info kernel` |

For each session, FortiGate performs two lookups: one for the first packet sent by the inititator, and then for the first packet sent by the responder. The routing information is added to the session table and subsequent packets in the session use this information. If there is a routing change that impacts the session, the routing information in the session table is reste and a new lookups will be performed.

In fortigate, the destination for a **static route** can be a subnet, an ISDB or a Named Address object of type Subnet or FQDN. An ISDB route is actually configured in the background as a Policy Route.

**RIP, OSPF, BGP and IS-IS** (CLI only) are supported as dynamic routing protocols.

**Policy routes** are a type of static routes that use not only the destination IP to route a packet, but other information as well (source IP, service, ports, ToS values, incoming interface). Policy routes are stored in a seprate routing table and take precedence over entries in the RIB. Policy routes have 2 possible actions:

* Forward traffic: Forward traffic according to the configured Gateway and interface
* Stop Policy routing: Skips all policy routes and uses the FIB

To see the list of policy routes, use `diagnose firewall proute list` command.

## Fortinet Route selection

* Most specific route
* if same route: Lowest distance
  * Higher distance routes are not installed in RIB, but they are installed in the routing table database
* if same distance: route that was learned last
* if same protocol: metric
* if same metric: priority
* if ECMP: multiple routes

The routing table database can be consulted using `get router info routing-table database` command.

## ECMP Load Balancing

When v4-ecmp-mode is used, load-balancing over equal cost paths is done using one of the following algorithms:

* Source IP
* Source-destination IP
* Weighted (static routes only) - based on interface weight
* Usage (spilover) - use one route until bandwidth threshold is reached, then start using the next route.

## SD-WAN

SD-WAN rules represent the routing policy for Fortinet's Secure SD-WAN solution. They are evaluated from top to bottom. An SD-WAN rule is actually a policy route that is used to steer traffic. You must also have corresponding firewall rules to allow the traffic.&#x20;

There is also an implicit rule that will be used for traffic that doesn't match any other rule.

When SD-WAN is enabled, the ECMP load-balancing algorithm changes from **v4-ecmp-mode** to **load-balance-mode**.&#x20;



| v4-ecmp-mode                                                                                         | load-balance-mode                                                    |
| ---------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| <ul><li>source IP</li><li>source-destination IP</li><li>weighted </li><li>usage (spilover)</li></ul> | volume algorithm                                                     |
| weight is defined in the route                                                                       | weight is defined in the SD-WAN member configuration                 |
| uses spillover thresholds defined on the interface                                                   | uses spillover thresholds defined in the SD-WAN member configuration |

Volume algorithm load balances sessions across members based on the measured interface volume and the member weight.

## RPF Check

RPF check is performed on the first packet of a new session. There are 2 RPF check modes:

* Feasible path (aka loose - default): FortiGate verifies that the routing table contains a route that matches the source address of the received packet and the incoming interface.  The matching route doesn't have to be the best route, though
* Strict: FortiGate verifies that the matching route is the best route in the routing table. If it isn't, then the RPF check fails.

The RPF mode is configurable:

```
config system settings
  set strict-src-check {disable|enable}
end
```

By default, FortiGate drops sessions where RPF check fails, but it can also be disabled per interface:

```
config system interface 
  edit INTERFACE
    set src-check disable
  next
next
```

## Link Health Monitor

This feature is used to detect dead links when the physical connection doesn't report an issue. For this reason, FortiGate can send probes to up to 4 destinations (beacons)

Supported protocols

* ICMP echo request (ping)
* TCP echo on port 7
* UDP echo on port 7
* HTTP GET
* TWAMP (Two Way Active Measurement Protocol) on TCP 862 (control session) and UDP 862 (test session)&#x20;

A link is marked as dead after 5 consecutive failed probes from all configured servers

A link is marked as alive after 5 consecutive failed probes from at least one server

| Action                   | Dead                                                                        | Alive                |
| ------------------------ | --------------------------------------------------------------------------- | -------------------- |
| Update Static Route      | Flag route as inactive --> static route is removed from routing table       | Flag route as active |
| Update Policy Route      | Disable policy route --> Policy route is skipped                            | Enable policy route  |
| Update Cascade Interface | Bring down **alert interface** --> traffic is move to a different interface | Bring up interface   |

The **alert interface** is configured as such on another interface. In this example port2 is set as the alert interface for prot1

```
config system interface
  edit port1
    set fail-detect enable
    set fail-detect-option detectserver
    set fail-alert-method link-down
    set fail-alert-interface "port2"
  next
end
```



