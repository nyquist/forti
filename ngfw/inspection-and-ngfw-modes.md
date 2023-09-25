# Inspection and NGFW Modes



## Inspection Mode: Flow-Based vs Proxy-Based

| Flow-Based Inspection                                                                                                                                                                                                                                                                                        | Proxy-Based Inspection                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| In flow-based mode, the firewall looks at each packet but forwards it without waiting for the full message flow to be completed. This means that the original traffic is not altered, and that the user experiences a faster response time, but it also means that some advanced features are not supported. | In flow-based mode, the firewall looks at each packet but forwards it without waiting for the full message flow to be completed. This means that the original traffic is not altered, and that the user experiences a faster response time, but it also means that some advanced features are not supported. |

## NGFW Mode: Profile-Based vs Policy-Based

Fortigate, or the individual VDOM works either in profile or policy mode.

| Profile-Based                                                                                             | Policy-Based                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Admin must create and use application control and web filter profiles and apply them to a firewall policy | Admin can apply application control and web filter configuration directly to a security policy                                                                                                                                                                 |
| Can use both profile-based and flow-based inspection mode as per the policy                               | Only works with flow-based inspection mode                                                                                                                                                                                                                     |
|                                                                                                           | In the "SSL Inspection & Authentication" consolidated policy, the admin defines the SSL inspection profile. If traffic is allowed, then the next step is to look at the security policy where web filtering, application control, antivirus, etc, are defined. |

Antivirus config is profile based, regardless of the NGFW mode.

The NGFW mode can be changed in the system settings but requires the removal of all existing policies in either mode.



