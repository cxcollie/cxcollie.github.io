---
layout: post
title: "TCP Long-lived Connection"
date: 2017-05-15
comments: true
tags: [Notes]
---

<div class="post-teaser"> Long live the king! 👑 </div>
<!-- more -->

<hr/>

### TCP Long-lived Connection

1. <a href="http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/overview.html">TCP long-lived connection</a> is set by setting keep-alive bit in its header<sup>[2]</sup>. The working mechanism is to attach a timer with your connection. When the keep-alive timer reaches 0, it will send a probe packet with no data on it and ACK flag on. By receiving the packet back, it knows the peer is alive.
2. We don't need to worry about sending no-data packet on application layer, where TCP is stream-based rather than packet based. So the underlying work is done automatically. The disadvantage is slightly higher network load.
3. It can help:
	* check for dead peers (if one side does not receive ACK packet from the other)
	* prevent disconnection due to network inactivity (avoid being eliminated from routing table of router by showing up frequently)
	* <a href="http://www.cnblogs.com/zuoxiaolong/category/508918.html">decrease data transmission delay</a> (avoid resetting TCP connection for each data transmission over same link)<sup>[1]</sup>.

### Working with Load Balancer

1. Here the load balancer is assigning connection at TCP layer. Thus the client is connecting 'directly' with server behind load balancer.
2. When a server fails, the client will detect that (with TCP long-lived connection) and re-connect (thru load balancer).
3. When the failed server comes back, we would like to direct some load from current servers to the new one. There are <a href="http://www.ateam-oracle.com/long-lived-tcp-connections-and-load-balancers/">3 possible solutions</a><sup>[3]</sup>: 
	* make the connection not long-lived (i.e. 10mins, but which is not short, so after 10mins it needs to reset connection thru load balancer, thus sacrificing part of the performance)
	* let the load balancer intervene to force re-connect (which is intrusive)
	* let the client acts as load balancer (to detect imbalance or node changes, which is also intrusive)
Among these 3, the first one would be the most recommended one according to the blog.

### References
1. [HTTP协议中的短轮询、长轮询、长连接和短连接 by 左潇龙](http://www.cnblogs.com/zuoxiaolong/category/508918.html)
2. [TCP Keepalive HOWTO](http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/overview.html)
3. [Long-lived TCP connections and Load Balancers by Chris Johnson](http://www.ateam-oracle.com/long-lived-tcp-connections-and-load-balancers/)