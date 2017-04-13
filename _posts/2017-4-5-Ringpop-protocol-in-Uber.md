---
layout: post
title: "Ringpop @Uber"
date: 2017-04-05
comments: true
tags: [System-Design, Engr-Blog]
---

<div class="post-teaser"> Introducing the largest 'candy jewel ring' in the world. </div>
<!-- more -->

<hr/>

* [Ringpop](#rp)
* [Actor Model](#am)
* [SWIM](#sw)

<div id="rp">
</div>

### Ringpop
Ringpop <sup>[1][2]</sup> is a scalable protocol developed at Uber, which can handle membership change, consistent hash, forward capability. The original one is written in Javascript as Uber is heavy Node.js.

<div style="text-align: center">
<img style="width: 50%" src ="{{site.url}}/images/2017-04/Ringpop-Forwarding-Request-Step-1.png" />
<p class='imageNotation'>No! This is not a candy jewel ring!</p>
</div>

1. Membership change. Ringpop use SWIM to discover new member and handle failure. Whenever a node finds a new node has joined, it updates the stored member list, and hence the consistent hashing ring.
2. Consistent hashing. Ringpop use it to map file/value to certain node, using FarmHash as its hash function, and red-black tree to represent the ring.
3. Forward capability. As a store request may need to take effect on other node, node can forward this request (to neighboring node or router of other node cluster). The forward request is sent through TChannel (in HTTP form?).
4. Actor Model is used as the concurrent model, which is discussed in the following.
5. In the video, the Flap Dampening is also mentioned, which can detect some bad-behaved node that does not react normally. A node is discarded from the cluster only when it is reported as flappy for certain times.

<div id="am">
</div>

### Actor Model
Actor model is used in Ringpop system. Let's briefly see what is it.

1. Actor model is used in concurrent system. As a substitute of multi-thread model, it gives a solution on interacting between different objects in a system.
2. "An actor is the primitive unit of computation. It’s the thing that receives a message and do some kind of computation based on it [^3]. " Different from that in OOP, different actor does not share memory with each other.
3. Each actor has a mailbox, which receives message to be processed in serial. If you want the mailbox to process in parallel, then you should have several actors. Upon receiving a message, actor can either create new actor, send message to another actor, or determine the state of next message.
4. In fault-tolerance manner, actor can process message all again from beginning.

<div id="sw">
</div>

### SWIM member control
SWIM is a membership maintenance protocol, which is similar with Gossip [^4].

1. At each time, node ni randomly pick a node nj to ping. If it does not receive ack as expected, it chooses another k random node to also ping that node. If all of them do not get ack from that node, it is (suspected to have) failed.
2. To have higher accuracy, we can add a suspicion mechanism, where a node is marked as suspected, using the SWIM protocol, and gossip this suspicion. When later that node response again, it is remarked as alive.
3. One of the advantage of SWIM is that it saves load between ni and nj. This is important as the loss of ack may due to heavy load on the link between them, where pinging more can only make this problem worse.

### References
1. [HOW RINGPOP FROM UBER ENGINEERING HELPS DISTRIBUTE YOUR APPLICATION](https://eng.uber.com/intro-to-ringpop/)
2. [Uber's Ringpop and the Fight for Flap Dampening](https://www.youtube.com/watch?v=OQyqJWQHp3g/)
3. [The actor model in 10 minutes](http://www.brianstorti.com/the-actor-model/)
4. [THE SWIM MEMBERSHIP PROTOCOL](https://prakhar.me/articles/swim/)