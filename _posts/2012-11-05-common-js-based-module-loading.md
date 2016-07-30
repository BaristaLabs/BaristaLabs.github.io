---
layout: post
title:  "CommonJS Module Loading"
date:   2012-11-05
author: "Sean McLellan"
tags: ["Bundles"]
---
CommonJS-like functionality has now been integrated with Barista.

1. Node.js follows the CommonJS module system, and the builtin require function is the easiest way to include modules that exist in separate files. The basic functionality of require is that it reads a javascript file, executes the file, and then proceeds to return the exports object. (See http://nodemanual.org/latest/nodejs_ref_guide/)

2. Barista now has CommonJS like functionality, there are two global functions available: require(<string>) and listBundles();
3. Require follows the CommonJS concepts, except that instead of reading/executing javascript file, it imports functionality from the Barista library.
4. Require may perform multiple operations, such as adding additional ‘types’ to the current runtime.
5. E.g. to use SharePoint functionality in Barista, one will need to write:
```
var sp = require(“SharePoint”);
```
The inclusion of require and it’s functionality lets script developers specify the name of the variable that holds the export object, and minimizes the amount of global variables that would otherwise be included by default.

6. Require also allows for extensibility: an interface, IBundle, can be implemented by types. If a fully-qualified type name is passed as the string parameter, the type will be loaded and the export object returned. This lets additional bundles be deployed separately from Barista.

7. listBundles lists all bundles that are available:

![alt text](/img/2012-11-05-common-js-based-module-loading-01.jpg "List Bundles")

This is a breaking change, existing scripts will need to add the require statements at the top of the script

