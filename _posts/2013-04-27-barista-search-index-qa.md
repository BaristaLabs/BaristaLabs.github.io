---
layout: post
title:  "Barista Search Index Q&A"
date:   2013-04-27
author: "Sean McLellan"
tags: ["Barista Search Index Bundle"]
---

Q&A:
 
Q: What powers the search index under the covers?
 
A: The Barista Search Index is powered by Lucene.Net 3.0.3 – Available on NuGet. The Barista Search Index hosts the Lucene Search Engine in a separate SharePoint managed windows service. This allows the service to operate independently of app pool recycles and allows for the index to cleanly shut down.
 
 
Q: Who else uses Lucene? Is it mature?
 
A number of sites use lucene. See http://wiki.apache.org/lucene-java/PoweredBy.
 
The site StackOverflow uses lucene to power its search. Sites like Twitter Analytics use it as well.
 
Lucene has been around since 1999, I would venture that it’s pretty darn mature. Lucene and Lucene.Net are both active Apache sponsored open-source projects.
 
 
Q: Why would I use the Barista Search Index over FAST Search or SharePoint Search?
 
A: The main reason for using the Barista Search Index is to provide an application-level search functionality that would be otherwise difficult or administratively burdensome to setup. Using FAST or SharePoint search to power an application-level search has the issue of having to wait until the next crawl for new content to be available – even with the continuous crawl option in SharePoint 2013, new content may take 5-10 minutes to appear depending on the load of the crawler. The FAST search index used to have a capability through the Content API to insert documents directly into the FAST search index, however, this capability is deprecated – I’m not aware of a Content API for SharePoint Search in SharePoint 2013. Inserting documents into FAST via SQL is traditionally a burdensome process.
 
You can get going extremely quickly with the Barista Search Index without making farm-wide configuration changes, requiring FAST and so forth.
 
 
For searching above the scope of an application, such as portal wide search and searching within documents, using FAST or SharePoint search is still recommended.
 
 
Q: I have a lot of data I potentially wish to index, can I hit the index directly without going through a Barista Service?
 
A: Yes. The Barista Search Index exposes a WCF endpoint that allows data to be indexed and search results to be retrieved through a WCF client. The endpoint is available at http://appservername:8500/Barista/Search/mex. This endpoint should be available to any SOAP client and is strongly typed. If the Administrator has changed the endpoint address, you’ll need to update accordingly.
 
 
 
Q: Is there a service locator for the Barista Index Service?
 
A: Yes. By referencing and using the SPBaristaSearchServiceProxy class. This class has behavior to query and locate and use the Barista Search Index within the SharePoint farm and configure the client to the endpoint address.
 
 
 
Q: Will you give me cookies?
 
A: No. Unless you’re blue and furry.