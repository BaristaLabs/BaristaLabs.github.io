---
layout: post
title:  "Barista SharePoint Overview"
date:   2013-09-13
author: "Sean McLellan"
tags: ["Barista Overview"]
---

## Barista Overview
At BaristaLabs, we've found that the same patterns tend to repeat over numerous custom SharePoint projects.  We sat down and thought about what we could create that would free developers from the time consuming tasks required when building custom SharePoint applications. Further, we thought about the pain points facing customers and ways to minimize them.
The result is the Barista platform.

#### What is Barista?

Barista is a platform that allows line-of-business developers to create powerful server-side services to power the most challenging business applications. With Barista, you can leverage existing investments in SharePoint but accelerate custom projects by minimizing the level of effort required to deploy custom solutions. Developers can immediately get started developing applications without need for traditional processes that slow development effort and increase the opportunity for error.

![alt text](/img/barista-overview.png "Barista Overview")

Focus on your application, not the infrastructure.

Once deployed to a SharePoint environment, Barista frees developers from time-consuming application-level deployments, allowing them to focus on the task at hand, shorting the development cycle.  By simplifying the developer experience, this allows more time spent on actually developing and tuning the application – increasing application quality. Barista scripts live within SharePoint document libraries, allowing them to be easily secured, versioned and published with existing SharePoint functionality.

##### Apps made easy.

Barista uses the same language that UI developers are familiar with and provides a development environment that works in the browser. This minimizes the skillset required for developing custom SharePoint applications, allowing for less technical developers to staff custom development projects that utilize SharePoint. From a developer perspective, Barista provides the ability to write and immediately see the outcome of code changes. With Barista, developers create server-side REST based APIs that respond to changes in real-time.
Performant, Scalable and safe.
Barista uses the same service application model that out-of-the-box SharePoint solutions, such as search, uses. This allows applications to be isolated from one another, as well as to scale as need arises. Barista scripts execute in a sandboxed environment, so farms will not be taken down by errant code that made it through testing. 

##### Upgradable.
Upgrading custom development projects between versions of SharePoint has traditionally been a painful experience. Barista abstracts the underlying SharePoint object model such that whole application scripts can stay the same, Barista provides the required under-the-scenes changes required for the scripts to work in later versions of SharePoint.

##### Ready for the Cloud.
The Barista architecture is interoperable and independent of the SharePoint environment. Once you create Barista applications you can run them on premises or cloud hosted - providing you for maximum flexibility.
Future versions of Barista will wrap the SharePoint 2013 client object model, allowing for painless interaction with hosted SharePoint environments.

##### Summary
* Accelerates custom SharePoint application development.
* Reduced need for individual development environments for developers.
* Isolated development environment – Multiple developers are able to develop in a single environment.
* Ability to be agile with test/deploy.
* Minimize farm-wide deployments for application-level code.
* Ease of installation and operation.
* Scalable and expandable.
* Reduces the level of technical specialty needed in building SharePoint solutions. 
* Substantial cost savings in managing Farm Solutions in the enterprise. (Something about this being the same as the 2nd bullet point – how to find the value???)
* Cloud points and reduced confusion..
* Reduced costs Scripts are developed right in the browser – Visual Studio is not required for developing scripts.

For a technical summary, please see [how it works]({{ "/2014/11/04/barista-sharepoint-how-it-works.html" | prepend: site.baseurl | prepend: site.url }})

> Updated on 2016-07-30