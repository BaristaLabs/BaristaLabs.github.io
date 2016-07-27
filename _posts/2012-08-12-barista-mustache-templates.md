---
layout: post
title:  "Barista Mustache Templates"
date:   2012-08-12
author: "Sean McLellan"
tags: ["Document Bundle"]
---

Managed to get a quickie feature in this morning before we all tripped and fell down a rabbit hole.

Templates are a pretty common and understood feature. In Barista, we should be able to pass a Json object to a template and generate a result.

Mustache (named for the curly-brace, turned sideways) is pretty common to node.js+javascript devs. There’s a bunch of implementations out there (including one for .net)

http://mustache.github.com/

In essence of time, I won’t go into the syntax (see website) but the important part is that now Mustache is a default feature within Barista. Only one minor change to the javascript file was required for it to run in the Jurassic script engine.

Example: Load a template from a doc lib, pass it some Json, return the generated Html. (Template ripped from the web site)

```javascript
var template = sp.loadFileAsString('/Lists/MyFiles/Content/DemoTemplate.htm');
var json = {
  "header": "Colors",
  "items": [
      {"name": "red", "first": true, "url": "#Red"},
      {"name": "green", "link": true, "url": "#Green"},
      {"name": "blue", "link": true, "url": "#Blue"}
  ],
  "empty": false
};

var html = Mustache.to_html(template, json);
web.response.contentType = “text/html”;
html;
```

Bam! You have an html server!


Example: Load a template from a doc lib, pass it some Json, send the generated html as the body of an email.

```javascript
var user = new SPUser("Domain\\User");
var template = sp.loadFileAsString('/Lists/MyFiles/Content/SweepstakesWinnerNotification.htm');
var json = {
  "name": user.name,
  "amountWon": "10,000,000",
};

var html = Mustache.to_html(template, json);
sp.sendEmail(user.email, '', '', 'MoneyRoom@mydomain.com', 'You’ve just won the Treasury sweepstakes!!', html);
```

Bam! You have a spam server!


And of course this works with the existing PDF capability.

```javascript
var template = sp.loadFileAsString('/Lists/MyFiles/Content/TeleworkForm.htm');
var json = {
  "name": user.name,
  “data”: doc.xml2Json(sp.loadFileAsString(‘/Lists/TeleworkForm/’ + user.loginName + ‘.xml’)),
};

var html = Mustache.to_html(template, json);
doc.html2Pdf(html);
```

And you've got a dynamic PDF generation capability.

We could integrate a more advanced templating engine (https://github.com/andyedinborough/RazorJS ?? ) but I think this is good enough… I’m thinking about xls capability (http://closedxml.codeplex.com/ is the top contender) so this should be good for now.

I love it when new features are easily added and add capability to existing functionality. Whoohoo.

Suggestions?

Next Up: Barista Fiddle (big feature).
