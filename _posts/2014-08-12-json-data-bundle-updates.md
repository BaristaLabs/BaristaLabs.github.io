---
layout: post
title:  "JSON Data Bundle Updates"
date:   2014-08-12
author: "Sean McLellan"
tags: ["JSON Data Bundle"]
---

Barista has the Json Data bundle to help with tasks associated with manipulating/validating Json data.

Currently, the Json Data Bundle includes the JsonDataHandler library and the Automapper library (http://johnkalberer.com/2011/08/24/automapper-in-javascript/)

​
Today, I've added a few new capabilities to the Json Data bundle.

The first is a native "Merge" capability that increases performance with merging Json Objects together. This is exposed as a new function on the Object prototype:

![alt text](/img/2014-08-12-json-data-bundle-updates-01.png "Code")

![alt text](/img/2014-08-12-json-data-bundle-updates-02.png "Result")

Another new capability is support for JSONPath (see http://goessner.net/articles/JsonPath/) which is like XPath for JSON Objects.

Ex:

![alt text](/img/2014-08-12-json-data-bundle-updates-03.png "Code")

![alt text](/img/2014-08-12-json-data-bundle-updates-04.png "Result")

​

Finally, JsonSchema (http://json-schema.org/) support has been added:

![alt text](/img/2014-08-12-json-data-bundle-updates-05.png "Code")

![alt text](/img/2014-08-12-json-data-bundle-updates-06.png "Result")
 


isValid2 will return an object that indicates what the errors are.

![alt text](/img/2014-08-12-json-data-bundle-updates-07.png "Code")

![alt text](/img/2014-08-12-json-data-bundle-updates-08.png "Result")
 


That's all for now!