---
layout: post
title: "Shuffle in Map-Reduce @Google"
date: 2017-04-04
comments: true
tags: [System-Design, Engr-Blog]
---

<div class="post-teaser"> How to shuffle cards🃏 like a pro? </div>
<!-- more -->

<hr/>

* [Shuffle](#shuf)

<div id="shuf">
</div>

### Shuffle

1. Different from the conventional 'shuffle' function, shuffle is an important stage in Map-Reduce. It works between Map and Reduce stage. Basically, after the master split the job, there are <a href="http://hokein.me/2013/10/17/map-reduce-shuffle/">3 main steps</a> on Map worker: partition, spill & sort and merge <sup>[1]</sup>. While on Reduce worker, there are: fetch, sort(merge), reduce.
2. In Map, it partition incoming data based on reducer (in memory); then when data size approaches threshold, it will spill them (into disk), and manipulate them; then merge them into a sorted data structure file.
3. In Reduce, it fetches data from Map worker thru HTTP GET; sorts partly-sorted data from different Map into a larger sorted one; then reduces them.
4. So the Shuffle stage happens between merge step of Map and fetch step of Reduce. To improve the performance, we can have a <a href="http://langyu.iteye.com/blog/992916">combine stage</a> after merge, i.e. in word count application, an output of ["aaa", {1, 1, 1}] can be combined into ["aaa", {3}] <sup>[2]</sup>, so to save storage space and decrease network transmission load.
5. Also, on Reduce worker, we can set whether to store input data (from Map) into memory or disk, and the threshold of it. When storing data into disk, there can be 2 types: hash-based and sort-based, where the former may produce more tiny partitions and the latter may <a href="http://www.jianshu.com/p/0ddf3ae19b49">consume more time</a> <sup>[3]</sup>.

### References

1. [Hadoop MapReduce Shuffle Procedure by Hokein](http://hokein.me/2013/10/17/map-reduce-shuffle/)
2. [MapReduce: detailed explanation of Shuffle procedure](http://langyu.iteye.com/blog/992916)
3. [Comparion of Shuffle between Spark and Hadoop](http://www.jianshu.com/p/0ddf3ae19b49)