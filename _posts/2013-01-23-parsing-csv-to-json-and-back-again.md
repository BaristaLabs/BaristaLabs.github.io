---
layout: post
title:  "Parsing CSV to JSON and back again"
date:   2013-01-23
author: "Sean McLellan"
tags: ["Document Bundle"]
---

The Document bundle now has routines to transform CSV to Json and back again.
 
So, given this Barista service:

```javascript
var doc = require("Document");
var csv = "Name, Age, Power\r\n\
    Sean, 20, 9001\r\n\
    Carl, 101, 9002\r\n\
                Frank, 9, 1000\r\n";
   
var result = doc.csv2Json(csv);
result;
```
 
The response is a JSON array.

```json
[
    [
        "Name",
        "Age",
        "Power"
    ],
    [
        "Sean",
        "20",
        "9001"
    ],
    [
        "Carl",
        "101",
        "9002"
    ],
    [
        "Frank",
        "9",
        "1000"
    ]
]
```
 
Since we have a header with our CSV, we can do one better – we can generate an array of JSON objects by specifying the “hasHeader” flag.

```javascript
var doc = require("Document");
var csv = "Name, Age, Power\r\n\
    Sean, 20, 9001\r\n\
    Carl, 101, 9002\r\n\
                Frank, 9, 1000\r\n";
   
var result = doc.csv2Json(csv, {hasHeader: true});
result;
```
 
 
Result:

```json
[
    {
        "Name": "Sean",
        "Age": "20",
        "Power": "9001"
    },
    {
        "Name": "Carl",
        "Age": "101",
        "Power": "9002"
    },
    {
        "Name": "Frank",
        "Age": "9",
        "Power": "1000"
    }
]
```

This works in conjunction with the other Bundles, for example, the SharePoint bundle.

```javascript
var sp = require("SharePoint");
var doc = require("Document");
 
var csvFile = sp.currentContext.web.getFileByServerRelativeUrl("/Documents/Cities.csv");
var result = doc.csv2Json(csvFile.openBinary(), {hasHeader: true});
result;
```

```json
[
    {
        "City Name": "Acton",
        "Criteria ID": "1018752",
        "DMA Region Name": "Portland-Auburn, ME",
        "DMA Region Code": "500"
    },
    {
        "City Name": "Albion",
        "Criteria ID": "1018754",
        "DMA Region Name": "Portland-Auburn, ME",
        "DMA Region Code": "500"
    },
    {
        "City Name": "Alfred",
        "Criteria ID": "1018755",
        "DMA Region Name": "Portland-Auburn, ME",
        "DMA Region Code": "500"
    },
    {
        "City Name": "Andover",
        "Criteria ID": "1018756",
        "DMA Region Name": "Portland-Auburn, ME",
        "DMA Region Code": "500"
    },
    {
        "City Name": "Auburn",
        "Criteria ID": "1018760",
        "DMA Region Name": "Portland-Auburn, ME",
        "DMA Region Code": "500"
    },
    {
        "City Name": "Augusta",
        "Criteria ID": "1018761",
        "DMA Region Name": "Portland-Auburn, ME",
        "DMA Region Code": "500"
    }
]
```
 
Bam – you’ve just created a simple REST service against a CSV file.
 
This works the other way as well. Given a JSON object, you can create a CSV file.

```javascript
var doc = require("Document");
 
var myArray = [{
    Hello: "World",
    Foo: "Bar"
}, {
    Hello: "Frank",
    Foo: "Baz"
}, {
    Hello: "Gina",
    Foo: "Boo"
} ];
 
var result = doc.json2Csv(myArray);
result;

```

```javascript
"World,Bar\r\nFrank,Baz\r\nGina,Boo\r\n"
```

Json2CSV also will output headers (based on the first object in the array)

```javascript
var doc = require("Document");
 
var myArray = [{
    Hello: "World",
    Foo: "Bar"
}, {
    Hello: "Frank",
    Foo: "Baz"
}, {
    Hello: "Gina",
    Foo: "Boo"
} ];
 
var result = doc.json2Csv(myArray, {hasHeader: true});
result;
```

```javascript
"Hello,Foo\r\nWorld,Bar\r\nFrank,Baz\r\nGina,Boo\r\n"
```
 
Other options include:

Specifying the delimiter
```javascript
{hasHeader: true, valueDelimiter: “’”}
```
 
Specifying the separator
```javascript
{hasHeader: true, valueSeparator: “\t”}
```
 
Preserve the leading/trailing whitespace
```javascript
preserveLeadingWhiteSpace, preserveTrailingWhiteSpace
```