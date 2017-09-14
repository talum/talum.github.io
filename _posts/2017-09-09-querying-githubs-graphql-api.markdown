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

I played a lot with the [GitHub GraphQL Explorer](https://developer.github.com/v4/explorer/) in order to get a sense of what my query would actually look like. One nice thing you'll notice about GraphQL is that the structure of your query will basically mirror the structure of the JSON payload you get back. Among the other many benefits of GraphQL is the ability to ask for exactly the data you need, nothing more, nothing less.


# GitHub
In order to get started with the GitHub API, you'll need to do a couple
setup steps, such as [creating a personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) in order to authenticate. You'll also need to grant that access token certain scopes.

When you're ready, it's time to query, and by query, I mean mess around with
the explorer until you get something that resemble the data you want.

I decided to start from a repository. After consulting some of the
[schema](https://developer.github.com/v4/reference/object/repository/)
documentations and more experimentation, I was able to form a query that
would go from the repository (a node), grab a ref with the name of `master`,
then leverage a `target`, in this case a Git object, and eventually use an
inline fragment to extract some data about commits. From there, I started
traversing the graph using edges and nodes, where edges are the connections
between nodes and nodes are basically different resources, to grab all of
the commits by this one individual. Full disclosure: I have not read all of
the docs about queries and GraphQL, but I did land on this query, which does
the job fairly well.

For reference, here's the function I created that will construct the query I
want while passing in some variables.

```elixir
  def query(repoName, owner, cursor) do
    "query {
      repository(name: \"#{repoName}\", owner:\"#{owner}\") {
        name
        ref(qualifiedName: \"master\") {
          target {
            ... on Commit {
              id
              history(first: 100, author: {id: \"MDQ6VXNlcjEyNDA1MDQ=\"}, after: \"#{cursor}\") {
                pageInfo {
                  hasNextPage
                }
                edges {
                  node {
                    oid
                    message
                    author() {
                      name
                      date
                    }
                  }
                  cursor
                }
              }
            }
          }
        }
      }
    }"
  end

```

Now, because I wanted to query a number of repositories and run script to
seed my database easily, I decided to write a module in the seeds file,
`/commits/priv/repo/seeds.exs`.

The content of the module follows below:

```elixir
defmodule Seed do

  def query(repoName, owner, cursor) do
    "query {
      repository(name: \"#{repoName}\", owner:\"#{owner}\") {
        name
        ref(qualifiedName: \"master\") {
          target {
            ... on Commit {
              id
              history(first: 100, author: {id: \"MDQ6VXNlcjEyNDA1MDQ=\"}, after: \"#{cursor}\") {
                pageInfo {
                  hasNextPage
                }
                edges {
                  node {
                    oid
                    message
                    author() {
                      name
                      date
                    }
                  }
                  cursor
                }
              }
            }
          }
        }
      }
    }"
  end

  def get_commits(repoName, owner, cursor, hasNextPage) when hasNextPage == true do
    token = Application.get_env(:commits, :github_access_token)

    {:ok, %HTTPoison.Response{status_code: 200, body: body}} = HTTPoison.post("https://api.github.com/graphql", Poison.encode!(%{"query" => query(repoName, owner, cursor)}), [{"Authorization", "bearer #{token}"}, {"Content-Type", "application/json"}])

    history = body
      |> Poison.decode!
      |> Kernel.get_in(["data", "repository", "ref", "target", "history"])

    nextPage = history["pageInfo"]["hasNextPage"]

    commits = body
      |> Poison.decode!
      |> Kernel.get_in(["data", "repository", "ref", "target", "history", "edges"])

    Enum.map(commits, fn c -> save(c, repoName) end)

    last_cursor = List.last(commits)["cursor"]
    get_commits(repoName, owner, last_cursor, nextPage)
  end

  def get_commits(repoName, owner, cursor, hasNextPage) when hasNextPage == false do
    IO.puts cursor
  end

  def save(commit, repoName) do
    message = commit["node"]["message"]
    date    = commit["node"]["author"]["date"]

    if !String.contains?(message, "Merge") do
      IO.puts message
      {:ok, formatted_date} = NaiveDateTime.from_iso8601(date)
      Commits.Repo.insert!(%Commits.Logs.CommitMessage{content: message, committed_at: formatted_date, repo_name: repoName})
    end
  end
end

```


# Looping in Elixir?



# Running the Seed File
Running the seed file is easy from the command line with:

```elixir
  mix run priv/repo/seeds.exs
```

And right after that, I had a pretty full database ready to be queried in a
non-GraphQL fashion.

All of this only touches the surface of GraphQL and somewhat
poorly demonstrates my current knowledge of Elixir. But hey, we all need to
start somewhere.

# Resources
- [GitHub GraphQL Guides](https://developer.github.com/v4/guides/)
- [GitHub GraphQL Forming Calls](https://developer.github.com/v4/guides/forming-calls/#authenticating-with-graphql)
- [Learn GraphQL](http://graphql.org/learn/queries/)
