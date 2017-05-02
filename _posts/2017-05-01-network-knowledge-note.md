---
layout: post
title: "Network Knowledge Note"
date: 2017-05-01
comments: true
tags: [Notes]
---

<div class="post-teaser">  </div>
<!-- more -->

<hr/>

### Network

1. Why 255/0 is not used in IPv4 address? 255 is used for broadcast, and 0 is used by device when booted and before receiving assigned IP address.
2. In the four TCP/IP layer structure, there are link (hardware), network (i.e. IP), transport (i.e. TCP) and application (logic representation) layers.
<div style="text-align: center">
<img src ="{{site.url}}/images/2017-05/protocols.gif" />
<p class='imageNotation'>Four TCP/IP layer.</p>
</div>
3. OSI layer: Physical -> Data link -> Network -> Transport -> Session -> Presentation -> Application.
4. IP: Internet Protocol (IP) is at network layer, which provides addressing, security (encapsulation), type of service, fragmentation and re-assembly. But it does not guarantee order or integrity.
5. TCP: Transmission Control Protocol (TCP) is at transport layer. It provides end-to-end reliability, sequencing of packets and flow control.
6. CSMA: mainly used when there can be jam of traffic in network channel, especially wireless network. CSMA/CA is collision avoidance (CA) version, which tries to avoid collision; CSMA/CD is collision detection (CD) version, which terminate transmission once collision is detected, and thus improving performance of CSMA by shortening retry time.

### Security

1. HTTPS process: client request secure session -> server responds with certificate (with public key on that) -> client encrypt the session key with public key -> server decrypt the session key -> the rest of the session will use the session key. Roughly speaking, it is on session layer of OSI layer.
2. Kerberos: a MIT-developed network authentication protocol. The basic process is:
	- User logins in client interface, and client logins with user info to Authentication Service (AS)
	- AS checks user identity, replies back with Ticket to Get Ticket (TGT)
	- Client uses TGT to request service on Ticket Granting Service (TGS)
	- TGS checks TGT and replies with Service Ticket
	- Client uses Service Ticket to request service on service provider device
3. Advantages of proxy server: 
	- hide inner IP topology
	- can act as a firewall
	- add an abstraction layer (on application, serucity and so on)

### References
1. [Networking Tutorial](http://www.comptechdoc.org/independent/networking/guide/index.html)
2. [Kerberos Explained](https://msdn.microsoft.com/en-us/library/bb742516.aspx)