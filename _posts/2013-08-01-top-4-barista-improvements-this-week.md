---
layout: post
title:  "Top 4 Barista Improvements This Week"
date:   2013-08-01
author: "Sean McLellan"
tags: ["Barista Improvements"]
---

The top 4 recent changes this week:

1. Updated imports of SuperSocket, SuperWebSocket, and Json.Net to latest versions (30% improvement in Json serialization/deserialization performance)
2. Added Barista Search Index bundle to Web Barista 
   1. Added a configuration section for defining Barista configuration within the web.config. In SharePoint Barista, configuration is stored in the farm property bag. In Web Barista, we needed to store this config somewhere. This allows us to create our index definitions within the web.config, and more as time goes on. ![alt text](/img/2013-08-01-top-4-barista-improvements-this-week-01.png "Configuration")
   2. The Search Service in SharePoint Barista uses SOAP, I wanted the service in Web Barista to use Rest. Due to fun serialization issues with the OOtB DataContractJsonSerializer, I added the necessary WCF behavior to use the Json.Net serializer in place of the DataContractJsonSerializer. This also gets configured within the servicemodel of the web.config. Future services will benefit from this too (WebSocketsService, SchedulerService)
    ![alt text](/img/2013-08-01-top-4-barista-improvements-this-week-02.jpg "Search Configuration")
3. Added Barista Search Index Unit Tests
   1. ![alt text](/img/2013-08-01-top-4-barista-improvements-this-week-03.jpg "Unit Tests")
   2. This brings the number of unit tests in the web project up to 58, each with multiple assertions. This figure does not include SharePoint-related unit tests.
4. Added “getContentTypeById” function to splist object – more on the content type arena as Joe and I work together on ways to facilitate moving to ECM.
 
Some prelim work on the scheduler service and WebSocketsServices was also performed.
 
Target areas for next week:

1. Provide IFolderDocumentStore and IEntityPartDocumentStore implementations on the FileSystemDocumentStore class
2. Add the ability for a documentstore to clone itself to another document store.
3. More design work around Scheduler/WebSockets service.
4. Work around content types in SharePoint Barista.