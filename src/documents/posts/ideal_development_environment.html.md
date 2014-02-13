```
title: The ideal programming environment
layout: post
tags: ['development', 'haskell', 'erlang', 'post']
```

# What is the ideal programming stack?
Nothing will be best across all domains of course but within:
## Web development
## Scalability
### The vast majority of projects don't need to scale past a few hundred users but it's nice to know that to scale up to hundreds of thousands is as easy as spinning up more nodes and having them auto connect to the cluster

# What are the right questions to ask to find out?
## Language to Database integration
Is it helpful to have the database native query language match the app dev language?
Mongo is nice and easy to work with in javascript because of JSON, is Riak that nice to work with in erlang?
Having messed around with haskell a little bit I'm really liking how nice static typing can be especially with good type inference.

# Comparisons
## Node.js MongoDB
## Haskell MongoDB
## Haskell Riak
## Haskell SQL - mySQL, postgresql
## Erlang Riak


# Editor integration
More preference so long as certain features are available:
## Jump to definition
## Compile error integration (including lint warnings)

