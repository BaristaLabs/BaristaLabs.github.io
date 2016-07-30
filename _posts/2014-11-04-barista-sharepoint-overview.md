---
layout: post
title:  "Barista SharePoint Overview"
date:   2014-11-04
author: "Sean McLellan"
tags: ["Business Cases", "Barista Architecture"]
---

Barista-SharePoint is a custom SharePoint Service application for SharePoint 2010/2013/2016 that allows SharePoint services to be created on-the-fly using JavaScript-based scripts. Barista gets you straight to coding web services without needing to create boilerplate WCF/WebAPI plumbing.

One can compare Barista to the recent developments in serverless architectures facilititated by AWS Lambda, Azure Functions, Auth0 WebTasks and HyperDev.

 
![alt text](/img/barista-birds-eye-view.png "Birds Eye View")

Barista exposes a custom WCF-based endpoint to WFE servers. Requests come in through this endpoint (http://[yourfarm]/_vti_bin/barista/v1/barista.svc) and are handed off to an associated Barista Service Application to be processed. By using the SharePoint Service Application  archtecture, scalability and isolation can be easily achieved.

Barista processes a incoming request by obtaining the script file indicated through the c= query string parameter and JITs the JavaScript code to .NET IL. This IL is then cached and executed by Barista pulling in any Barista Bundles that are required by the indicated script.

These bundles provide for a rich layer of integration that facilitate common business-related tasks. The bundles included with SharePoint allow for:

* SharePoint Object Model interaction through Server-Side components
* Active Directory
* Rich document generation, including PDF conversion, XLSx SpreadSheet Generation and HTML-To-PDF conversion
* iCal Feed Creation
* Caching and file bundling
* Structured JSON Document Storage in SharePoint.
* Lucene.Net based index to facilitate search-driven-applications. 
* Much much more...

When the code is executed, the result is transformed to a JSON value and returned as the response. Developers are also able to directly manipulate the request parameters (header/body/etc...) to provide for custom scenarios.

Barista also has extensibility mechanisms to allow for custom bundles to be developed and deployed _without SharePoint Farm Downtime_. IIS resets are not performed when a Barista Bundle is deployed through Barista's own farm-deployment mechanism.


Barista provides an interactive web-based tool, also hosted in SharePoint (http://[yourfarm]/_layouts/baristafiddle/index.htm) called Barista Fiddle, that developers can utilize to rapidly create Barista Scripts and interactively test them and validate the service response. Barista Fiddle provides for linting and intellisense to find errors and discover functionality quickly.

For an overview, download [this document](/assets/barista-data-sheet.docx)

Read the [Barista Blog](/blog/) and the [documentation](/docs/) to learn more about Barista!


> Updated on 2016-07-30