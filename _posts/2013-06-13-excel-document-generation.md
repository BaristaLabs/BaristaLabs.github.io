---
layout: post
title:  "Excel Document Generation"
date:   2013-06-13
author: "Sean McLellan"
tags: ["Document Bundle"]
---

This will be a slightly abridged post as my eyelids are drooping – but I just checked in some improvements to Barista in relation to the ability to read/write to Excel Documents.
 
This functionality is centered around various enterprise-y use cases around Excel Documents – Creating Excel-based rep rts on the fly, manipulating existing spreadsheets, and so forth.
 
After including the “Documents” bundle, there is a top-level object called “ExcelDocument” … an empty constructor can be passed, a Base64ByteArray instance (From web.request, or various other sources), Url to a SPFile or a SPFile Previously retrieved.
 
![alt text](/img/2013-06-13-excel-document-generation-01.png "Code")

Once you have an excel document object, you can interact with excel docum nts. Since this is Barista, effort has been made to make working with the exceldocument as Javascript friendly as p ssible. For instance, there is a helper method to get the contents of a worksheet as a Json object. For instance, in a barista script responding to a POST from an &lt;input type=’file’&gt;…

![alt text](/img/2013-06-13-excel-document-generation-02.png "Code")
 
 
As expected, there are also mechanisms to set cell values.

![alt text](/img/2013-06-13-excel-document-generation-03.png "Code")
 
There’s also a mechanism populate a worksheet from a Json array:

![alt text](/img/2013-06-13-excel-document-generation-04.png "Code")
 
 
The getBytes() function on the excel document object will return a Base64ByteArray which can be used to return the contents of the Excel Document as the response, allowing the user to download the document.
 
 
That’s about it for now – there’s also functions which allow for copying spreadsheets (tabs) between excel documents, reorganizing them, etc.
 
 
Use the help functionality for more info – let me know if there are any use cases that I’ve missed with the implemented functionality or if you could get me to the coast of somewhere beautiful.
 
 
Night,