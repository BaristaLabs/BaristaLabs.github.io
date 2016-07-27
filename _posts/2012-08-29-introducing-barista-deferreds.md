---
layout: post
title:  "Introducing Barista Deferreds"
date:   2012-08-29
author: "Sean McLellan"
tags: ["Deferred"]
---

One of the concepts that JavaScript developers are familiar with is the concept of deferreds.

jQuery implements deferreds in a number of places, most commonly used when invoking an asynchronous ajax call and the developer wishes for some behavior to be run when the operation is complete:

http://api.jquery.com/category/deferred-object/

Example: Since the jQuery.get method returns a jqXHR object, which is derived from a Deferred object, we can attach a success callback using the .done() method.

```javascript
$.get("test.php").done(function() { 
  alert("$.get succeeded"); 
});
```


Yesterday, I introduced the concept of Deferreds to Barista. In Barista, since the javascript engine is tied to .Net we can utilize multi-threading. In Barista, creating a new deferred with a function as a constructor parameter will actually invoke that function on a new thread.

Since the executing script may potentially complete prior to an asynchronous task being completed, we also have a global function called ‘waitAll’ which will accept a single, or array, of deferreds and wait until all deferreds have completed.

Example: In a new thread, randomly wait for a period of time. When complete, set the value of ‘result’ to the number of milliseconds waited.

```javascript
var result;

var deferred = new Deferred(function () {
        var value = Math.floor((Math.random() * 1000) + 1);
        delay(value);
        return value;
    }).done(function (value) {
                result  = value;
});

waitAll(deferred);

result;

//Returns the number of milliseconds delayed.
```

This makes multi-threaded programming using barista pretty easy – multi-threaded web traversals to gather security or report on lists/files, or menu permissions checking, or other scenarios are that much more performant since they’re multi-threaded.


As part of the deferred work, I’ve reworked the Ajax object to actually return a deferred object when { async = true } on the parameter object. This leads to some interesting scenarios:


Example: This script will recursively call itself to perform a traversal of all child webs in the context as a flattened array.

```javascript
var result = new Array();
var calls = new Array();
var webs = sp.currentContext.web.getWebs();
webs.forEach(function (w) {
    result.push(w.url);
    var deferred = web.ajax(w.url + "/_vti_bin/OFS.SharePoint.Services.Rest/Barista.svc/eval?c=" + encodeURIComponent("/_layouts/OFS.SharePoint.Services.UnitTests/Content/Barista_RecursiveWebsRetrieval.js"), { useDefaultCredentials: true, async: true });
//Todo: make getting the url of the currently running script easier

    deferred.done(function (data) {
        if (data.length > 0) {
            data.forEach(function (url) {
                result.push(url);
            });
        }
    });
    calls.push(deferred);
});

waitAll(calls);

result;
```

//Example result ( I have just three randomly named webs off my root web in the root site – everything else is a site collection):

```json
["http://ofsdev/hTkCFsUfIPhVyFa",
"http://ofsdev/okHKxJAHoSPjOHS",
"http://ofsdev/kKWEHVvsLmtsOCQ",
"http://ofsdev/hTkCFsUfIPhVyFa/deepweb"]
```

It’s worthwhile to discuss what could happen when this script is executed to examine the potential benefits.

* The script will asynchronously wait for responses from all recursive calls
* Since WCF allows for a thread to be automatically created to handle to a request, a new thread will be created when the child ajax request comes in (up until the max threads configured in wcf/iis)
* Since each call to the service is a new session, a properly-configured load balanced environment would potentially round-robin the requests to other WFEs in the farm.

So, in essence, this script is not only multi-threaded in that each child web is processed in a new thread, but it also has the potential of delving out work to other servers in the farm such that each WFE is tasked with performing a portion of the total work – e.g. # of child webs/number of servers in the farm.

Well that’s pretty cool.

The deferred implementation in Barista has a subset of the methods that can be used with jQuery – I don’t have any of the ‘progress’ type functions, or cancellation. However, from what is implemented, I think this opens up a number of ‘advanced’ scenarios and makes creating multi-threaded tasks easy, and especially makes aggregation of data a really interesting scenario.

Next up: Logging.