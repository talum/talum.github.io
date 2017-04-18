---
layout: post
title: "Promises in JavaScript: A Gentle Introduction"
date: 2017-04-17 20:21:52 -0400 
comments: true
categories: ["JavaScript", "promises"]
---

Lately, I've been reading about Promises in JavaScript (major
shoutout to Kyle Simpson and his [You Don't Know JS series](https://github.com/getify/You-Dont-Know-JS)), so I
figured it was time to write a blog post about what I've learned, especially since Promises
are now native in ES6. 

That's great, you're probably thinking, but why do I need to know this? The
answer is that Promises simplify the handling of asynchronous behavior in
JavaScript and have the potential to eradicate callback hell as we know it.
Never encountered callback hell? You probably have, but just didn't realize
it had the potential to be as hellish as it is. Any time you have a series
of asynchronous actions and need deterministic values to be returned, you're probably doing
some crazy gating or latching technique in order to make sure that the data
you want is present.

For instance, let's say you want to do something like this: 

```javascript

let a = ajax('http://some.url')
let b = ajax('http://some.other.url')

function addResponse() {
  return a + b
}

addResponse()

```

You make two ajax calls and want to return the value of both the calls. The
problem is that you don't know exactly when both `a` and `b` will be
available. In order to work around this problem, you could write something
like this using callbacks (and a gate):

```javascript

let a = ajax('http://some.url', sum)
let b = ajax('http://some.other.url', sum)

function sum() {
  if (a && b) { //cool gate #amirite
    return a + b
  }
}

```

This way, we wait until both values resolve before summing them. This
approach, though, could be improved and clarified with Promises. As Kyle
Simpson points out in chapter 2 of his book, [Async and Performance](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch2.md), "It turns out that how we express asynchrony (with callbacks) in our code doesn't map very well at all to that synchronous brain planning behavior." In other words, using callbacks isn't inherently intuitive to our sequential, planning brains. Thus, we turn to Promises.

# What Are Promises?

At their core, Promises are just a way of expressing a future value. Simpson
likens a Promise to a receipt or IOU at a fast food restaurant. You request
something and you get a promise that you'll eventually receive something
back. (Going forward, I'm going to stop capitalizing the word "promises." I
think you get it.)

# Promise Basics

According to the [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), the syntax for a promise is as follows:

```javascript

new Promise ( /* executor */ function(resolve, reject)  { ... })

```

The executor is just a function that is passed the arguments `resolve` and
`reject`. Usually the executor does some asynchronous stuff. When it's done,
it'll call the `resolve` function with a final value. If an error gets
thrown, `reject` will be called instead. Either way, with Promises, you're
guaranteed to get some sort of value, which eliminates the "inversion of
control" problem of callbacks. (When you use callbacks, you're effectively
passing execution of your program, so functions could get called once as
intended, too many times, or never at all.)

Promises have three states: pending, fulfilled, and rejected. Pending means
that it's neither fulfilled or rejected, fulfilled means that the operation
was successful, and rejected means that the operation failed.

# How to Use Promises

If you were writing you're own async operation, you'd probably write
something like this: 

```javascript

function fetchSomeData() {
  return new Promise(function(resolve, reject) {

    // an operation to async
    let request = new XMLHttpRequest()
    request.open('GET', '/someUrl')

    request.onload = function () {
      resolve(this.responseText) // when the loading is done, it'll resolve
      with the value
    }

    request.send()
  })
}

```

Now, you can call

```javascript

  fetchSomeData()
    .then((data) => {
      console.log(data)
    })

```

and the data should print out to your console (when it's good and ready).
You can imagine that this is how a lot of libraries such as `fetch` or
`axios` likely end up implementing asynchronous calls with promises.

# How You'll Use Promises Most of the Time

# Cool Promise Tricks

# Resources
- [MDN: Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [You Don't Know JS: Async and Performance](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance)
