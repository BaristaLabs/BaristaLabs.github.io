---
layout: post
title:  "SP JSOM vs Barista"
date:   2013-04-25
author: "Sean McLellan"
---

Recently it came up on how to retrieve the urls to pictures in a pictures library.
 
In Barista, that code looks like the following.

```javascript
require("Linq");
var sp = require("SharePoint");
var pictures = sp.currentContext.web.getListByTitle("Images");
var items = pictures.getItems();
var result = Enumerable.From(items)
    .Select(function(i) { return i.fieldValues.EncodedAbsUrl; })
    .ToArray();
 
result;
```

producing a reusable service that’s pretty easily maintained, if this data gets used by multiple pages, and the requirements change, say, to update to filter by some criteria, I can update the script once, and all the consuming pages are updated.
 
 
I can query it with one line of jQuery, and use promises to succeed/fail.

```javascript
$.getJSON(“/_vti_bin/Barista/v1/Barista.svc?c=script.js”)
.success(function(data) { alert(data); }
.error(function(data) { alert(“an error has occurred…”); }
 
I can create a unit test to make sure this is working.
//Include qunit
asyncTest( "Test to verify that I can retrieve images", 1, function() {
  $.getJSON(“/_vti_bin/Barista/v1/Barista.svc?c=script.js”)
    .success(function(data) {
    if (data.length > 1)
         ok( true, "image data has been returned by the service." );
  })
    .error(function() {
        ok(1!=1, “the image service failed.”);
   });
});
```

I set out to use the SharePoint JavaScript Client Object Model to do this… why re-invent the wheel when there’s the web…
 
I came across a similar question on stack exchange… no response.
http://sharepoint.stackexchange.com/questions/36844/to-get-the-url-of-a-document-item-using-javascript-by-passing-id-of-the-docume
 
I came across this to use the SharePoint 2013 REST object model….
http://blogs.msdn.com/b/uksharepoint/archive/2013/02/22/manipulating-list-items-in-sharepoint-hosted-apps-using-the-rest-api.aspx
 
 
Not it either… Well, ok, I guess I’ll code it using the code straight from the horse’s mouth, and modifying it from there
http://msdn.microsoft.com/en-us/library/hh185007(v=office.14).aspx
 
 ```javascript
function retrieveListItemsInclude() {
 
    var clientContext = new SP.ClientContext();
    var oList = clientContext.get_web().get_lists().getByTitle('Images');
 
    var camlQuery = new SP.CamlQuery();
    camlQuery.set_viewXml('<View><RowLimit>100</RowLimit></View>');
    this.collListItem = oList.getItems(camlQuery);
 
    clientContext.load(collListItem, 'Include(Id, DisplayName, EncodedAbsUrl)');
 
    clientContext.executeQueryAsync(Function.createDelegate(this, this.onQuerySucceeded), Function.createDelegate(this, this.onQueryFailed));
}
 
function onQuerySucceeded(sender, args) {
 
    var listItemInfo = '';
    var listItemEnumerator = collListItem.getEnumerator();
       
    while (listItemEnumerator.moveNext()) {
        var oListItem = listItemEnumerator.get_current();
        listItemInfo += '\nID: ' + oListItem.get_id() +
            '\nurl: ' + oListItem.get_fieldValues()["EncodedAbsUrl"];
    }
alert(listItemInfo);
}
 
function onQueryFailed(sender, args) {
 
    alert('Request failed. ' + args.get_message() + '\n' + args.get_stackTrace());
} retrieveListItemsInclude();
```

Ok, that was a mouthful.

One can easily see how Barista scripts are more easily created and maintained over the SP JSOM.