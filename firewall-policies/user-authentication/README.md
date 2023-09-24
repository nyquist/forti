# User Authentication

Firewall policies can also allow or deny access to authenticated users. The following methods are supported:

## **Active User Authentication**

### **Local password authentication**

* In this case, users are locally defined on the FortiGate system.
* You can also enable **Two-factor authentication**
  * OTPs can be provided as:
    * hardware FortiToken
    * software FortiToken Mobile app. FortiToken Mobile Push allows users to accept the authorization request from their mobile - each FortiGate comes with 2 FortiToken Mobile activations by default. The same FortiToken cannot be used on different FortiGates, unless a FortiAuthenticator service is used.
    * email
    * SMS

### **Server based password authentication** aka **Remote password authentication**

* In this case FortiGate sends the users credentials to the authentication server
* Supported protocols: POP3, RADIUS, LDAP, TACACS+
* You can either create local users with remote authentication enabled, or you can create a user group linked with a remote autehentication server. In the latter case, any user authenticated by the server will have the privilages of the group.

### **Troubleshooting**

You can test user authentication from CLI:

```
diagnose test authserver ldap SERVER USERNAME PASSWORD
diagnose test authserver radius SERVER {pap|chap} USERNAME PASSWORD
```

## Passive User Authentication

* Supported protocols: FSSO(Fortinet Single Sign-On), RSSO (RADIUS Single Sign-On), NTLM

## User Groups

User Groups are collections of users. Policies can include user or user groups.

Guest user groups are special groups where many users can be created at once using random userids and passwrds and have a predeterminde expiration time.&#x20;

## Authentication while processing Firewall Rules

User authentication is checked when the user or usergroup is defined in the SOURCE field of a policy. DNS traffic is exempt from authentication check because it is considered a base protocol needed to setup the connection.

Only HTTP, HTTPS, FTP and Telnet are able to show the authentication dialog used in active authentication. All other services are not allowed until the user is succesfully authenticated with one of the supported protocols.

Passive authentication works with any protocol because authentication is done on a separate channel.

When processing firewall rules the traffic can:

* match an ALLOW rule with authentication: Traffic is not allowed and the next rule is checked.
* match an ALLOW rule without authentication: Traffic is allowed. User is not prompted for authentication
* match a DENY rule without authentication: Traffic is denied. User is not prompted for authentication
* no match is found but traffic matched a rule with authentication: User is prompted for credentials and traffic is allowed or denied based on user info.

To prevent improper user, the following options are available:

* All firewall policies must have authentication enabled. Otherwise, it is likely that traffic will go throu the rules that don't require authentication
* On CLI you can set `set auth-on-demand {always|implicit}` in order to trigger authentication request on rules that require authentication. By default this is set to "implicit"
* User captive portal authentication at the interface level to force user authentication before they are allowed access

### Authentication Timeout

Default timeout is set to 5 minutes of idle time.

```
config user setting
  set auth-timeout-type {idle-timeout|hard-timeout|new-session}
end
```

* **idle-timeout** is reset when a new packet is received from the host IP
* **hard-timeout** is reset after the specified period regardless of user behavior
* **new-session** is reset when a new session is created from the source IP
