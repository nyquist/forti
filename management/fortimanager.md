# FortiManager

## Default settings

IP: 192.168.1.99/24 on port1. ping, HTTP, HTTPS and SSH are enabled.

username: admin, blank password

## Basics

FortiManager makes use of ADOMs to logically separate groups of managed FortiGate devices.

* Global ADOM layer
  * holds th Global Object Database
  * holds Header and Footer Policies
* ADOM layer
  * Holds ADOM Object Databse
  * holds ADOM Policies
* Device Manager layer
  * Device specific configurations

FortiManager can act as a log collector for FortiGate devices, but it has less functionality and capacity than FortiAnalyzer. FortiManager can manage FortiAnalyzer and provides a unified view to the users.

## FortiManager Ports



{% tabs %}
{% tab title="Destination Ports" %}
* FM to FG: TCP 541 (IPv4), TCP 542 (IPv6)
* Firmware Image Download: TCP 443
* FM to FDN (FortiGuard): TCP 443
* DNS, syslog&#x20;
{% endtab %}

{% tab title="Listening Ports" %}
* FDN to FM for AV and IPS update push: UDP 9443
* FG to FM&#x20;
  * for FortiGuard AV and IPS update request: TCP 8890
  * for FortiGuard antispam and web filtering rate lookup: UDP 53 or UDP 8888
* FortiClient to FM&#x20;
  * for antivirus and IPS updates: TCP 80
  * FortiGuard antispam and web filtering rate lookup: UDP 53 or UDP 8888
* XMLAPI: TCP 8080
* GUI and JSON API: TCP 443
* FM to FM&#x20;
  * for heartbeat in a cluster: TCP 5199
  * in cascade mode: TCP 8891, 8900, 8901
{% endtab %}
{% endtabs %}



