---
layout: post
title: "Reactive Programming"
date: 2017-05-09
comments: true
tags: [Notes]
---

<div class="post-teaser">  </div>
<!-- more -->

<hr/>

Similar with asynchronus programming<sup>[3]</sup>, reactive programming (RP) tries to solve the problem of intensive request. The 4 major characteristics of RP<sup>[1]</sup> are: responsive, resilient, scalable, and message-driven.

### Responsive

Basically, responsiveness requires the application to response quickly, no matter in normal case or in face of node failures. This is also the reason for using reactive programming.

### Resilient

Resilient requires application to tolerate some failures. In details, it can be resilience on external dependencies, that is to operate normally and securely even when the external dependency becomes wild; resilient performance, no huge increase in response delay under thundering herd effect; and resilient storage, like persistent storage in face of failures.

### Scalable

A common problem for nowadays web application. Often times you want your application to be able to scale out, but not burning your brain cell for crazily complicated solution. Reactive programming makes it easier to scale out, fortunately.

### Message-driven

Most of the applications transferred to reactive programming may be using observer pattern before, for which the message-driven model perfectly fits. In message-driven model, there can be combination of event-driven and actor-based.
<br><br>
Event-driven can be used to handle the sequential order of operations. An imperative style will implement it with callbacks, which makes caller wait for the come-back of the calling function. Another potential problem of event-driven is callback-hell<sup>[4]</sup>, where the recipients of messages are anonymous callbacks instead of addressable recipients<sup>[1]</sup>. So the [actor model]({{site.url}}/blog/2017/04/05/Ringpop-protocol-in-Uber#am) is necessary.
<br><br>
For Reactive programming, there are some tools to use, like Rx (Reactive extension, and [here](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754) is an excellent tutorial for RxJS) and Akka (a toolkit for building applications using the Actor pattern in Scala or Java).

<div style="text-align: center">
<img style="width: 30%" src ="{{site.url}}/images/2017-05/ReactiveX.png" />
<p class='imageNotation'>Logo of ReactiveX</p>
</div>

### References
1. [What is Reactive Programming? by Kevin Webber](https://blog.redelastic.com/what-is-reactive-programming-bc9fa7f4a7fc)
2. [The introduction to Reactive Programming you've been missing by andrestaltz](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)
3. [Notes on Reactive Programming Part I: The Reactive Landscape by Dave Syer](https://spring.io/blog/2016/06/07/notes-on-reactive-programming-part-i-the-reactive-landscape)
4. [Callback Hell](http://callbackhell.com/)