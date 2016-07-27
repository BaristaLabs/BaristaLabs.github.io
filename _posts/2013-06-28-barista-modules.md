---
layout: post
title:  "Barista Modules Overview"
date:   2013-09-13
author: "Sean McLellan"
tags: ["Barista Modules", "Extensibility"]
---

The past few weeks have been tweaking and refining Barista as it gets used to build AMS 1.5. At this point I’m really ~~eating my own dogfood~~ drinking my own coffee, while simultaneously coming up with some solid patterns around “Barista App” design, file layout, and so-forth.
 
Some of the polish has been around better error handling if an error does occur, minor tweaks around the Document Store, improvements to the recently introduced ExcelDocument, better auto-setting the content-disposition header for return-types of Base64EncodedFile,  additional included JS files, and so forth. There’s been over 20 check ins since the last update.
 
 
One scenario that popped up as a potential area of improvement is around dependency loading.
 
#### New Define functionality in Barista
Take, for instance, a Barista application that contains functionality that is modularized into separate JS files.
 
Having separate script files makes an application easier to maintain for evident reasons.

![alt text](/img/2013-06-28-barista-modules-01.png "Code")
 
However, those includes at the top get a bit annoying to maintain across a number script files. Further, if a script dependency has its own includes, this potentially causes multiple loads of the same dependencies.
 
Barista already has the notion of “Bundles”, what I’m introducing here is the ability to create user-defined bundles.
 
 
Using the “define” top-level function, one can define bundles that in turn can have bundle dependencies of their own:
 
define([Bundle Name], [string Path to Script file –OR- Function], [Name of dependency, array of dependencies, or object with the property names being the name of dependency], [Bundle Description], [IsModule]);

Let’s restructure the references above to use the new dependency loading feature.
 
First, let’s define a top-level bundle for AMS:

![alt text](/img/2013-06-28-barista-modules-02.png "Code")

 
This defines a user-defined “ams” bundle there are a few things to note here:
1)      The “this” object within the function scope refers to the global object.
2)      Note the list of dependencies – the AMS bundle requires the Document Store and Active Directory bundles and specifies them in a way such that the objects that are returned by those bundles are injected into the scope.
 
If we now reference the common file that this definition lives in, we can see that the bundle is now listed in the available bundles:

![alt text](/img/2013-06-28-barista-modules-03.png "Code")
 
Let’s now start defining other common bundles – in this case we’re going to reference script files as the source – all of these bundles depend on the master AMS bundle, and the “ams-recon” bundle requires config and util.

![alt text](/img/2013-06-28-barista-modules-04.png "Code")
 
If we do our help function again, we’ll see the new bundles listed.

![alt text](/img/2013-06-28-barista-modules-05.png "Code")

We can start using our bundles with the “require” functionality already used. Note that requiring a bundle will pull in all dependencies for you.
 
So, our original example becomes:

![alt text](/img/2013-06-28-barista-modules-06.png "Code")
 
Thus, the definition and inclusions that recon has can change and evolve without needing to update all references everywhere.
 
We’re all fairly familiar with CommonJS and RequireJS – the functionality in Barista isn’t using either of those libraries so it does not attempt to enforce a hierarchical structure on references, in fact, the implementation allows for script references to be absolute paths to scripts hosted anywhere across the farm, relative paths to the current web, relative path to the current request, as well as files hosted as deployed files in the hive.
 
A similar improvement is to provide the same capability to 3rd party Bundles – A third-party bundle is a .net type that implements the IBundle interface. Third-party bundles can also be defined by specifying the fully-qualified type name as the first “define” parameter.

![alt text](/img/2013-06-28-barista-modules-07.png "Code")
 
 
There’s still a good bit of work to do to add functionality to Barista to make it an enterprise-level app platform, and make developing for SharePoint, or SharePoint 2013 apps, less about the plumbing and more about the business.