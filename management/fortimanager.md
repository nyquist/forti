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

### ADOM Modes

* Normal: provides full access to make changes from the FortiManager&#x20;
* Backup: backs up config changes made directly on the device. ADOM is read only, but scripts can still be used to make changes to devices (because they connect directly to the device)

### ADOM Device Modes

* Normal: A FortiGate device can be added to a single ADOM only
* Advanced: Each VDOM can be assigned to a different ADOM

### Admin Profiles

* System Admin
* Restricted Admin: Can have access only yo Web filter Profile, IPS Sensor and Application Sensor

### Workspace mode

* disabled (default): admins can make changes without restrictions as long as they have access.
* Workspace Mode (workspace mode: normal): only one admin at a time can make changes. The lock can be per ADOM, per device or per policy. The admin must lock/unlock the worksapce.
* Workflow Mode (workspace mode: workflow): Admins must create sessions to change the policy. Another admin must approve before the change can be installed on a device.&#x20;

### ADOM Revisions

ADOM Revisions create a snapshot of all policy and objects configurations for the ADOM. Revisions can be locked to prevent deletion

## FortiManager System

### FortiManager Ports

{% tabs %}
{% tab title="Destination (FMG initiates)" %}
* FM to FG: TCP 541 (IPv4), TCP 542 (IPv6)
* Firmware Image Download: TCP 443
* FM to FDN (FortiGuard): TCP 443
* DNS, syslog&#x20;
{% endtab %}

{% tab title="Listening Ports (FMG listens)" %}
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

### FortiManager HA

There is one primary and multiple (up to 4) secondary FMGs. Syncing is done using TSL 1.0 over port TCP 5199. Syncing covers device configuration and configuration revisions, but not logs, FortiGuard data or local config settings (IP addresses, routes, HA, SNMP, etc)

When primary FMG is upgraded, the secondary FMGs are upgraded as well.

Failure can be manual (a secondary devices is manually promoted to primary) or automatic (using VRRP)

## FortiManager Features

* **API**: JSON API (JSON-RPC) and (?) REST API
* **Meta Fields** - allows admins to add extra information for FortiGate units
* **Backups and Restores**: can be done via GUI or CLI (scheduled)
  * it can also be used to migrate to a different FortiManager
* **Offline mode**: disables communication with FortiGates. Used to load a backup or to make changes without affecting the managed devices
* **Provisioning Templates**
* **Scripting**:
  * CLI
  * TCL - must be enabled via CLI. These scripts don't go through FGFM tunnel, but rather through SSH
  * can run on:
    * device database
    * policy packages
    * Remote FG directly
* **Log Collection:** FortiManager can act as a log collector for FortiGate devices, but it has less functionality and capacity than&#x20;
* **FortiAnalyzer**: FortiManager can manage FortiAnalyzer and provides a unified view to the users

### **Device Registration**

* From FMG, using the wizard. Supports OAUTH or "legacy" login methods
* From FMG, using the wizard for offlince device: Add Model Device. Can link device by SN or PSK. Requires FMG IP to be defined on FortiGate
* From FortiGate, via registration requests: Requests have to be approved in FMG
* A keepalive is sent between FortiGate and FortiManager which includes the config checksum. Initially login credentials are required for initial discovery, or when reclaiming the tunnel interface, but then the SN becomes the  base of the authentication.
* FGFM uses TCP 541 for IPv4 and uses 169.254.0.0/16 to assign IP addresses to the managed devices. 169.254.0.1 is reserved to the FortiManager.
* A VDOM counts as one device. A FortiGate HA cluster counts as one device.&#x20;
* **FortiManager behind a NAT device**: only FortiManager can discover the device. It is recommended to setup the NAT IP as the FMG IP on the device
* **FortiGate behind a NAT device**: FortiManager can discover the device through the NAT IP but it won't try to reestablish connection automatically if it is disconnected
* **Both devices behind NAT**: Similar to FortiManager behind NAT

### Configuration Management

* **Managed devices configuration:**
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
* **Recovery logic**:
  * FortiManager sends both set and unset commands to FortiGate. If connection to FortiManager fails, FortiGate applies the unset commands after 15 minutes.
  * If the configuration still fails after unsetting, then the FortiGate can reboot (can be enabled in FMG CLI) to recover the previous working configuration

### Policy Management

* Each ADOM has a common object database
* Each policy has an instalation target which defines the list of devices it applies to. There is also the option to define the target per policy entry so that a policy package can be shared among multiple devices.
* Dynamic Objects give the possibility to map one logical object to a unique definition per device
* Normalized interfaces give the possibility to map logical interface names to physical interfaces of the device
* Policy Check can provide recommandations for improveming the policy
* Policy Package status:
  * Imported
  * Synchronized
  * Never installed
  * Modified
  * Out of Sync
  * Conflict
  * Unknown
* Policy Package can be installed through install wizard. Re-install option will still give the preview option.

### FortiGuard Services

FortiManager can act as a local FortiGuard Cache



