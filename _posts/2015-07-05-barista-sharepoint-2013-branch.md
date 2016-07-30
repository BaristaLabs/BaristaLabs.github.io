---
layout: post
title:  "Barista SharePoint 2013 Branch"
date:   2015-07-05
author: "Sean McLellan"
tags: ["SharePoint 2013"]
---

There is now a branch within the repository that supports SharePoint 2013

Some Highlights:

* Barista Fiddle improvements (Editor updates, Autocomplete, code folding, syntax checking)
* New WkHtmlToPdf bundle for Html to Pdf document conversion (Based on WkHtmlToPdf library which isn't a commercial product, has ability to set page sizes/margins/etc)
* Reduced GAC deployments -- all dependencies except Barista.Core, Barista.SharePoint and Barista.Core are now located in the hive rather than the GAC to minimize collisions.
* Minification in Web Optimization bundle no longer depends on YUI Compressor
* Addition of endpoint to get general Barista status (Machine name, Barista deployment status, etc: /_layouts/barista/v1/barista.svc/status)
* Improvements to the WCF Pipeline
* General bugfixes across the bundles.

Roadmap:

* Additional Barista Fiddle improvements (Tabs, Dynamic Auto-complete, better save/load to SharePoint mechanism)​​
* Separate Bundles into individual assemblies and use DI to lift them (Work started)
* Provide mechanism for individual bundles to be approved via a farm administrator in the service application management screen
* Provide mechanism for bundles to be deployed via different mechanism than SP deployment
* Provide v2 of barista services which uses another underlying script engine (V8, Chakra, other... specific goals in mind are performance, sandboxing, real-time debugging, ECMAScript 6 support...)
* General work items in TFS...