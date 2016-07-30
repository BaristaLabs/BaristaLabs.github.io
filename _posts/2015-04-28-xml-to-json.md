---
layout: post
title:  "XML to JSON"
date:   2015-04-28
author: "Sean McLellan"
tags: ["Document Bundle"]
---

A quickie - Sometimes we want to convert from XML to JSON. Either we have some XML from a other source (InfoPath, RSS/Atom Feed, etc) and we want to make a JSON service from it.

Barista makes it easy, just get any xml doc and run it through xml2Json:

![alt text](/img/2015-04-28-xml-to-json-01.png "XML String Conversion")

Which gets you:
 
![alt text](/img/2015-04-28-xml-to-json-02.png "Result")


Thus you can access it's properties:
 
![alt text](/img/2015-04-28-xml-to-json-03.png "Property Access")

Result:

![alt text](/img/2015-04-28-xml-to-json-04.png "Result")

and just pull in the SharePoint bundle to load the actual files.

![alt text](/img/2015-04-28-xml-to-json-05.png "XML File Conversion")

json2Xml is also exposed.