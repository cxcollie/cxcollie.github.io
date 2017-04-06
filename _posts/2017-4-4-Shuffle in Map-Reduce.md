---
layout: post
title: "Shuffle in Map-Reduce"
date: 2017-04-04
---

* [Shuffle](#shuf)

<div id="shuf">
</div>

### Shuffle

1. Different from the conventional 'shuffle' function, shuffle is an important stage in Map-Reduce. It works between Map and Reduce stage. Basically after the master split the job, there are 3 main steps on Map worker: partition, spill & sort and merge [^1]. While on Reduce worker, there are: fetch, sort(merge), reduce.
2. In Map, it partition incoming data based on reducer (in memory); then when data size approach threshold, it will spill them (into disk), and manipulate them; then merge them into a sorted data structure file.
3. In Reduce, it fetch data from Map worker thru HTTP GET; sort partly-sorted data from different Map into a larger sorted one; then reduce them.
4. So the Shuffle stage happens between merge stage of Map and fetch stage of Reduce. To improve the performance, we can have a combine stage after merge, i.e. in word count application, an output of ["aaa", {1, 1, 1}] can be combined into ["aaa", {3}] [^2], so to save storage space and decrease network transmission load.
5. Also, on Reduce worker, we can set whether to store input data (from Map) into memory or disk, and the threshold of it. When storing data into disk, there are hash-based and sort-based, where the former may produce more small partitions and the latter may need more time [3].

1. [Hadoop MapReduce Shuffle Procedure by Hokein](http://hokein.me/2013/10/17/map-reduce-shuffle/)
2. [MapReduce: detailed explanation of Shuffle procedure](http://langyu.iteye.com/blog/992916)
3. [Comparion of Shuffle between Spark and Hadoop](http://www.jianshu.com/p/0ddf3ae19b49)