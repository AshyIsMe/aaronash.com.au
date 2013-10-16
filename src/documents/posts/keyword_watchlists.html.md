```
title: Keyword Watchlists
layout: post
tags: ['watchlists', 'keyword watchlists', 'post']
```




Recently I've added keyword based watchlists to my side project [watchlists](www.watchlists.com.au).  The idea with these is to be able to keep an eye on the market for certain types of news items based on whichever keywords you happen to be interested in.


### Simple keyword based matching
 
In order to keep it simple for now I've kept the matching very basic.  It will simply look for a match of every single keyword specified against every official news item that is released to the market.  So this means that you will only see the items with every keyword in the headline. It also means you'll miss out on the ones that are missing even just one of the keywords. So choose your keywords carefully.

Later on I plan to extend this keyword based searching to use a proper engine like [elasticsearch](www.elasticsearch.org) to allow for fuzzier searches and also full text scanning of the news documents themselves.

### Screenshot

<a href="/images/keywordwatchlists.png">
<img src="/images/keywordwatchlists.png" class="img-responsive">
</a>

### Next up, email alerts
But for now my next focus is to add email alerts to [watchlists](www.watchlists.com.au) to allow for the kinds of real time news alerts that can really start to come in handy for certain trading styles.
