# FortiManager

## Default settings

IP: 192.168.1.99/24 on port1. ping, HTTP, HTTPS and SSH are enabled.

username: admin, blank password

## ADOMs

FortiManager makes use of ADOMs to logically separate groups of managed FortiGate devices.

* Global ADOM layer
  * holds th Global Object Database
  * holds Header and Footer Policies
* ADOM layer
  * Holds ADOM Object Databse
  * holds ADOM Policies
* Device Manager layer
  * Device specific configurations

ADOMs are not enabled by default. They can be enabled via GUI or CLI

ADOM Modes:

* Normal: provides full access to make changes from the FortiManager&#x20;
* Backup: backs up config changes made directly on the device. ADOM is read only, but scripts can still be used to make changes to devices (because they connect directly to the device)

ADOM Device Modes:

* Normal: A FortiGate device can be added to a single ADOM only
* Advanced: Each VDOM can be assigned to a different ADOM

Admin Profiles

* System Admin
* Restricted Admin: Can have access only yo Web filter Profile, IPS Sensor and Application Sensor

Admin operation

* Workspace Mode (default disabled): only one admin at a time can make changes. The lock can be per ADOM, per device or per policy. The admin must lock/unlock the worksapce.



## Log Collection

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

## FortiManager features

* **API**: JSON API (JSON-RPC) and (?) REST API
* **Meta Fields** - allows admins to add extra information for FortiGate units
* **Backups and Restores**: can be done via GUI or CLI (scheduled)
  * it can also be used to migrate to a different FortiManager
* **Offline mode**: disables communication with FortiGates. Used to load a backup or to make changes without affecting the managed devices
* **Provisioning Templates**
* **Device Registration:**
  * From FMG, using the wizard. Supports OAUTH or "legacy" login methods
  * From FMG, using the wizard for offlince device: Add Model Device. Can link device by SN or PSK. Requires FMG IP to be defined on FortiGate
  * From FortiGate, via registration requests: Requests appear
  * A VDOM counts as one device. A FortiGate HA cluster counts as one device.&#x20;
* **Scripting**:
  * CLI
  * TCL - must be enabled via CLI
  * can run on:
    * device database
    * policy packages
    * Remote FG directly
* **Managed devices status:**
  * Synchronized/Auto Update
  * Modified: Modified on FMG, not pushed to device
  * Out of Sync: Modified on FM, not synced with FMG
  * Conflict: Changes both on FMG and FM(not synced)
  * Unknown
* **Config Installation**
  * Install wizard
  * Quick install - no preview option
* **Config Revisions**
  * Reverting to a previous revision will revert the device database to the previous version. Then an install is needed

