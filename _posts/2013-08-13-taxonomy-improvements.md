---
layout: post
title:  "Taxonomy Improvements"
date:   2013-08-13
author: "Sean McLellan"
tags: ["SharePoint Taxonomy Bundle"]
---

I’ve just checked in a bunch of improvements to the SharePoint Taxonomy related bundle in Barista.
 
1. Taxonomy is now a bundle unto itself – you can still get a taxonomy instance from spSite.getTaxonomySession(), however, you can also do the following:

  a. ![alt text](/img/2013-08-13-taxonomy-improvements-01.png "Code Snippet")

  b. You can supply a SPSite instance, a Guid instance or a url as the argument.

2. The TaxonomySession has a “static” function to sync the taxonomy hidden list on a site. (Technically a function on the prototype)

  a. ![alt text](/img/2013-08-13-taxonomy-improvements-02.png "Code Snippet")

  b. This is similar to the ResyncHiddenList on the TermStore prototype, but according to the SP docs – “The method enumerates through every item in the hidden list to ensure it is up to date.”

3. TermSets are now surfaced

  a. ![alt text](/img/2013-08-13-taxonomy-improvements-03.png "Code Snippet")

  b. Note that the functions on the taxonomy session object now return collection objects. If you need to get all the term sets as an array, you can use the function as above, or, use one of the getters.
    
  c. ![alt text](/img/2013-08-13-taxonomy-improvements-04.png "Code Snippet")

4. I’ve also added additional overloads to retrieve termsets, and termstores, that it looks like I missed on the first pass.

  a. getTermsWithCustomProperty, offlineTermStoreNames, termStores
 
Hope this helps, back to the Content Deployment Bundle..