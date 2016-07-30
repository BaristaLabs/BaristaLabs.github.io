---
layout: post
title:  "JavaScript Error Handling"
date:   2012-11-18
author: "Sean McLellan"
tags: ["Error Handling"]
---
One of the things that we was seeing in the SharePoint dumps is that Barista JavaScript errors get raised and logged as full-blown Exceptions.

Obviously is a bit sub-optimal since .Net has to unwind the stack trace, write it to the log, etc when these occur. Realistically, Barista JavaScript exceptions should not be considered 'System' exceptions in this manner and handled differently.


Another thing that was annoying is that when a JavaScript error occurred -- e.g. some uninitialized variable or a mis-matched closing token -- you'd have to hunt and peck in your source code to try and determine where the heck this was occurring.



Thus, I've made a few changes, Mostly because it's cold, windy, and rainy,(although I've been out on the beach 4 times already, and it's extraordinarily beautiful) but also since Stanley Jobston is my hero.


First: JavaScript exceptions no longer are true "System" exceptions, they get caught and the response is modified to display an error results page. They still get logged in the ULS under the "Barista" category, however, so they can be kept track of. 

Secondly, full stack traces with code location is now available in the JavaScript exception pages:

![alt text](/img/2012-11-18-javascript-error-handling-01.png "Detailed Error Messages")
 
![alt text](/img/2012-11-18-javascript-error-handling-02.png "Stack Traces")

This is also available when using the include(<ScriptUrl>) functionality so that a fully-modularized script will report the location of the exception and the file name where a javascript error occurred.

Hopefully this will make script debugging less annoying and make Barista more System-Administrator friendly.
