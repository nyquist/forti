# SNAT

SNAT means the source address is translated. When translated the source, you can use the outgoing interface address or an IP pool.

## Outgoing Interface IP

With this option, the source address will be translated to the IP address of the outgoing interface. If there are multiple internal sources (which usually happens), PAT (Port Address Translation) is used. This means each session gets allocated a different source port so that the same source IP can be shared between multiple internal hosts.

## Dynamic IP Pool

When using a dynamic IP Pool, the translated source IP will be selected from the available addresses in the pool. Make sure that proper routing is configured so that traffic properly reach the firewall. When creating the IP Pool, you can define the type and hence the method of allocating resources from the pool.

### Overload

With a pool of type **overload**, PAT is used. Each session gets its source IP and port translated. Then the same IP, with a different port can be used for another session.

### One-to-one

With a pool of type **one-to-one**, PAT is not used. Each internal host gets its source IP translated to one from the pool on a first come, frist served basis until the pool is depleted. Subsequent attempts from other hosts will fail until IP addresses are released back into the pool.

### Fixed-port-range

With a pool of type **fixed-port-range**, the adminstrator assigns a list of source IP address to be translated into a list of destination IP addresses. The FortiGate will then automatically assign fixed ranges of ports (blocks) for each source on a first-come first-served basis. The advantage to using this method instead of "overload" is that it makes the tracking of the original host much easier since each source has a known list of ports that will be used. To help tracking the FortiGate generates a system event log when an allocation from the pool is performed.&#x20;

You can get more infromation about the allocation using these commands:

```
diagnose firewall ippool list
diagnose firewall ippool-fixed-range list natip NAT_IP [PORT]
```

### Port-block-allocation

With a pool of type **port-block-allocation**, the administrator provides the external IP range along with block size and number of blocks that can be allocated to each internal host.  This limits the number of blocks and ports that a source can use and prevents a few hosts from exhausting the pool.&#x20;

## Central SNAT

In Central SNAT mode, NAT config moves outside of the policy. The set of NAT rules are processed from top to bottom. A firewall rule must allow the traffic before it can be processed by Central SNAT

## Troubleshooting

```
diagnose firewall ippool-all list
diagnose firewall ippool-all stats [IP_POOL]
```
