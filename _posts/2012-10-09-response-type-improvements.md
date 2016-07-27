---
layout: post
title:  "Response Type Improvements"
date:   2012-10-09
author: "Sean McLellan"
tags: ["Web Bundle"]
---
It’s been about a month, and it feels really great to get back behind the espresso machine.

Barista was originally created with the thought that it would be, in most cases, be a way to create JSON services, however, based on today’s Barista session, I’ve made some improvements to allow Barista to easily create services with other types in mind (HTML, Binary, Xml, etc)

This is great, since we’re vetting the possibilities and exploring more of the potential uses of Barista as we beta it.

Improvements:

1)	Output content type is auto-detected.
a.	Barista knows about Html, Xml, JSON, and Binary data and will set the content-type of the response appropriately.
b.	This behavior can be overridden by specifying the desired content type with web.response.contentType = “…” and setting web.response.autoDetectContentType to false.
2)	Output Stream type is auto-detected.
a.	If the output type is a JSON object, it stringifies and serializes as text, if it is a string, it serializes as text, if it’s a binary object, it outputs as base64.

With the above features, today’s script can be (psudo) simplified to:

```javascript
var htmlResult = sp.getFileAsString(<Url>);
htmlResult = htmlResult.replace("{{key}}", "Value"); //TODO: Use Mustache instead.
htmlResult;
```

Barista will detect that the content type is text/html (given that the file contains <html> as a start tag) and will return the raw text, rather than the JSON-escaped text string.

3)	Helper feature to get the full URL to execute the current script with the Barista service – makes it easy to figure out what to paste into that CEWP url field.

a.	Knows if the script is saved, if it is, generates the url with the script path, otherwise, uses the full script as an inline-url eval.

![alt text](/img/2012-10-09-response-type-improvements-01.png "Eval URL")

b.	 

![alt text](/img/2012-10-09-response-type-improvements-02.png "Eval Dialog")

4)	For html responses from the Barista service, the result pane will switch modes and the result will be rendered within an iframe. Makes it easier to visualize and debug HTML based results.
 
![alt text](/img/2012-10-09-response-type-improvements-03.png "HTML Response")
