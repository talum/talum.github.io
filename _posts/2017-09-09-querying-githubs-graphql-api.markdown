---
layout: post
title: "Querying GitHub's GraphQL API"
date: 2017-09-09 21:38:53 -0400 
comments: true
categories: ["graphql", "elixir", "phoenix"]
---

As part of my "Commits" app, I decided to fetch commit data from GitHub's
new GraphQL API. I figured that while I'm learning something new, I might as
well just try learning as much as possible, which is why you're about to see
some really ugly code and a pseudo explanation of it all.

# GraphQL

GraphQL stands for graph query language. GitHub has a pretty nice [intro to
GraphQL](https://developer.github.com/v4/guides/intro-to-graphql/) if you're
interested. In summary, when you have a GraphQL API instead of a REST API,
you can fetch data from related resources in a single query from one
endpoint, [https://api.github.com/graphql](https://api.github.com/graphql),
in the case of GitHub. With a REST API, you'd likely have multiple endpoints
you'd need to hit in order to gather all the data you need. 

One thing to note that this is usually the case with a well-maintained,
public API. If you're working with an API you design that isn't totally
RESTful, you probably haven't run needed to make multiple queries in order
to obtain the data you need. In Rails-land, for instance, we often define
serializers that specify how we want an object to be represented after we
query or change it in an endpoint.

In order to form a query, you send a single post request to the endpoint.
Within the body of the request you place your query as a string. 


# GitHub

# Looping in Elixir?

# Resources
- [GitHub GraphQL Guides](https://developer.github.com/v4/guides/)
- [GitHub GraphQL Forming Calls](https://developer.github.com/v4/guides/forming-calls/#authenticating-with-graphql)
- [Learn GraphQL](http://graphql.org/learn/queries/)
