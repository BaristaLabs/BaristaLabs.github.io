---
layout: post
title:  "Introducing the Barista Search Index"
date:   2013-04-26
author: "Sean McLellan"
tags: ["Barista Search Index Bundle"]
---

Say you have a set of data you would like to be able to easily query – the data may be a aggregation of a number of list items from separate lists, sets of data stored in a Barista document store, information stored in XML files and not otherwise promoted to fields of list items – for instance, infopath forms stored in SharePoint, data gathered from the web via Ajax calls, data from SQL server surfaced via the SQL Data bundle and so forth.
 
The Barista Search Index bundle allows loosely typed content to be stored and, later, queried upon. Note that the search index should not be used as a database – in the architecture of your data storage solution, your persistent data store should be separate from the search index so that you can re-create your index from your persistent store. Also, the Barista Search Index is an Information Retrieval system - -this means it has slightly different semantics than you would expect when working with a traditional database. Further, since the  Barista Search Index is loosely typed, it works with any JSON document without needing to define a schema first – as long as you have a JSON document and you specify an id field, that JSON object can be added to the index. This is good since most of the functionality in Barista (including the retrieval of list items, and the ability to read and parse XML documents and Feeds) works with JSON objects. Note that in its current incarnation, there isn’t a way to index binary document formats such as DOCx, PDF, XLSx and so on – future capability might be added to support this through IFilters, but for now, full-text indexing of document formats is out of scope.
 
To get started with the Barista Search Index bundle, you’ll need to set up a search index directory. A directory is a location where the actual files that store the index are kept. Setting up an index is done through the Barista Service Application management page in Central Admin.
 
In the following screenshot, I have a number of indexes set up, mostly for test purposes. You’ll notice that there’s three types of indexes to choose from, RAM Directory, which is a volatile, but fast index, File Directory, which stores files on the file system or a network share, and the SharePoint Directory which stores files within SharePoint. The SharePoint directory is the slowest of the three – all that SQL I/O doesn’t come cheap.

![alt text](/img/bsa-01.png "Barista Search Locations")

 
For this example, we’re going to use the Barista Search Test index. Note that since we’re using a RAM directory we don’t need to define a storage path to use, since everything is in RAM. If the SPBaristaSearchIndex service is stopped or restarted, the index will be reset.
![alt text](/img/bsa-02.png "Barista Search Setup")
 
When you create the index, remember the name of the Index. The index name serves as the key.

 
Once we’ve got the index defined, we can hop on into Barista Fiddle and start coding away. Let’s start by adding some items into the index. Let’s pretend that we’re making a website for CarMax – we want to index some information about cars and then query upon them. We’ll hard code the data for now, but the data could be stored as list items, in a database, or pulled from an external web service – whatever would make sense for the actual application. I’m going to require the “Barista Search Index” bundle, store the result as ‘search, set the indexName property to the name of the index that we’ve previously set up, and add a document to the index.

![alt text](/img/bsa-03.png "Add document to index")
 
You’ll note that I’ve added an “@id” property as part of my JSON object – this is to uniquely identify records in the index and is required – the index property can be any string – this might mimic the primary key of the original data source or could be something more descriptive such as “cars/1” to denote type or denote a hierarchy. We’ll just use “1” for now. Cost units are BitCoins – the universal currency of the interwebs.
 
If we execute this, we’ll have one car in the index, but that’s not exciting – let’s add a couple more cars.

![alt text](/img/bsa-04.png "More Cars")
 
If you index a document that has an id of an existing document, you’ll overwrite the current document with the new document. Documents can be removed by the deleteDocuments and deleteAllDocument functions on the index searcher object.
 
 
Alright! Our New/Used car business is starting to burn rubber!! Let’s start querying for cars!
 
To perform queries, you’ll need to use the “search” function that’s defined on the indexer searcher object – the object that got returned to us from the require(“Barista Search Index”). The quickest way to get started querying objects is by using the text-based query syntax.
 
Let’s perform a search for all jeeps in the index. To do this, we’ll use “make” as the field we’re searching on, and will specify jeep as the value, as in the following:

![alt text](/img/bsa-05.png "Search Query")
 
