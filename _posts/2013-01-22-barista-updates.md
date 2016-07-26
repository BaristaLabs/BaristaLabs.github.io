---
layout: post
title:  "Barista Updates"
date:   2013-01-22
author: "Sean McLellan"
tags: ["General Improvements"]
---
* Better BaristaFiddle compatibility with IE8
  * Thanks to a fulfilled bug filing with CodeMirror, the annoying discrepancy between cursor position and ‘actual’ cursor position in IE8 should be fixed.

   Thanks CodeMirror guys.
 
* Updates/Additions of JS Libraries.
* Better/Simpler parsing of AJAX responses when using the Web bundle.
  * The ajax function will now better handle external RSS feeds and attempt to auto-decode to a JSON object.
  * For instance,

 ```javascript
var web = require("Web");
web.ajax("http://www.metroalerts.info/rss.aspx?rs");
```

Results in the JSON object:
 
```json
{
    "rss": {
        "@version": "2.0",
        "channel": {
            "title": "Washington Metropolitan Area Transit Authority Alerts",
            "link": "http://www.wmata.com/",
            "description": "Washington Metropolitan Area Transit Authority Alerts",
            "language": "en-us",
            "generator": "eAlert Messaging System  http://www.ealert.com/",
            "webMaster": "hostmaster@mis-sciences.com (MIS Hostmaster)",
            "ttl": "15"
        }
    }
}
```
Without any additional work needed. If a RAW object is required, specify:

```javascript
web.ajax("http://www.metroalerts.info/rss.aspx?rs", { dataType: "Raw" });
```

* Beneath-the-scenes improvements in Search, OData for future functionality.