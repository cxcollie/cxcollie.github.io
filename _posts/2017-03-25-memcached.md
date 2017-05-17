---
layout: post
title: "Scaling Memcache @Facebook"
date: 2017-03-25
comments: true
feature: true
tags: [System-Design, Engr-Blog]
---

<div class="post-teaser"> How to improve your memory? </div>
<div class="post-teaser"> 1. Get more sleep... </div>
<!-- more -->

<hr/>

#### Reading notes of "Scaling Memcache at Facebook"

This paper talks about the implementation and optimization Facebook engineers have adopted when constructing the Memcache-based system, which is very inspiring. Here are some notes about this paper.

### Section 1. Background
1. Scenario: Users perform more reads than writes, and the storage includes MySQL database and HDFS.
2. In Memcache system, it is possible to read stale data, which indicates eventual consistency. Therefore, the probability of reading stale data is also a factor to be tuned, besides latency.

### Section 3. In a Cluster
3. The all-to-all pattern of fetching data often causes single-server bottleneck. They found that data replication can alleviate the load of servers under this problem, but may lead to memory inefficiency. Thus came up with the idea of optimizing memcache client.
4. The functions of memcache client include request batching, error handling and so on, and it maintains a map of all available servers (for finding the address of certain data to be fetched).
5. A directed acyclic graph (DAG) is constructed for representing dependency between data to fetch data in parallel.
6. **mcrouter** is used to coalesce connection between web thread with memcached server, which uses UDP connection instead of TCP to reduce latency and server load.
7. Payoff: mcrouter controls window size similarly with window in TCP connection. If the window size is small, application needs to send requests in more windows; if is large, there may be incast congestion, which often happens in all-to-one scenario.

#### Section 3.2 Load Reduction using **Lease**
8. Lease is, when a client experiences a cache miss in memcache, the server gives a lease token, so that the client can set data when come back.
9. Lease can mitigate problem of reading stale data, as the token can be invalidated by server; and thundering herd, as only one token will be given in a certain period, i.e. 10s, then other request without the token will be asked to come back after some time, then the data is often present there.

#### Section 3.3 Handling Failure
10. A small amount of servers are acting as stand-by, which is called Gutter. This proportion is around 1%.
11. The cost of Gutter is reading slightly stale data.

### Section 4. Replication
12. Replication of data can enable more independent failure domains, tractable network configuration, and a reduction of incast congestion.
13. In front of mcrouter there is another layer with mcsqueal, which can batch deletes into fewer packets, then send them to specific mcrouters.
14. Regional pool: using several frontend clusters to share a same set of memcache servers to reduce replica of data.
15. Cold cluster warmup: let clients to communicate with "warm clusters", which have normal hit rate, rather than persistent storage, to mitigate load on storage.

### Section 5. Consistency
16. The consistency model across regions is master-slave.
17. When writting in a non-master region, a remote marker is used to detect whether certain data is stale. It leads to increased latency, but decrease probability of reading stale data.

### Section 6. Single Server Improvement
18. LRU is used to determine which data to be evicted. And slab class can determine the memory size needed for certain data. When a slab is about to be evicted, but its usage frequency is 20% more than the average, then its memory size will be expanded.
19. To evict short-lived keys lazily, they are placed in a circular buffer of linked-list, which is moved every certain unit of time (a common approach). Each time items in the end slot will be evicted.