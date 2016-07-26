---
layout: post
title:  "Custom Barista Packages"
date:   2015-05-09
author: "Sean McLellan"
tags: ["Extensibility"]
---

I've introduced a managed way of adding packages to Barista. A barista package is a zip file that contains assemblies that contain one or more IBundle implementations. This allows additional bundles to be deployed and automatically registered with Barista without a GAC or SharePoint deployment.

Here's how it works:

1) A farm admin runs a SharePoint PowerShell command to deploy a package:
![alt text](/img/2015-05-09-custom-barista-packages-01.png "Deploy Barista package")

2) In the Barista service application management screen, the farm administrator approves the package for use:
![alt text](/img/2015-05-09-custom-barista-packages-02.png "Approve Barista Package")
​
While packages are deployed on a farm-wide basis, package approval is a Service Application scoped operation. Thus, one service application could be restricted to use certain packages, and another, another set.

3) Once approved, the bundles contained within the package appear in the list of available bundles
​![alt text](/img/2015-05-09-custom-barista-packages-03.png "Use Barista Package")