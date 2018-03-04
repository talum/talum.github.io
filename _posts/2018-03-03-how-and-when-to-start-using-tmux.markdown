---
layout: post
title: "How and When to Start Using tmux"
date: 2018-03-03 22:22:10 -0500 
comments: true
categories: ["tmux"]
---

Over the past week, I've been trying to start using tmux in earnest.

On a typical day, I don't need to start a process in more than two or three
tabs. But this past week, I reached a breaking point and possibly a new
record when I had to get practically the entire system working locally!

This included the following:
- the main Rails monolith
- webpack for all the app's JavaScript
- a message broker app written in Sinatra
- A sneakers process to manage workers
- A Dockerized version of some other assorted dependencies, including
  elasticsearch, postgres, redis, and rabbitMQ
- a Phoenix app for our IDE backend
- a Rails console

Navigating between all these windows became rather onerous, especially since
I am just awful at tab management. And then there's the trouble that I've
been using vim as my text editor lately, so that takes yet another window,
or two, since my work was spanning multiple codebases.

Tmux solves most of that problem while also giving you the opportunity to
attach and detach from sessions. There are even plugins that allow you to
persist your layout between sessions or system reboots, which I hope to look
more into soon.

My secondary reason was to remote pair with tmate, which is way more
performant than trying to screenshare and give remote control to someone
using software like Zoom.us or Screenhero.


## tmux setup
vi bindings in conf

## tmux essential commands

## tmate

## more to explore with plugins and such, but that's enough tmux to get
started.

### Resources
- [A Gentle Introduction to tmux](https://hackernoon.com/a-gentle-introduction-to-tmux-8d784c404340)
