# Diagnostics

## Basic commands

```
get system status 
get hardware nic INTERFACE 
get system arp
execute ping {IP|HOSTNAME}
execute ping6 ...
execute traceroute {IP|HOSTNAME}
execute ping-options ...
```

## Debug Flow

1. Define a filter
   1. `diagnose debug flow filter FILTER_CONFIG`
2. Enable debug output&#x20;
   1. `diagnose debug enable`
3. Start the trace
   1. `diagnose debug flow trace start REPEAT`
4. Stop the trace
   1. `diagnose debug flow trace stop`

Debug flow can also be used in GUI: **Network>Diagnostics>Debug Flow**. After debug stops, you can also download a packet trace output in CSV format.

[https://docs.fortinet.com/document/fortigate/6.2.3/cookbook/54688/debugging-the-packet-flow](https://docs.fortinet.com/document/fortigate/6.2.3/cookbook/54688/debugging-the-packet-flow)

### Slowness

It is likely caused by high CPU or memory usage.&#x20;

`get system performace status diagnose sys top 1`

* to sort by high CPU, use Shift+P
* to sort by high MEM, use Shift+M

### Memory conserve mode

* FortiOS protects itself when memory usage is high.
  * Green: 82% - Threhsold at which FortiGate exists Conserve mode
  * Red: 88%: Threshold at which FortiGate enters Conserve mode
  * Extreme: 95%: Threhsold at which new sessions are dropped

```
config system global
 set memory-use-threshold-red PERCENTAGE
 set memory-use-threshold-extreme PERCENTAGE
 set memory-use-threshold-green PERCENTAGE
end
```

What happens in Conserve mode?

* System configuration cannot be changed
* FortiGate skips quarantine action (including FortiSandbox analysis)
* IPS engine goes is considered failed
  * if fail-open is enabled: packets are allowed without IPS scanning&#x20;
  * if fail-open is disabled: packets are dropped without IPS scanning

```
config ips global
 set fail-open {enable|disable}
end
```

* for traffic that requires proxy-based inspection (and if memory usage has not exceeded the extreme threshold yet the beahvior can be changed with the `set-av-failopen` setting
  * **off**: All new sessions with content scanning enabled are dropped
  * **pass**(default): All new sessions are allowed without content scanning
  * **one-shot**: Similar to pass, but it will remain in this state even after leaving conserve mode. It must be manually changed or restart the device
  * This setting also applies to flow-based AV inspection
* When the limit of sockets to process proxy-based traffic is reache, the behavior is controlled by the `av-failopen-session` setting.
  * **enable**: Sessions are allowed
  * **disable**: Sessions that require proxy-based inspection are blocked

```
config system global
 set av-failopen {off|pass|one-shot}
 set av-failopen-session {enable|disable}
end
```

To see status of conserve mode, use: `diagnose hardware sysinfo conserve`
