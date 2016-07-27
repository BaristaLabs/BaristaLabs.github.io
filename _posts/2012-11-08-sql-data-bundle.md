---
layout: post
title:  "Introducing SQL Data"
date:   2013-01-22
author: "Sean McLellan"
tags: ["SQL Data Bundle"]
---
Finally got some time this evening to add a new feature to Barista: The ability to query sql data.

All that needs to happen is to require("Sql Data") and pass a parameter to DynamicModel.
From there, use "query" and pass it a sql statement, you'll get JSON back.

![alt text](/img/2012-11-08-sql-data-bundle-01.png "JSON Response")

but there's more:

Setting the properties "tableSchema" and "tableName" will allow to use some convenience methods:

"all" will return all records in the specified table.
If a JSON object is passed as a parameter, easy ordering/filtering can will occur:
Possible options: limit, orderBy, columns, args

![alt text](/img/2012-11-08-sql-data-bundle-02.png "Query")

"paged" will return a paged representation of the data. It defaults to a page size of 20.
Similarly to 'all', if a json object is passed, additional filtering can occur.
Options: sql, primaryKeyField, where, orderBy, columns, currentPage, pageSize, args.

![alt text](/img/2012-11-08-sql-data-bundle-03.png "Paged Query")

Barista knows about the SQL Binary data type, so you can create a SQL Image browser if you wanted to.

![alt text](/img/2012-11-08-sql-data-bundle-04.png "Binary Data Type")

There's support for Insert, Update, Delete, Scalar, Single…
 
![alt text](/img/2012-11-08-sql-data-bundle-05.png "Insert")
![alt text](/img/2012-11-08-sql-data-bundle-06.png "Retrieve")
![alt text](/img/2012-11-08-sql-data-bundle-07.png "Update")
![alt text](/img/2012-11-08-sql-data-bundle-08.png "Retrieve")
![alt text](/img/2012-11-08-sql-data-bundle-09.png "Delete")
![alt text](/img/2012-11-08-sql-data-bundle-10.png "Query")
 
 



The "Schema" property will return the schema of the table you're interested in.

![alt text](/img/2012-11-08-sql-data-bundle-11.png "Schema")