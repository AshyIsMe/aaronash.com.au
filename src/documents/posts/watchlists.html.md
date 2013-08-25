```
title: Watchlists
layout: post
tags: ['watchlists','post']
```

##[www.watchlists.com.au](http://www.watchlists.com.au)


   * News based share trading style
   * alerts
   * keyword news analysis

Recently I've been working on a project called Watchlists (www.watchlists.com.au) .  It's a sharemarket news watching site for the Australian Stock Exchange (ASX).  

There are plenty of similar sites for technical analysis based trading but not really many that focus directly on news based trading.  Next up I'll be adding keyword based watchlists and email alerts.

The general idea being that you would set up some news keywords you are interested in (such as "exceptional drill results") and then you see which companies have had news matching those keywords and optionally get email alerts as more news items hit the market.

##Why am I doing it?


   * Fun
   * Profit
   * Learning Nodejs and playing on AWS

I've also been using this as an opportunity to learn Node.js.  So far it's been fun and I'm enjoying the minimalistic approach of express and mongodb (with mongoose).

The only negative thing I've noticed about it so far is that it's not really great for doing large data loads from csv into the database.  My  first naive script to do that for the past 5 years of ASX news data was hitting Out of Memory limits and being killed by the OS.  Though this is because I was firing off the mongoose insert events asynchronously and building up a massive amount of callbacks on the stack.  Rewriting this to use asyncjs and fire off the db inserts synchronously fixed that up.

Eventually this will hopefully be my main tool for trading Aussie shares.  I'm going to add paid accounts using Amazon Payments though I'm not expecting too many paid users to sign up.  If I could get the paid accounts to cover the AWS costs of hosting I'd be pretty happy.


##Show me the screenshots!

<img src="/images/screenshot1.png" class="img-responsive">

So far the interface is pretty basic but it is functional for the basic use case.  I have a webscraping script that grabs the latest news from the ASX site and updates each user's watchlists as needed.


