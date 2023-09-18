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

Fortiguard services by default use anycast.

