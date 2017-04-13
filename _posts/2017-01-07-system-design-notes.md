---
layout: post
title: "System Design Note 2: Design Process and Salted Password Hash"
date: 2017-01-07
comments: true
tags: [System-Design]
---

<div class="post-teaser"> How to get a Salted Password: </div>
<div class="post-teaser"> 1. Place <strike>chicken</strike> password in slow cooker. Season with salt and pepper.</div>
<div class="post-teaser"> 2. Cook on high, stirring occasionally, for 4 hours...</div>
<!-- more -->

<hr/>

* [Process in system design](#pro)
* [Salted Password Hash](#sph)

<div id="pro">
</div>

### Process in system design
1. Features(2 mins). Specify features of this system.
2. Estimation(5 mins). Number of user, traffic(read and write), QPS and outlier behavior.
3. Design goals(2 mins). Latency, consistency and availability.
4. Skeleton of design(5 mins). API and work flow of read and write request.
5. Deep dive(20-30 mins). How to deal with fault tolerance, read and write efficiently in database layer, load balancer, sharding,
6. Factors to be considered: availability, performance(latency), reliability, scalability, manageability and cost.

<div id="sph">
</div>

### Salted Password Hash
1. The account database is usually attacked, so password is not stored in the database. Rather, some indirect information, like the salted password hash is stored.
2. Upon receiving a password, its hashcode is checked against the database. To improve its security, a salt, which is a random string, is added when hashed. So in database we will store the salt and final hashcode.
3. To crack such system, we can use lookup table or rainbow table(where more hashes are stored in same table).

### Other notes
1. RDBMS works better for joining relations, while NoSQL has more flexible scheme and efficient column analysis.
2. HBase do consistent hashing inherently, and writes are cheaper.
3. Concerns with distributed data/functionality: data locality and consistency

### References
1. [CS75 Lec9 - Scalability by David Malan](https://www.youtube.com/watch?v=-W9F__D3oY4&index=1&list=WL)
2. [Salted Password Hashing - Doing it Right](https://crackstation.net/hashing-security.htm)
3. [System design on InterviewBit](https://www.interviewbit.com/courses/system-design/topics/interview-questions/)
4. [Scalable Web Architecture and Distributed Systems by Kate Matsudaira](http://www.aosabook.org/en/distsys.html)