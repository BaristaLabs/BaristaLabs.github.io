---
layout: post
title:  "Top 10 Barista Improvements This Week"
date:   2013-07-03
author: "Sean McLellan"
tags: ["Barista Improvements", "TFS Bundle"]
---

The top 10 recent changes in Barista:
 
1. Barista has reached 1,000 checkins!! Now, granted, some of those were Drop checkins, but still, it’s a milestone.
   1. ![alt text](/img/2013-07-23-top-10-barista-improvements-this-week-01.png "Checkins ")
2. NetworkCredential is now exposed as a top-level object
   1. You can create a new NetworkCredential object and pass it to common objects. Right now the bundles that utilize the NetworkCredential are Proxy, Sql, Raven (Web/MVC Barista), TFS (SharePoint/Web/MVC) (More on this below)
3. On improvements in the NetworkCredential front, SharePoint Barista now can interact with the SharePoint Secure Store Service to retrieve pre-defined credentials:
   1. There is a new property on the sp instance retrieved from requiring the SharePoint bundle: secureStore. This object has behavior to perform the following operations:
   2. Keep in mind that SharePoint exposes a whole permissions model around granting users rights to access to the secure store. This functionality in barista is not bypassing the SharePoint mechanisms in any way.
4. Updated TFS Bundle.
   1. TFS can now be used to connect to a remote TFS Instance (Up to TFS 2012 Update 1) and retrieve work item data.
   2. In this end-to-end example which shows some of the above capabilities, we connect to the RDA TFS2012 instance and perform a WIQL-based query to retrieve work items.
      1. I’ve set up an application in my secure store named “RDATFS"
      ![alt text](/img/2013-07-23-top-10-barista-improvements-this-week-02.jpg "RDATFS" )
      2.And set the user permission to be my RDA account:
      ![alt text](/img/2013-07-23-top-10-barista-improvements-this-week-03.jpg "Permission")
      3. Now, I’m able to access these credentials through Barista – since I’m the owner I get access immediately, but if I wanted to grant access others, I can do that within the Secure Store Service.
      4. Let’s now use those credentials and pass them to RDA’s TFS – allowing me to enumerate all work items. In this example, I’m just getting the title, the state and the last changed date, but all information is available.
      ![alt text](/img/2013-07-23-top-10-barista-improvements-this-week-03-01.png "Secure Store" )
    5. More work to come in this area, some changes in the object model will occur, some work to be done to retrieve the existing queries, some work to be done to work with TFS Version Control.
    6. The intent here is to facilitate a way of external parties to be able to have a read view into work item statuses stored on the TFS server through a web page, without knowing an  credential. A second goal is to be able to synchronize files and folders in a document library with those stored within TFS. (E.g. TFS-Based Content Deployment.)
 
5. Within Web/MVC Barista, Barista Fiddle now uses AngularJS – so I can add features/functionality in a faster, more maintainable way – this work will roll back to SharePoint Barista soon.
6. Within Web/MVC Barista, A new bundle that facilitates interaction with RavenDB is now available:
    1. ![alt text](/img/2013-07-23-top-10-barista-improvements-this-week-04.png "Fiddle")
    2. ![alt text](/img/2013-07-23-top-10-barista-improvements-this-week-05.jpg "Fiddle")
7. A new Unit Testing bundle is now available to SharePoint and Web/MVC Barista.
   1. This bundle exposes the top-level object “chance” which generates random data, and “assert”
   2. Behind-the-scenes, assert uses the same unit-testing framework as is in Visual Studio, so that unit test code coverage metrics can be obtained.
8. Unit Tests are now maintained under the WebHost example impl.
   1. ![alt text](/img/2013-07-23-top-10-barista-improvements-this-week-06.jpg "Unit Tests")
9. Minor Code Quality changes within the Jurassic import, and a pull of the latest two fixes by Paul Bartrum on codeplex – one can only hope that he’ll continue to make changes that I’ll pull into Barista.
   1. ![alt text](/img/2013-07-23-top-10-barista-improvements-this-week-07.png "Jurassic")
10. The Latest Kendo UI has now been included with SharePoint Barista under /_layouts/BaristaJS/kendoui.complete.2013.2.716/
    1. For what’s new, visit http://www.kendoui.com/blogs/teamblog/posts/13-07-17/what's-new-in-kendo-ui-q2-2013-.aspx
 
Namaste!