# Logging

## Event types

* Traffic logs: record traffic flow information
  * Forward traffic logs - info about traffic that went through a firewall policy
  * Local traffic logs - traffic send to FortiGate mgmt IP addresses, including GUI
  * Sniffer logs - traffic seen by the one-arm sniffer
* Event logs: record system and admin events
  * System event logs - operations such as FortiGuard updates and GUI logins
  * User logs - login/logout events for firewall policies with user authentication
  * Router, VPN, Wireless logs&#x20;
* Security logs: record security events such as IPS or antivirus

## Log levels

Log levels define the importance of the log entry:

* 0 - Emergency = System Unstable
* 1 - Alert = Immediate action required
* 2 - Critical = Functionality affected
* 3 - Error = Error exists that can affect functionality
* 4 - Warning = Functionality could be affected
* 5 - Notification = Information about normal events
* 6 - Information = General System Information
* 6 = Debug = Diagnostic information for investigating issues

## Log storage

* **Local Storage**: FortiGate can store logs on its hard drive (for higher end models that support it). By default, logs are deleted after 7 days. The system will reserver 25% of the disk space for system usage so only the rest is available for logs
  * use `diagnose sys logdisk usage` to verify current usage
* **Remote Storage**:&#x20;
  * **Syslog** - a centralized logging server
    * up to 4 servers are supported
  * **FortiCloud** - a subscription based Fortinet service for log storage. A free tier comes with every fortigate
  * **FortiSIEM**
    * up to 4 servers are supported
  * **FortiAnalyzer / FortiManager:** After FortiGate is registered with FortiManager or FortiAnalyzer, it can send logs to them.
    * Logs can be sent to FortiAnalyzer or FortiManager
      * real time
      * every minute
      * every 5 minutes (default)
      * store-and-upload (CLI only) - this allows local storage and then upload at a scheduled time to conserve bandwidth
    * When FortiAnalyzer becomes unavaialable for a short period of time (e.g to cover a reboot after an upgrade), FortiGate can cache the logs locally until FortiAnalyzer is available again.

When using VDOMs, you can add up to 3xFortiAnalyzer and 4xSyslog servers in the global settings. Then at VDOM level you can override global syslog servers.

## Logging Protocols

By default, FortiGates use UDP 514 to send logs, but the port can be changed, as well as the protocol. TCP can be used for a reliable data transfer. When using TCP, the communication can also be encrypted using OFTPS.

## Log Filtering

In the GUI we can select which types of logs are enabled, but in CLI the config can be made even more granular.

To get traffic logs, the "Log Allowed Traffic" option must be enabled on each rule. When it is enabled, then you can chose to log:

* only **Security Events**: then security events will appear in the forward traffic log and in the security log
* **All Sessions:** then a log is generated for every single session and security events will appear in both forward traffic log and security log.

### Hiding Usernames

Usernames can be hidden in logs (as required by law in certain jursidictions) through a CLI command. Then the username "anonymous" will replace the actual username in the logs

### Event Alerting

On specific events, FortiGate can also send email notifications&#x20;

## Backing Up Logs

Logs can be backed up to a USB drive or computer disk using the GUI, or to a FTP, TFTP or USB drive using CLI commands. The logs are stored in LZ4 format

```
execute backup disk alllogs
execute bakcup disk log LOG_TYPE
```

By default, logs are rolled when they reach 20MB. Additionaly you ca upload rolled log files to an FTP server using the CLI.



