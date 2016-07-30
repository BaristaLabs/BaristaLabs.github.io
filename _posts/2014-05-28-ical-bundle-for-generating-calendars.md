---
layout: post
title:  "iCal Bundle for generating calendars"
date:   2014-05-28
author: "Sean McLellan"
tags: ["iCal Bundle"]
---

One recent bundle added to Barista is the ability to generate iCal feeds on the fly.

iCal feeds are a standards-based representation of a calendar that can be subscribed via Outlook and other mail clients. Some iCal feeds out in the wild include Moon Phases, Game Release dates and so on.

A potential feature to a Barista-based application is the ability to surface expiring contracts information via a calendar so a COR would potentially not have to leave outlook to get this information.

There's a whole website to subscribe to iCal calendars.
http://www.icalshare.com/


The following example code creates and returns an iCal Calendar with a single event via Barista

```javascript
var iCal = require("iCal");

var cal = iCal.createCalendar();
cal.addLocalTimeZone();

//iCal works with events.
var event = cal.createEvent();
    event.summary ="My Event"
    event.location = "Some Where";
    event.organizer = "sean@baristalabs.io";
    event.start = new Date();
    event.uid = "guid"
    event.isAllDay = true;

//Add a html description for the event.
var property = new iCalendarProperty("X-ALT-DESC");
    property.addParameter("FMTTYPE", "text/html");
    property.value = "<html><body>" +
"<b>This is my event!!</b></body></html>";

event.properties.add(property);

cal.getBytes("myCalendar.ics");
```