---
layout: post
title:  "Barista Web Optimization"
date:   2014-03-09
author: "Sean McLellan"
tags: ["Web Optimization bundle"]
---

Now that the dust is slowly starting to settle on a particular Barista client, we're looking at various perf characteristics of the application and finding ways to improve it.

The first thing that we noticed is that the way that AngularJS recommends that you divide up your Controllers/Services/Directives leads to having a large number of files that are loaded by the browser as script references. Future versions of Angular will introduce Lazy Loading -- require.js-like functionality to dynamically load files when necessary, but future versions of Angular will also remove IE8 testing support.

Bundling the application's files at deploy time is something that we thought of as well, but we find that having the flexibility to hotfix files on the fly is a very compelling story, and we didn't want to eliminate this ability, or make this more complicated than necessary.

Thus we wanted to do something now to help reduce the number of requests due to script file loading, but not wait for an AngularJS to update.

##### Barista to the rescue.

To accommodate this need, I've introduced the Web Optimization bundle to Barista. This lets us reap the benefits of optimization work not only in AMS, but in any application that uses Barista.

The Web Optimization bundle includes various mechanisms to help optimize web requests. For bundling, you define a bundle definition file, and Barista will combine the files in the definition into one file.

E.g.:

```javascript
var wo = require("Web Optimization");

var bundleDef = '<bundle>\
  <file>/AMS/Scripts/Vendor/jquery/jquery-1.10.3.js</file>\
  <file>/AMS/Scripts/Vendor/jquery-ui/jquery-ui-1.10.3.custom.js</file>\
  <file>/AMS/Scripts/Vendor/jquery-layout/jquery.layout-latest.js</file>\
</bundle>';

wo.bundle(bundleDef, "jquery-bundle.js");
```

The bundle function is intelligent in that is caches the files and just checks timestamps the next time the files are bundled -- this also lets us craft the response to return a 304 if the bundle is not modified, thus letting us rely on http-level caching:

```javascript
var wo = require("Web Optimization");
var web = require("Web");

var sp = require("SharePoint");
var amsBundleDef = sp.loadFileAsString('/AMS/Scripts/ams-app.bundle');
var unmodified = false;
var update = true;

if (barista.isDefined(web.request.headers["If-Modified-Since"])) {

    var ifModifiedSince = new Date(web.request.headers["If-Modified-Since"]);
    if (wo.hasBundleChangedSince(amsBundleDef, ifModifiedSince) == false) {
        unmodified = true;
        update = false;
    }
}

var result = null;
if (unmodified) {
     web.response.statusCode = 304;
    web.response.expires = 31536000;
    web.response.lastModified = new Date(web.request.headers["If-Modified-Since"]);
else {
    var bundle = wo.bundle(amsBundleDef, "ams-bundle.js", update);
    web.response.setHeader("Content-Disposition", "inline");
    web.response.expires = 31536000;
    web.response.lastModified = bundle.lastModified;
    result = bundle.data;
}

result;
```

Additionally, the Web Optimization bundle has the capability of minifying CSS and JavaScript files on the fly. This can be be performed either via the following functions:

```javascript
var wo = require("Web Optimization");
wo.minifyJs(jsString);
wo.minifyCss(cssString);
```

or, setting the 3rd parameter to true on the bundle function:

```javascript
var bundle = wo.bundle(amsBundleDef, "ams-bundle.js", update, true);
```

Finally, the ability to GZip the response has been added:

```javascript
var result = wo.gzip(result);
```

With these additions we find the following benefits with our Angular-based application:

*The number of requests for application js files has been reduced from 127 individual requests for JS files down to 2.
*The number of css requests has been reduced from 18 down to 2.

Overall file transfer when loading the app has been reduced as well:

Bundled Without GZip/Bundled + GZip/Bundled + Minified + GZip:
 

![alt text](/img/barista-web-optimization.png "Barista Web Optimization")


Future additions to the Web Optimization bundle may include the ability to create sprites on the fly, image resizing and so forth.


###### Summary

Web applications benefit greatly from standardized optimization techniques --  by adding these techniques to Barista, we leverage the capability across a number of apps, and eliminate the need of reproducing this type of code.