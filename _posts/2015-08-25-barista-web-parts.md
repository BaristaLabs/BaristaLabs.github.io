---
layout: post
title:  "Barista Web Parts"
date:   2015-08-25
author: "Sean McLellan"
tags: ["Web Parts"]
---

We recently had a question about the difference between the Web Parts that Barista includes

Barista installs with two web parts that have slightly different intents:

##### Barista Web Part
Lets you execute barista code, defined in its web part properties and render the html based result as the web part contents. It invokes the barista service server-side, so there is no additional request.

##### HTML Viewer Web Part
Created to overcome some of the limitations of the page viewer web part. Given an arbitrary url, it will get and render the response, optionally controlling things like retries, request proxy, converting relative urls in the response to absolute, and so forth.