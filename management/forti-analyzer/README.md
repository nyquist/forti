# Forti Analyzer

FortiAnalyzer - aggregates log data from Fortinet devices but also supports other syslog capable devices.

FortiAnalyzer uses PostgresSQL and supports [SQL ](datasets-and-sql.md)for logging and reporting

## System Configuration

### Operating modes

* Analyzer (default) - central log aggregator for log collectors (FAZ in collector mode) or devices
* Collector: collects logs and forwards them to an analyzer, syslog server or CEF (Common Event Format) server. It doesn’t support event management and reporting

### FortiAnalyzer Fabric

Multiple FAZ in Analyzer mode can create a FortiAnalyzer Fabric which consinsts of a supervisor and multiple members. The supervisor can provide a view into aggregated data from all members.

### FortiAnalyzer and Security Fabric

FAZ supports the Security Fabric by storing and analyzing the logs from the units in a Security Fabric group as if the logs come from a single device. Traffic logging is done by the first FortiGate in the Security Fabric that handles the traffic. Upstream FortiGates in the Security Fabric will send logs only if they perform NAT or for UTM events (if configured)

FAZ ADOMs can be used to group devices and split access to their logs.

## Logging

* Registered device sends logs to FAZ.
* Logs are decompressed and saved in a log file on FAZ disk
* Saved logs are indexed in SQL database for analysis.
* Per scheduled, on based on size, the logs are rolled over: rename the file with a timestamp and compress it.

## FortiAnalyzer Features

### Log View

Log View = view and search through all logs

### Forti View

Forti View = shows real-time and historical data

* includes:
  * Top threats,
  * IOC (indicator of compromise) - based on IOS signatures coming from FortiGuard

Forti View Monitors:

* Designed for NOC and SOC with large monitors

Log Fetching allows fetching data from another FAZ for advanced analysis.

FAZ FabricView allows connector to other services: ITSM, generic web hooks, Storage providers, Security providers.

* Asset Center - displays information about known endpoints and users grouped by endpoint
* Identity Center - similar, but grouped by user

### FortiSoC

FortiSoC provides FortiSOAR (Security Orchestration, Automation and Response) and FortiSIEM (Security Information and Event Management) as MEAs (Management Extension Application).

FortiSoC provides 3 dashboards:



{% tabs %}
{% tab title="Events" %}
Events Dashboard: tracks security events

* Events are generated based on received logs.
* Event Handlers look for specific conditions in the logs
* If a match is found, notifications can also be sent as email, SNMP traps, to a fabric connector (like web hooks) or to a syslog destination.
* **Event status**:
  * **Unhandled** (eg. when logs contain action=pass)
  * **Contained** (eg. when logs contain action=quarantine)
  * **Mitigated** (Eg. When action = block/drop)
  * **Blank** - (Eg. when there is a mix of actions)
* Events can be acknowledged or an incident can be created from them for further investigation
{% endtab %}

{% tab title="Incidents" %}
Incidents Dashboard: tracks security incidents

* Incidents can be created manually or automatically (via playbooks)
{% endtab %}

{% tab title="Playbooks" %}
Playbooks Dashboard: tracks executed playbooks.

* Allow you to automate common SOC tasks.
* Playbooks have a trigger
  * Event trigger
  * Incident trigger
  * On schedule
  * On demand
* Playbooks have one or more tasks. Tasks can run sequential or in parallel.
{% endtab %}
{% endtabs %}

**Outbreak detection service** is a licensed feature that allows FortiAnalyzr admins to receive outbreak alerts, event handlers and reports from FortiGuard.

### Reports

Reports are sets of data organized as charts. A chart consists of [Datasets ](datasets-and-sql.md)and Formats (table, chart, etc)

Forti analyzer comes with predefined templates for reports which can be used by analysts to create actual reports.

Reports can be sent over email or uploaded to SFTP servers. They can also be attached to incidents.
