# Fortinet Basics

## Default Settings&#x20;

* Default IP: 192.168.1.99/24
  * On entry-level boxes, the default IP is configured on the "Internal" interface or "Port 1". DHCP Server is also enabled
  * On mid-range and high-end boxes, the default IP is configured on the "MGMT" interface
* Default User: "admin"
* Default Password: ""

Console port gives direct access to the device

## Foritguard Subscription Services

Fortiguard Subscription Services are provided by Fortiguard Distribution Network.&#x20;

* Fortigate asks FDN everytime it performs webfiltering, DNS filtering or spam checking.&#x20;
* Fortigate downloads antivirus and IPS data regularly (once a day)

Fortigate can access Fortiguard services using:

* UDP (53, 8888) to service.fortiguard.net
* HTTPS (53,443, 8888) to securefw.fortiguard.net

When Fortigates used Fortiguard for DNS request, it defaults to using DoT (DNS over TLS)

All Fortiguard services use 3rd party SSL certificate check and OCSP stapling check.

Fortiguard services by default use anycast.&#x20;

## Basics

**VDOMs** (Virtual Domains) is a feature that allows the partition of a physical firewall into multiple virtual firewalls with their own security policies, routing tables, interfaces. By default, FortiGate supports 10 VDOMs. Higher-end models can be licensed for additional VDOMs.

**Interface Roles** are used to limit the configuring options in GUI based on the role of the interrface. The "Undefined" option shows all settings.

FortiGate adminstration can be performed via CLI or GUI. Users have associated **user profiles** that contain the permisions.&#x20;

* "super-admin" profile is used by the default "admin" user and gives full access to everything. It can't be deleted.
* "prof-admin" profile also provides full access, but only to the VDOMs

Resetting a lost admin password can be performed when there is physical access to the device, using the "maintainer" account which can be used only for the first 60 seconds after boot (varies by model). The password is "bcpb\<SERIAL-NUMBER>".

### Configuration Backups

Configurations can be backed up from GUI using default format. When encryption is enabled, the entire file is encrypted with a password that is provided when it is created. The file can only be restored on a device with the same model and OS version.

When encryption is not enabled, the sensitive information (passwords, preshared-keys) are hashed in order to be obfuscated. When restoring from a non-encrypted backup, only the model must match.

Config can be backed up to a remote location using default fortigate format or the yaml format:

```
execute backup yaml-config {ftp|tftp} FILENAME SERVER [USERNAME [PASSWORD]]
execute restore yaml-config {ftp|tftp} FILENAME SERVER [USERANME [PASSWORD]]
```

