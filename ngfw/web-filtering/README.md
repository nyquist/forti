# Web Filtering

Depending on the mode of operation, FortiGuard supports different web filtering features.

* **Policy-Based NGFW mode**: web filter (URL categories) is defined directly under the firewall policy
* **Profile-Based NGFW mode**: A web filter security profile can be applied to each security rule.
  * **Flow-Based inspection**: Web filtering is based on URL categories
  * **Proxy-Based inspection**: In addition to URL Categories, web filtering also supports:
    * Search engine filtering
    * Static URL filter
    * Rating Options
    * Proxy Options

## HTTP Inspection Order

1. Static URL Filter
   1. If Exempt, DISPLAY page
   2. If Block, BLOCK page
   3. If Allow, go to Category Filter
2. Category Filter
   1. If Block, BLOCK page
   2. if Allow, go to Advanced Filters
3. Advanced Filter
   1. if Block, BLOCK page
   2. If Allow, DISPLAY page

## Web filter Caching

FortiGate can maintain a list of recent website ratings in memory in order to reduce the number of calls to FortiGuard. By default, the timeout is set to 15 seconds, but it can go up to 30 seconds.
