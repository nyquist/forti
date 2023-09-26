# IPS/IDS

IPS is used to detect:

* **Exploits** are known, confirmed attacks and are detected with IPS/WAF/Antivirus signatures
* **Anomalies** are potential zero-day or DoS attacks and are best detected with behavioral analisys (rate-based IPS signatures, DoS policies, protocol constraints inspection)

The IPS engine is used for:

* Application Control
* Flow-based AntiVirus
* Flow-based WebFilter
* Flow-based EmailFilter

IPS Packages are updated by FortiGuard. These updates cover:

* IPS signature database
* Protocol decoders
* IPS engine

IPS updates are based on a FortiGuard subscription. The database can be:

* regular (default)
* extended (impacts performance)

Models with hardware acceleratio can benefit from it for IPS engine.

IPS goes into fail-open mode when there is not enough available memory in the IPS socket buffer for new packets. You can disable fail-open and then new packets are dropped.

## IPS Profile

You can add signatures to the IPS Profile (Sensor) individually or using a filter that adds all signatures matching the filter. The engine evaluates signatures from top to bottom.

You can also exempt IPs from specific sigantures to bypass false-positives

Actions available are:

* **Allow**: Allow traffic to continue to its destination
* **Monitor**: Same as allow, but also Log
* **Block**: Silently drop traffic matching the signature
* **Reset**: Generates a TCP RST packet when the signature is triggered
* **Default**: Sets it to the default setting as defined by FortiGuard
* **Quarantine**: allows you to quarantine the attacker's IP for a set duration.

### Botnet Protection

Botnet Protection relies on a list of IP Addresses that are provided by FortiGuard via the ISDB updates. You can set Botnet Protection to:

* **Disable**
* **Block**
* **Monitor**



