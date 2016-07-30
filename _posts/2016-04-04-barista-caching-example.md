---
layout: post
title:  "Barista Caching Example"
date:   2015-04-04
author: "Sean McLellan"
tags: ["Web Bundle"]
---

A customer recently wanted an example of using caching in Barista.

```javascript
var web = require("Web");
require("Moment");
var tomorrow = moment().add(1, 'hours').toDate();

//web.addItemToCache("test1234", "someData", tomorrow);
//web.removeItemFromCache("test1234");
//web.getItemFromCache("test1234");

var test = {
    now: new Date(),
    this: new Date((new Date()).getTime() + 1 * 60 * 1 * 60000),
    tomorrow: tomorrow
};

test;
```