If we execute this now, we’ll get this back:

![alt text](/img/bsa-06.png "Query Result")
 
Not bad! What was returned to us is the score of the document, the internal lucene document id, the document id that we specified, metadata and the actual JSON object that we originally passed in. We’ll be discussing each later on.
 
Now the text-based query syntax is pretty powerful – we can do joins with “AND” and “OR”

![alt text](/img/bsa-07.png "Search Query")
 
![alt text](/img/bsa-08.png "Query Result")

 
You are able to nest them, chain them, and so on. We can use double quotes to indicate phrases:

![alt text](/img/bsa-09.png "Search Query")

![alt text](/img/bsa-10.png "Query Result")
 
Another way to query is by using the query objects that are defined on the index searcher object. If we do a “help” on the index searcher object we’ll see a bunch of ‘createXXXXquery’ functions. These functions are factory functions for the various query types.:
createTermQuery
createTermRangeQuery
createPrefixQuery
createIntRangeQuery
createDoubleRangeQuery
createFloatRangeQuery
createBooleanQuery
createPhraseQuery
createWildcardQuery
createFuzzyQuery
createQueryParserQuery
createODataQuery
createRegexQuery
createMatchAllDocsQuery
 
Now I won’t go into each of these here – once you get the hang of a few, they’re mostly self-explanatory and the help goes a ways to explain them.
 
The first query we’re going to visit is the matchAllDocsQuery – appropriately enough this query, well, matches all docs in the database. If we get a matchAllDocsQuery from the factory and pass it to our searcher, we’ll immediately get the results.
 
![alt text](/img/bsa-11.png "Search Query")

![alt text](/img/bsa-12.png "Query Result")
 
Just what we expected – this returned all docs that we’ve added to our index.
 
A Term query lets us query on a single term, similar to what we did above with the text-based query.

![alt text](/img/bsa-13.png "Search Query")

![alt text](/img/bsa-14.png "Query Result")

 
I think you get the hang of it, and probably can figure out the other query types just by experimenting but I’ll mention just two more queries.
 
The booleanQuery lets you combine multiple queries into one – just like we did with the OR clause above, except more structured. We’ll start by creating two term queries and add them to a Boolean query as clauses.


![alt text](/img/bsa-15.png "Boolean Query")
 
And we’ll get the same results as before. It’s important to note the second argument on the add – There are three options, “Should”, “Must” and “MustNot”. These are analogous to “OR, “AND”, “AND NOT”, respectively.
 
Using  “Should” has a few interesting things around it. The Boolean Query object has a property named “minimumNumberShouldMatch” that you can specify, at a minimum, how many should clauses must match. This is useful for those instances where you’re writing a query that satisfies the query “Give me documents that match any two of apples, or bananas, or strawberries” without needing to resort to writing something like ((apples or bananas) or (apples or strawberries) or (bananas or strawberries))
 
The other query, well, class of queries I’m going to mention is the Range queries. These let me query on a range – e.g. show me all cars between, alphabetically, some text.

![alt text](/img/bsa-16.png "Search Query")

![alt text](/img/bsa-17.png "Query Result")

 
The first argument is the field name that we’re interested in, the second and third are the min and max ranges. Nulls can be specified for the min and max to make the query open-ended. Optional fourth and fifth Boolean parameters allow the query to be inclusive on the min and max values, the default is to be exclusive.
 
 
Note that we’re doing a character-based search in this example. We should be able to do the same thing with a numeric range, right? Show me all cars that cost above 550.

![alt text](/img/bsa-18.png "Search Query")
 
Result:
 
![alt text](/img/bsa-19.png "Query Result")
 
Uhoh. What happened? Turns out that numeric fields are stored twice – once as their TEXTUAL value, and another as their NUMERIC value. To access the numeric value, append “_Range” to the field name:

![alt text](/img/bsa-20.png "Search Query")

![alt text](/img/bsa-21.png "Query Result")

 
You may be wondering how the query objects relate to the textual query that we first performed. In actuality, the textual query is parsed by a QueryParser object and turned into individual query objects. With the query objects you tend to have fine grained control, and the ability to perform numeric queries that the full-text query does not let you do. If you have a single-text-box based query input mechanism, and want to provide a range of search capability, perhaps the textual query is better suited, so long as you instruct your users on the use of the syntax.
 
