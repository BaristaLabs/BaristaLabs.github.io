---
layout: post
title:  "Service to zip files in a doclib with Barista"
date:   2012-08-24
author: "Sean McLellan"
tags: ["Document bundle"]
---

A dose of espresso, now with Linq-like syntax, brewed up by your local barista:

```javascript
//Create a service that returns a zip file of all files within the root folder of the ‘OFSServicesUnitTests’ that start with the letter ‘B’

var list = new SPList("/Documents");
var files = list.rootFolder.getFiles(); //TODO: Change this to a CAML query with a built-in CAML builder so we’re not enumerating all items in the folder.
var bFiles = Enumerable.From(files)
                                                .Where(function(f) { return f.name.indexOf('B') == 0; })
                                                .ToArray();
var zip = new ZipFile();
bFiles.forEach(function(f) { //Using ECMA 5 ‘forEach’ function on Arrays
        zip.addFile(f.name, f.openBinary());
    });
web.response.contentType="application/zip"; //system knows about byte arrays now, and so ‘response.isRaw’ is no longer needed.
zip.finish();
```