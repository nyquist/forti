# VDOMs

A VDOM splits a FortiGate into multiple logical devices and it can help splitting a security domain into multiple security domains.

Each VDOM has independent security policies and routing table. Traffic from one VDOM cannot go to a different VDOM by default.&#x20;

In a multi-vdom environment, one VDOM will have the Management VDOM role. By default, this is the **root** VDOM, but acutally any VDOM can be assigned as the Management VDOM. This designation means that traffic originated by the FortiGate will originate from the Management VDOM. Therefor, in order to be able to access FortiGuard services, the Management VDOM needs internet access.

To enable VDOMs, you can use GUI (System>Settings) or CLI:

```
config system global
  set vdom-mode {no-vdom|multi-vdom}
end
```

Enabling VDOMs does not reboot, but it logs out all active admin sessions. Traffic still runs through the device.

**super\_admin** profile (includes **admin** user by default) can configure and back up all VDOMs.&#x20;

By default, **root** VDOM acts as management VDOM, but it is a Traffic VDOM.&#x20;

After multi-VDOM mode is enabled, you can create new VDOMs and chose the VDOM type:

* **Admin VDOM** - used for management only and doesn't pass any data. There can be only one Admin VDOM.
* **Traffic VDOMs** - processes traffic and can have separate security policies

[NGFW mode ](../ngfw/inspection-and-ngfw-modes.md)is a per-VDOM setting, as well as the mode of operation: NAT mode or transaptent mode. This is configurable through CLI only:

```
config vfdom
edit VDOM_NAME
  config system settings
    set opmode {nat|transparent|
  end
```



| NAT mode                                                    | Transparent mode                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------------------ |
| Routes according to OSI Layer 3 (IP Addresses) as a router  | Forwards according to OSI Layer 2(MAC Addresses) as a transparent bridge |
| FortiGate interfaces have IP Addresses associated with them | FortiGate interfaces don't need to have an IP address to be in use       |

In transparent mode, by default all interfaces on a VDOM belong to the same broadcast domain, even if they have different VLAN IDs. That means that all interfaces will be used when broadcasting traffic.

You can assign a forward-domain to each interface through CLI to change this behavior.

| Global settings     | Per-VDOM Settings                       |
| ------------------- | --------------------------------------- |
| Hostname            | Operating mode (transparent, NAT)       |
| HA settings         | NGFW mode (profile-based, policy-based) |
| FortiGuard settings | Routes and network interfaces           |
| System time         | Firewall policies                       |
| Admin accounts      | Security profiles\*                     |

\*Security profiles can be created globally for use by multiple VDOMs. Some features (URL filters) are not available in global profiles. The name of a global profile starts with "g-".

Regardless of the context you are, you can use "sudo" to run diag commands in other VDOMs.&#x20;

## Inter-VDOM Links

In case of NAT-to-NAT inter-VDOM links, both sides of the link must be on the same IP subnet because they are point-to-pint network connections.

Inter-VDOM links cannot be created between L2 transparent mode VDOMs. This prevents potential L2 loops. But you can connect a transparent VDOM to a NAT mode VDOM.

Inter-VDOM links are created in the Global settings of the GUI.

FortiGate devices with NP4 or NP6 processors include inter-VDOM links that can be used to accelerated inter-VDOM traffic (npu0\_vlink0, npu0\_vlink1, npu1\_vlink0, npu1\_vlink1)
