---
layout: post
title:  "Example Barista CAML Query"
date:   2012-11-09
author: "Sean McLellan"
tags: ["SharePoint Bundle"]
---

Barista ships with SPCamlQueryBuilder included with the SharePoint bundle. Here's an example:

```javascript
var sp = require("SharePoint");

var camlBuilder = new SPCamlQueryBuilder();
var caml = camlBuilder.Where()
        .TextField('FileLeafRef').BeginsWith('B')
        .ToString();

var list = new SPList("/Lists/BaristaUnitTests");

var bListItems = list.getItemsByQuery(caml);
bListItems
```