Another note before we move on. Field names of nested objects are referenced by using ‘.’ Notation. E.g:

![alt text](/img/bsa-22.png "Search Query")

![alt text](/img/bsa-23.png "Query Result")

 
For arrays, the field is stored multiple times, so all you need to do is supply the path of the field you’re interested in, and any matches of any values in the array will be returned:

![alt text](/img/bsa-24.png "Search Query")

![alt text](/img/bsa-25.png "Query Result")

 
Note that the StandardAnalyzer used converts all token values to lowercase, so be sure to lowercase the input values in your query. Property name casing is retained.
 
 
Field Options
---
 
Let’s start by attempting to do a query for Alfa Romeros…
 
![alt text](/img/bsa-26.png "Search Query")

![alt text](/img/bsa-27.png "Query Result")
 

Uhoh! Why didn’t I get any results??
 
The answer is that when a document is stored in the search index, the contents of the fields are tokenized, e.g. split apart and stored separately to aid in fast retrieval in what is called an inverted index. The text “Alfa Romeo” is two separate terms to the search index, and thus won’t be returned in a single term query.
 
To resolve this and to prevent the search index from analyzing and tokenizing the values of a field, we can re-index the Alfa Romeo and set some field options.

![alt text](/img/bsa-28.png "Search Query")
 
 
Index field options are set via a double ‘@@’ followed by the qualified field path suffixed by “.Index”, the value of which being one of the following options: “No, NotAnalyzed, NotAnalyzedNoNorms, AnalyzedNoNorms, Analyzed”, The field default is Analyzed. There are other field options for Storage and TermVector, which are out of scope of this document.
 
Once we have updated our index, we can now perform our query again – note that since our make field is now not analyzed, it follows the original casing:

![alt text](/img/bsa-29.png "Search Query")

![alt text](/img/bsa-30.png "Query Result")
 
Please reference “Lucene In Action” for an explanation of how the analysis process works.
 
Filtering
---
 
If we want to first filter the objects prior to querying the objects we can use a filter. Using help will also show a few factory functions for filter objects. (try saying that five times fast)
 
createPrefixFilter
createTermsFilter
createQueryWrapperFilter
 
to use a filter, create it using one of the factory methods and pass an arguments object with the query and filter as properties. We’ll be following this pattern for the rest of the examples.
 
For instance, Show me all Alfa Romeos and Jeeps that are between 450 and 600 BitCoins.

![alt text](/img/bsa-31.png "Search Query")

![alt text](/img/bsa-32.png "Query Result")

 
Skip and Take
---
 
Skip and Take work similarly to the Linq Skip and Take expressions and allow for really easy paging.

![alt text](/img/bsa-33.png "Search Query")

![alt text](/img/bsa-34.png "Query Result")
 
Sorting
---
 
Sorting is performed by the createSort() function. With the object returned, a number of sort fields can be added. For instance, we’ll sort by cost, descending.

![alt text](/img/bsa-35.png "Search Query")

![alt text](/img/bsa-36.png "Query Result")
 
The default sort is by score, use help(sort) for a full explanation of options.
 
Highlight and Explain
---
 
If you need to debug as to why a particular field did not show up as a match in your search results, or why one did, you can always use the .explain function. The first argument is the query to run, the second is the lucene doc id that is returned as part of search results.

![alt text](/img/bsa-37.png "Search Query")

![alt text](/img/bsa-38.png "Query Result")

 
Note that non-matches can also be explained.
 
 
![alt text](/img/bsa-39.png "Search Query")

![alt text](/img/bsa-40.png "Query Result")
 
Highlight works similarly, but gives a HTML fragment of the matching terms

![alt text](/img/bsa-41.png "Search Query")

![alt text](/img/bsa-42.png "Query Result")
 
 
So there you have it, a world-wind overview of the Barista Search Index bundle. There’s more to be said (Scoring and Boosting come to mind) but it’s late, and I’m wondering why all my scores are null. So I’ll continue this another time.
 
 