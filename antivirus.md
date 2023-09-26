# Antivirus

FortiGate uses the following techiniques to detect viruses, and uses them in this order:

1. Antivirus scan: detects viruses based on exact signature match
2. Grayware scan: Not techinically a virus, but categorized as malware (PUP/PUA scanning)
3. ML/AI scan: probability based scanning. Can prevent zero-day attacks but also increase the false positives.

## Sandboxing

Another option is to use Sandboxing. This feature givces the possibility to upload files to a sandbox VM where the software is examined for malware detection. There are 2 types of sandobxing:

* FortiGate cloud: requires an active FortiCloud account
* FortiSandbox cloud: requires an entitlement license embedded to FortiGate

In addition, FortiGate can be configured to receive a signature database from FortiSandbox Cloud or a FortiSandbox appliance.

FortiGate decides which files should be sent to the FortiSandbox based on information received from FortiGuard, but inline scanning is only available in Proxy-mode inspection.

## Antivirus Databse

In the system settings you can configure the schedule for downloading antivurs update. Alternatively you can manually upload virus definitions after they have been downloaded from Fortinet support website.

If you want to enable **PUP/PUA (Potentially Unwanted Program/Application) scanning - Grayware scanning**, there is also an option in the System>FortiGuard settings.

There are multiple antivirus **signature databases**, congurable through CLI. All FortiGate devices include the extended database which is the default database that can be used.  On some models you can enable the **extreme** database which is suitable for high-security environments.&#x20;

## Antivirus Security Profile

In the security profile you can define if antivirus will work in flow-based or proxy-based mode.



{% tabs %}
{% tab title="Proxy Inspection Mode" %}
In proxy-based mode there are 2 optsion:

* default scanning: enhances scanning of nested archive files without buffering the container archive file
* legacy scanning: the full container is buffered and then scanned.

Proxy mode Inspection for the Antivirus profile can only be used if the proxy-mode is set on the policy as well.

As opposed to Flow-Inspection mode, in proxy-mode antivirus scanning cannot be hardware accelerated
{% endtab %}

{% tab title="Flow Inspection Mode" %}
In flow-based mode, a combination of the default and legacy scanning is used.

In flow-based mode, the IPS engine reads the content of each packet, caches a local copy and forwards the packet to the receiver. When the last packet is received, the packet is put on hold and a copy is sent to the IPS engine. With all packets received, the IPS engine extracts the payload and assembles the whole file which is then sent to the AV engine for scanning.

* If a virus is detected after some packets have already been sent to the user, FortiGate res
*
* ets the connection without sending the last packet.
  * The URL of the infected file is also cached and another attempt to download it will be blocked and a replacement message is presented to the client
* If the virus is detected before sending packets to the user, then the user is presented with a replacement message directly
* If no virus is detected, the last packet will also be sent to the user to complete the transimission.

If the file size is bigger than the buffer size, it won't be scanned, but a log can be enabled for these files. The buffer size can also be changed. It defaults to 10MB. There is a separate setting for the size of uncompressed files. Higher values increase memory usage.

As opposed to Proxy-Inspection mode, in flow-based mode, antivirus scanning can be hardware accelerated
{% endtab %}
{% endtabs %}







In the security profile you have the option to enable **CDR (Content Disarm and Reconstruction)** which allows FortiGate to replace unsafe or malicious content before it is forwared.  This is supported only in Proxy mode.

**FortiGuard Virus Outbreak Prevention** database is an additionally licensed feature that provides protection before a virus signature becomes avaialble on FortiGuard.

**Malware block list** is a feature that allows FortiGate to enhance it's database by linking to dynamic external lists.



&#x20;











