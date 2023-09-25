# Proxy-Based Web Filtering

## Search Engine Filtering

This feature is only available when full SSL inspection is enabled and works in proxy-based inspection mode only. It will enforce "safe search" mode on supported search engines: Google, Yahoo, Bing and Yandex.

It will also log all search keywords.

## Web Content Filtering

This feature is only available when full SSL inspection is enabled and works in proxy-based inspection mode only.It allows an admin to control access to web pages that contain specific patterns.

The patterns can be defined as&#x20;

* simple (exact match)
* wildcard
* regex

The actions available are:

* Exempt
* Block

### Video Filter Profile

This feature is only available when full SSL inspection is enabled and works in proxy-based inspection mode only.&#x20;

### Video Filter Categories

Video Filter Categories makes use of the  classification of these videos on supported websites (youtube  - requires an API key in the config, vimeo, dailymotion). Just like with Web Categories, you can decide which video categories you want to:

* allow: Allow the video without checking other video filter features, but go through other security features
* monitor: Generate a log and continue to the other video filter features
* block: Generate a log and block access to the video

### Youtube specific settings

You can enable **Moderate** or **Strict** access to youtube - based on Google's definition of these filters, but you can also define specific Channel IDs that can have their action overriden.





