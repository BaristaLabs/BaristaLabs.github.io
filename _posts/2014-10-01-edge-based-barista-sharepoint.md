---
layout: post
title:  "Edge Based Barista SharePoint"
date:   2014-10-01
author: "Sean McLellan"
tags: ["edgejs"]
---

Barista currently uses Jurassic as its JavaScript compiler, it's been extremely nice but recently I've been thinking about using Node.js within the pipeline. Some advantages of this would be better performance and the ability to use the rich ecosystem of node.js libraries available. SharePoint will still be exposed through the object model using edge.js.

What I'm thinking of at a High-Level:
![alt text](/img/2014-10-01-edge-based-barista-sharepoint-01.png "High-Level Barista-SharePoint-Edge-Node diagram")

Barista gets registered as a Provider Hosted app in a SharePoint environment.
Barista exposes a set of JSON endpoints via Node/Express.

1. Requests coming into the Barista endpoints get authenticated via sharepoint-passport.
2. Barista performs requests via the SP Client Object Model to retrieve the relevant script files hosted in SharePoint.
3. These scripts are executed in a JavaScript sandbox (Hosted either through DukTape, or leveraging Node's VM capabilities via Jailed (https://github.com/asvd/jailed​) or another mechanism, perhaps custom... separate discussion).
  * The sandbox only has minimal capability injected into it -- similar to what the current Barista sandbox provides -- A limited require capability to load bundle dependencies, and a barista global.
  * Sandbox has a max execution time. Optimally, other thresholds could be tracked (Memory usage/CPU).
  * Sandbox interacts with existing Barista .net bundles (Document, and so forth) via Edge.js.
4. Scripts are evaluated in the sandbox and the response returned.

Other node modules can be exposed to the sandbox with minimal effort -- I envision a administrator-editable whitelist of modules that are exposed to the sandbox with a curated default list.

An additional capability would be to have a Barista "Hub" that would be an install on on-prem environments that exposes more of the server-side object model to Barista, but isn't directly accessible. Pie-in-the-sky, this could utilize a reflection based design that once installed, could just simply call and transform/return the result. Alternatives would be a t4-based template that generates an interface.