---
layout: default
title: Example
permalink: example/
---

``` js
var clientContext = new SP.ClientContext();
var targetList = clientContext.get_web().get_lists().getByTitle('Ticker and Outage');
var view = targetList.get_views().getByTitle("Home Page - Outage");
var query = new SP.CamlQuery();
clientContext.load(view);
clientContext.executeQueryAsync(Function.createDelegate(this, 
	function(){ 
        	query.set_viewXml(view.get_htmlSchemaXml()); 
                listItems = targetList.getItems(query);
                clientContext.load(listItems, 'Include(LinkTitleNoMenu, ID, Announcement_x0020_Start, Announcement_x0020_End, Event_x0020_Start, Event_x0020_End)');
                clientContext.executeQueryAsync(Function.createDelegate(this,this.onQuerySucceeded), Function.createDelegate(this, this.onQueryFailed));      
                  }), 
        	Function.createDelegate(this, function(){ alert('error loading view');})
);        
```

```js
var list = new SPList("/Lists/TickerAndOutage");
list.getItemsByView("Home Page - Outage");
```