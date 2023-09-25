# Basic Web Filtering

### URL Category Filtering

It is a subscription based FortiGuard service. FortiGate checks the category for each website, but if there is a rating error (error communicating with FortiGuard, or lack of contract), by default FortiGate will block the websites. This configuration can be changed in the web-filter profile.

You can also use FortiManager to act as a local FortiGuard server. To do this, the database must be downloaded to the FortiManager first. You can verify the connection to FortiGuard using `diagnose debug rating`.

The actions available for each category, are:

<table data-full-width="true"><thead><tr><th>Proxy-Based</th><th>Flow-Based (Profile)</th><th>Flow-Based (Policy)</th></tr></thead><tbody><tr><td>Allow</td><td>Allow</td><td>Allow</td></tr><tr><td>Block</td><td>Block</td><td>Deny</td></tr><tr><td>Monitor</td><td>Monitor</td><td></td></tr><tr><td>Warning</td><td>Warning</td><td></td></tr><tr><td>Authenticate</td><td>Authenticate</td><td></td></tr></tbody></table>

**Warning** = Gives the user a warning, but the user can accept the risk and proceed.

**Authenticate** = will verify user authentication via local or remote methods. A user group that is allowed to access the resource must be defined.

**Web rating override** is a feature that allows specific websites to be have their category rating overridden in order to have a different action performed on it.

### &#x20;Static URL Filtering

The list of entries is checked from top to bottom. Entries can be:

* simple (exact match)
* wildcard
* regex

The actions for each entry can beL:

* **Allow**: access is permitted. Traffic is passed to remaining security inspections
* **Block**: access is denied. A replacement message is sent to the user
* **Monitor**: Same as allow, but a log is also created
* **Exempt**: access is permitted. Any other security inspection is bypassed.

