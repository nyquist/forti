# Security Fabric

Fortinet Security Fabric is created with:

* Core
  * Minimum 2x FortiGates (one will become the root)
  * at least one of FortiAnalyzer, FortiAnalyzer Cloud or FortiGate Cloud
* Recommended
  * FortiManager, FortiAP, FortiSwitch, FortiClient, FortiClient EMS, FortiSandbox, FortiMail, FortiWeb, FortiNDR, FortiDeceptor
* Extended:
  * other products (Fortinet or 3rd party) that can use th Fabric's APIs

To **setup a Security Fabric**, select the intended root device and enable "Security Fabric Connection" on the interfaces towards other Fortinet devices. Then enable "Security Fabric" connector and select "Serve as Fabric Root". FortiAnalyzer can be configured before or after.&#x20;

On downstream devices, enable Security Fabric Connection on the interface towards the root and then select "Join Existing Fabric" and specify the IP of the root device.

Back on the root FortiGate, the downstream devices should be authorized to join the Fabric.

By default, **object synchronization** is enabled in fabric settings. This feature can be configured on the root using `set fabric-object-unification {default|local}` command. Using local disables object synchronization. This feature can also be disabled on the downstream devices if we use `set configuration-sync local`. Both of these settings are inside the `config system csf` CLI item.

On the root, each object can also be included or excluded from object synchronization.

FortiGate members of the Fabric will detect directly connected devices either agentless or anget-based(ForitClient). These devices are also shown in the topology.

Automated workflows (stitches) are event-based responses that FortiGates can run. With a Security Fabric, the stitches can be created on the root device only, but can cover any device in the Fabric.

