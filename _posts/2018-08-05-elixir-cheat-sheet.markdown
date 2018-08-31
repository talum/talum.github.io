---
layout: post
title: "Which Data Structure Should I Use? An Elixir Cheat Sheet"
date: 2018-08-05 10:14:24 -0400
comments: true
categories: ['elixir']
---

As an Elixir novice, I think one of the hardest things about getting started in
Elixir is figuring out what kind of data structures you want to use. My team
has recently started going all in on Elixir, so I've been trying to brush up
in earnest. Often, however, I'll be reading over some of my team's code and have a hard time deciphering what I'm
even looking at. The syntax looks very similar to Ruby (which I know pretty
well), but the patterns, conventions, and data structures are just
_slightly_ different. It reminds me a lot of when I learned Spanish for the
first time and thought to myself, _this alphabet looks familiar, but there
are all these extra letters!_. Thanks to cognates, I can get by in Spanish. Learning Elixir feels a lot like that to me.

But anyway, since I'm learning Elixir now, I thought it'd be useful to
provide a cheatsheat or overview of the differences in data structures I've noticed as a
Rubyist exploring Elixir.

## Data Types

If you're coming from Ruby (or most other programming languages), integers,
floating-point numbers, ranges, and regular expressions are all probably
familiar to you. Fortunately, those all exist in Elixir too. There are a few
differences, but I haven't dealt too much with them yet.

Atoms are like symbols in Ruby. They begin with colons and their names are
their values. For example, `:hello` is a valid atom in Elixir. They're often
used to tag values.

Elixir has the additional data types, `Port` and `PID`, which
are used in process communication. They are entities that are made available
through the Erlang VM.

A Port is used to communicate (read/write) to resources outside your
application. They are great for starting operating system processes and
communicating with them. For example, you might want to open a port to run a
OS They are great for starting operating system processes and communicating
with them.

A PID is a reference to a process. Whenever you spawn a new process, you'll
get a new PID. Expect to talk a lot about PIDs. You'll probably need to hold
onto PIDs so you can send different processes messages.

The real challenge with Elixir in my opinion, though, is figuring out how to
organize these basic data types into structures you can use. So let's take a
look at the various collection types and why you would use each.

## Collection Types
- Tuples
- Lists
- Keyword Lists
- Maps
- Structs
- HashDicts
- Binaries

## Resources
- [Basic Types](https://elixir-lang.org/getting-started/basic-types.html) on
  Elixir Lang
- Programming Elixir by Dave Thomas
- [Port Hex Docs](https://hexdocs.pm/elixir/Port.html)


