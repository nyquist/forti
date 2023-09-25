# Application Control

Application control allow the detection fo applications like Facebook, Skype, Gmail, etc. Application control uses the IPS engine which uses flow-based inspection.&#x20;

Application control updates are covered by FortiCare standard support but it requires a subscription. The Application Control database is different from the IPS database.&#x20;

The Application Control signature is organized in a hierarchy giving admins the possibility to manage categories of applications, entire applications or even application features.



{% tabs %}
{% tab title="Profile-based NGFW mode" %}
An Application Control profile must be created and applied to the firewall policy

The profile has the following filters:

* Categories
* Overrides
  * Application overrides: Provides flexibility by assigning applications or features to new categories
  * Filter overrides: ?????

For each entry, the follwing actions are available:

* **Allow**: Continue to next scan and do not log
* **Monitor**: Same as allow, but generate a log
* **Block**: Drop packets and log
* **Quarantine**: Block the traffic from an atacker IP until the expiration timer is reached. Also generate a log&#x20;

Some applications require deep inspection in order to be correctly classified. Traffic that can't be matched to a particular application will go into the "Unknown Application" category.

When using Application Control, you can tweak some operations using these options:

* **Allow and Log DNS Traffic**: Enbale it to allow DNS traffic for the application sensor.&#x20;
* **QUICK**: Blocking it (default) disables the use of quick which uses UDP and can't be scanned by web filtering.
* **Replacement Messages for HTTP-based Applications**: Sends a replacement message to the user when HTTP-based applications are blocked.

Protocol enforcement can be used to configure known service to only work on known ports, while being blocked on other ports. In case the engine detects a violation, it will take one of the actions: Block or Monitor

* When the IPS engine (protocol dissector) confirms the service, protocol enforcement can check if the service is using an allowed port. If it is not, it is considered a violation and an action will be taken.
* When the IPS engine cannot confirm the service, it will still be considered a violation if the protocols defined as enforced as rulled out of the potetnial services.&#x20;
{% endtab %}

{% tab title="Policy-based NGFW mode" %}
In policy-based NGFW mode, application control is configured directly on the security policy. In the consolidated SSL inspection profile, an inspection profile must be selected and Central NAT is also required instead of NAT on the policy.

You can add an application category or an application to the security policy rule and the action will be one of:

* ACCEPT
* DENY

If an URL filter is also added to the rule, then Application Control will only scan web-based apps.

In Policy-based mode, traffic is inspected this way:

1. FortiGate allows the traffic but marks the session as "may\_dirty" while passing the traffic to the IPS engine.&#x20;
2. As soon as IPS engine identifies the application, it updates the session with the "dirty" and "app\_valid" flags and also an application ID
3. FortiGate looks up the application ID in the security policy and takes the appropriate action


{% endtab %}
{% endtabs %}

## Application Control Traffic Shaping

A traffic Shaping policy can be created in Policy & Objects > Traffic Shaping menu.

There are two shaping policies available:

* Shared shaper: applies a total bandwidth to all traffic using that shaper. It applies for ingress-to-egress traffic
  * The reverse shaper is the policy applied for the reverse traffic (egress-to-ingress)
* Per-IP shaper: applies traffic shaping to each source IP in the security policy.









