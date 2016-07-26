---
layout: post
title:  "Barista Fiddle Improvements"
date:   2015-06-20
author: "Sean McLellan"
tags: ["Barista Fiddle"]
---

So I had previously shifted emphasis to the SP2013 branch of Barista. With some potential work coming up that might use Barista SP2010 on a separate team, I've added some features to the SP2010 branch to make it easier for someone less versed with Barista and JavaScript to get immediate feedback. 

1) I've updated the UI slightly -- updating KendoUI and CodeMirror and other components that BaristaFiddle uses. This results in a slightly better experience overall, especially in down-level browsers, in terms of performance and fixing that annoying issue where the cursor position and the line would be slightly mismatched.

2) JSHint support.
  I've added the JSHint addin with CodeMirror to display JavaScript syntax errors within fiddle.
​  ![alt text](/img/2015-06-20-barista-fiddle-improvements-01.png "JSHint in Fiddle")

  This should help with those who might not have been working with JavaScript as long as we have fix common errors.

3) Autocomplete support.

Full autocomplete support in fiddle is now in. Completion displays when pressing ctrl-space (similar to visual studio) and a few other mechanisms described in the new help dialog:

​
![alt text](/img/2015-06-20-barista-fiddle-improvements-02.png "Keyboard Shortcuts")
​

Require displays available bundles:
​

![alt text](/img/2015-06-20-barista-fiddle-improvements-03.png "Bundle Population")

Completion on objects:
​

![alt text](/img/2015-06-20-barista-fiddle-improvements-04.png "Completion on Objects")

Function Parameter Suggestions:


​![alt text](/img/2015-06-20-barista-fiddle-improvements-01.png "Function Parameter Suggestions")


This should allow devs new to the barista object model discover the built-in functionality.
Caveats: Autocomplete is currently static -- E.g. bundles added via strongly-typed-assembly-name and included script files currently aren't pulled in. This is future functionality.


It's my hope that these improvements allow teams that might not have a "Sean-bot"(tm) to easily become familiar with the built-in functionality in Barista.

Namaste,