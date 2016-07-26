---
layout: post
title:  "Barista Multi-Threading"
date:   2014-01-28
author: "Sean McLellan"
tags: ["Deferred bundle"]
---

Barista supports multi-threading with the "Deferred" bundle.

You can easily perform multi-threaded tasks that have traditionally been a pain to write.

If you're familiar with the Task Parallel Library in .Net, deferred borrows from these concepts. Deferreds are similar in nature to JavaScript promises, but have some differences -- A future version of Barista will most likely further borrow from the promise conventions or implement a .net based version of the 'Q' library.

When requiring the Deferred bundle, the following are added to the global scope:
Deferred() -- Creates a new Deferred Object
delay(...) -- Function that waits for the specified time.
waitAll(...) -- Function that accepts an array of deferred objects, and waits until each is complete.

To get started with Deferred, require the deferred bundle, and new up a Deferred object, passing in the function you want to execute asynchronously.

```javascript
require("Deferred");

var deferred = new Deferred(function () {
        var value = Math.floor((Math.random() * 1000) + 1);
        delay(value);
        return value;
});
```

Next, on the deferred object, define the behavior that occurs when the async function is done

```javascript
var results = []
deferred.done(function (result) { results.push(result); });
```

To synchronously wait for the deferred, call the .wait() function on the deferred object.


An end-to-end example:

```javascript
require("Deferred");
require("Moment");

var combined = [];
var calls = [];

for (var i = 0; i < 10; i++) {
    var deferred = new Deferred(function () {
        var value = Math.floor((Math.random() * 1000) + 1);
        delay(value);
        return value;
    });

    deferred.done(function (result) { combined.push(result); });
    calls.push(deferred);
}

var startTime = moment();
waitAll(calls);
var endTime = moment();

var total = endTime - startTime;

combined;

var sum = 0;
combined.forEach(function(c) { sum += c; });

var result = "Sequentially, this would have taken " + sum + "ms, but you only waited " + total + "ms";


result = {
    message: result,
    combined: combined
};
```

The SharePoint bundle can be used in conjunction with the deferred bundle to perform multi-threaded lookups on SharePoint.