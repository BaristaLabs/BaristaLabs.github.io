---
layout: post
title:  "Barista Improvements"
date:   2013-04-20
author: "Sean McLellan"
---

Had some time to enact some additions and fixes that have been piling up over the week:
 
Deployment
---
* The deployment scripts have been updated and are better. There is now a single deployment script/bat file that can be called via Deploy.bat and it’ll do everything that’s needed to install Barista while you wait.
  * The deployment scripts will attempt to stop anything that might be referencing the barista.core.dll/barista.sharepoint.core.dll/barista.sharepoint.dlls to minimize locked assemblies in order to minimize potential orphaning – this doesn’t mean that locked assembiles won’t still occur however…
  * The deployment scripts now deploy the Barista Search Service and the Barista Web Socket Service.
 
* The Visual Studio SharePoint solution deployment functionality (Right-Click Deploy) has been updated to call all the scripts as if the scripts themselves were run.
  * here was some flakiness with how CKSDev was calling the PS1 scripts that was a load of frustration, I’ve taken these out and I’m just calling “master” scripts in the pre/post deployment steps of the SharePoint project tab. In fact, CKSDev isn’t a dependency anymore (although it’s still nice for right-click attach to w3wp… get the CKSDev 2012 version for a slimmed down CKSDev)
  * You can right-click and select “Deploy” on Barista.SharePoint and it’ll do as you expect, deploy… with none of the previous flakiness.

Active Directory
---
* The getADUser() should have had a single parameter which allows a loginName to be passed and a single result returned, if no loginName is passed (or null or whitespace) the current user is returned. The method with the single parameter was not being exposed correctly

![alt text](/img/2013-04-20-barista-improvements-01.png "getADUser()")

  * Please use getADUser(<loginName>) over searchAllUsers(<loginName>)
  * Internally, the Active Directory bundle has been refactored and the bundle brought to the Core project (for future inclusion in non-sharepoint barista)
 
String bundle
---
* The string library has a reference to ‘window’ which, well, just initially work in Barista as there is no window object.
* For the simple case, the string bundle is overkill. I’ve added the following capabilities to the built-in string javascript prototype to make things more like .Net
* .startsWith, .endsWith
* String.format.
* Keep in mind that this is Barista specific, and won’t work anywhere else (unless you use a library such as http://stackoverflow.com/questions/2534803/string-format-in-javascript)
  * This allows for the following:

![alt text](/img/2013-04-20-barista-improvements-02.png "string.format()")
 
* The string bundle itself is now functional, I’ve updated it to 1.3.0: (See http://stringjs.com/ for use)

![alt text](/img/2013-04-20-barista-improvements-03.png "string bundle")
 
Sugar bundle
---
* The SugarJS library is now available as a Barista Bundle

![alt text](/img/2013-04-20-barista-improvements-04.png "sugar bundle")
 
A plethora of changes are coming in the Search bundle… new post to come…