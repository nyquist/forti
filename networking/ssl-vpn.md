# SSL VPN

{% tabs %}
{% tab title="SSL VPN Server" %}
| Tunne Mode                                                                                                                                                                    | Web Mode                                                                                                                                                       |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| It is accessed through a FortiClient because it requires a virtual adapter (called fortissl) on the client host. The tunnel is up only while the SSL VPN client is connected. | <p>It is accessed through a browser. Supports FTP, HTTP(S), RDP, SMB, SSH, Telnet, VNC, ping<br>The SSL VPN stays up only while the SSL VPN portal is open</p> |
| The tunnel assigns a virtual IP address to the client from a pool of reserved addresse and all traffic is encapsulated with SSL/TLS                                           | After logging in to the SSL VPN portal, it will act as a reverse proxy or a HTTP(s) gateway that connects you with the applications on the private network.    |

### Configuration

#### 1. Configure user accounts&#x20;

Both local and remote authentication is supported, with the exception of FSSO.

#### 2. Configure SSL VPN Portal

Go to VPN > SSL VPN Portals. Here you can configure the portal and select the mode (tunnel, web)



{% tabs %}
{% tab title="Tunnel Mode" %}
When you enable tunnel mode, you also have the option to enable split tunneling or not. If you want to enable it, you have 2 options:

* Enabled based on policy destination - means the list of subnets will be taken from the firewall policy that matches SSL VPN
* Enabled for Trusted destinations - allows the list of subnets to eb configured on the SSL VP
{% endtab %}

{% tab title="Web Mode" %}
When you sepcify Web mode, you have the option to customize the portal.
{% endtab %}
{% endtabs %}

#### 3. Configure SSL VPN Settings

In VPN>SSL VPN Settings, you first link an interface to the SSL VPN Portal and select a certificate that will be used by the portal

In tunnel mode, you define the IP range for SSL VPN and you can specify the allocation method (first-available or round-robin)

The DNS server can be specified or left as the DNS server of the client. The second option would be useful only if split tunneling is not used.

In WebMode, you can customize different aspects of the Web Portal page.

Finally you can select which user groups can access the VPN Portal

#### 4.  Firewall Policy to and from the SSL VPN Interface

Traffic coming from the users via a SSL VPN will enter the FortiGate through the ssl.VDOM\_NAME interface.

First you need to allow access from ssl.VDOM\_NAME interface to the interface which is linked with the SSL VPN portal (?)

Then, you need to allow traffic from ssl.VDOM\_NAME interface to other interfaces where the other resources are reachable.

### Client Integrity Check

This feature enables additional checks of the client before it is allowed to connect.  The check is done after user authentication completed. This feature is supported with Windows clients only. It can be enabled through the GUI on the portal page but it is configurable only through the CLI.&#x20;
{% endtab %}

{% tab title="SSL VPN Client" %}
FortiGate can also be configured as a SSL VPN client using a SSL-VPN Tunnetl interface. The client dynamically adds a route to the subnets that are returned by the SSL VPN server. If teh server spcifies 'all' as the supported then a default route is added. Otherwise, when not all subnets are specified, we operate in a split tunnel setup.

### Configuration

### 1. Create a PKI user

The CN should be the same as the one configured on the FortiGate.&#x20;

### 2. Create SSL VPN tunnel

Create a new interface of type SSL-VPN Tunnel.&#x20;

### 3. Create SSL VPN Client Settings

In VPN > SSL-VPN Clients, create a new client and sepcify client name, SSL VPN Interface, SSL VPN Server IP and port, the PSK and the client certificate.

Route is added automatically when SSL VPN connection is up

### 4. Create a Firewall Policy to allow traffic from internal interface to SSL VPN interface

A policy needs to be created to allow traffic from internal interfaces to the ssl vpn interface.
{% endtab %}
{% endtabs %}

## Timeouts

```
config vpn ssl settings
  set idle-timeout <0-2592200>
  set login-timeout <10-180>
  set dtls-hello-timeout <10-60>
  set http-request-header-timeout <1-60>
  set http-request-body-timeout <1-60>
end
```

## Debugging

```
diagnose debug enable
diagnose vpn ssl {list|info|statistics|debug-filter|hw-acceleration-status|tunnel-test|web-mode-test}
diagnose debug application sslvpn -1
```





