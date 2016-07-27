---
layout: post
title:  "Introducing Barista Logging"
date:   2012-08-29
author: "Sean McLellan"
tags: ["Logging"]
---

One feature request that was brought up when we were demoing Barista Deferreds was the issue of logging.

This is a bit dry of a topic (who really cares about logging) but it’s also interesting to examine what’s common to JS devs and discuss how Barista might implement it.

In the “real-world”, most browsers (Chrome, IE 9+, Firefox+firebug) support the console object (http://getfirebug.com/wiki/index.php/Console_API). This pattern is useful to follow in order to make Barista logging familiar to existing  JavaScript developers. However, web browsers log the data to the built-in dev console client side, Barista scripts run server side – there’s no dev console.. or console in general. So the question is where these logged messages should go in Barista.

Since Barista is built on top of SharePoint, I think it’s important to capitalize on SharePoint features. So the ULS log in this case makes the most obvious sense as the backing store. It’s got all the built-in logging, filtering and aggregating benefits, but sometimes it can be difficult to interact with and requires access to the server to actually get at the logs. 

The existing WSS Logging OData service is one way that we already have of exposing ULS Data – this has worked fairly well, but the WSS Logging database isn’t updated by SharePoint continuously – there’s an OOtB timer job which does the magic – every 5 min or so.  Since Barista lightweight services that log to the Console should be able to retrieve logged messages in near-real time, this makes using our WSS Logging OData service less desirable (Although it’s still there to call via the previously mentioned ajax function)

So, from this, our logging requirements are something like the following:

1. Follow the console pattern so that it’s familiar to JS devs.
2. Use the ULS as a backing store.
3. Allow for near-real time retrieval of log entries 

  To get at a near-realtime ULS log I wanted to use something similar to the Merge-SPLogFile PowerShell command. Unfortunately, through some trial-and-error, attempting to invoke this method either by reflection or by invoking the cmdlet directly fails – it looks like it attempts to start a timer job, which requires writing to the SharePoint Config DB which in all cases requires Farm Admin privileges. Bummer. So that’s out. Second best is to just to open and expose the ULS log file on the current server.

4. Retrieved entries can be specific to the WFE the script is executing on (WSS Logging OData Service can be used for advanced farm-wide scenarios)

Let’s code:

First, I’ve  exposed a console object with a similar API as the FireBug console object that writes to the ULS log.

Second, I’ve added a global ‘log’ object that contains a ‘getLocalLogEntries’ method which will return log entries from the ULS log of the current server. Based on some discussion with Paul, this uses a streaming reader so the whole, potentially multi-megabyte, log file isn’t loaded up in RAM each time the log wants to be read. I’ve also added a helper method that can retrieve the current correlation id.


So, after all this discussion, here’s the short example. This writes ‘A catastrophic error has occurred’ with an ‘Unexpected error’ log level, gets the current Correlation Id and then returns all entries with that correlation id from the ULS log of the current server.

```javascript
console.error('A catastrophic error has occurred.');
delay(500); //A little delay to allow the message to be written.
var correlationId = util.getCurrentCorrelationId();
var logEntries = log.getLocalLogEntries(correlationId);
logEntries;
```

```json
[{"Area":"Barista","Category":"Console","CorrelationId":"bafaf09c-07fe-4395-b4ea-3b8ef3d44c49","EventID":"1","Level":"Unexpected","Message":"A catastrophic error has occurred.","Process":"w3wp.exe (0x24A0)","TID":"0x0D24","Timestamp":"2012-08-29T21:14:49.200Z"}]
```

Voila!

Other functions on the console object are

```javascript
console.log(object[])
console.debug(object[])
console.info(object[])
console.warn(object[])
console.assert(object[])
```

each logs to the ULS log with a corresponding log level – depending on configuration, the level may be auto-filtered from the log.
objects passed to the functions will be json serialized when written to the log

There is also:

```javascript
console.time(name); //Creates a new timer under the given name, console.timeEnd(name) stops the timer and returns the elapsed time.

log.getLocalLogEntries also has a second parameter which limits looking in log files to n number of days.
```

One can combine logging with query string parameters – though this is more of an example rather than illustrating a potential common practice.

e.g. 

```javascript
if (web.request.queryString.debug == “true”)
                console.debug(“Hi, log some important data.”);

http://OFS.SharePoint.Services.Rest/Barista.svc/eval?c=/Script.js&debug=true would enter into ‘debug’ mode and write more detailed trace logs.
```

I think this will make logging easier within Barista scripts, and more importantly, make the ULS log accessible to Barista devs. Thoughts?

Next up: Barista Fiddle ;)
