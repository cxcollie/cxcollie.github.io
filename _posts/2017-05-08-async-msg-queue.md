---
layout: post
title: "Asynchronous Message Queue"
date: 2017-05-08
comments: true
tags: [Notes]
---

<div class="post-teaser">  </div>
<!-- more -->

<hr/>

### Asynchronous Programming

Many websites or applications are experiencing crazy number of users nowadays. The imperative programming model can hardly satisfy that need. There comes the reactive programming, and the components inside that.
<br><br>
One analogy is when you are doing a laundry. In traditional way, you put your dirty clothes in the washer (calling **washer()**); stand there waiting for around half an hour (waiting for the return of **washer()**); then throw the wet clothes into dryer (calling **dryer()**); another half an hour of waiting; then next basket of clothes. This is an old-school single-threaded model. Boring, right?
<br><br>
The other problem is you need to busily wait until all the work are done. This is like letting your front-end halting there for seconds (with users having no idea what is going on behind the screen). So you decide to use multithreaded-model, and buy some automated-washer/dryer-robots.

<div style="text-align: center">
<img style="width: 30%" src ="{{site.url}}/images/2017-05/laundry.jpg" />
<p class='imageNotation'>I-Love-Laundry<sup>[6]</sup></p>
</div>

With the multithreaded-model, you can put different jobs into different threads. You have the washer in one thread, and dryer in the other thread. This is a perfect example for **producer-consumer** pattern, where the washer acts as a producer, and dryer as a consumer. You decide that the washer puts the wet clothes in a basket, for the dryer to pick-up and dry it. 
<br><br>
Good news is you don't need to wait there until work is done. You are sure that your intelligent dryer will pick-up the clothes automatically. Now you can leave the clothes in washer and hopefully they will be washed and dried later in the future.
<br><br>
Then the problem is: how to build the buffer? And how is the work past through the buffer (whether using push/pull pattern for notification)? And what if, the buffer fails?

<div style="text-align: center">
<img style="width: 30%" src ="{{site.url}}/images/2017-05/mailbox.jpg" />
<p class='imageNotation'>Oh! Another message!<sup>[7]</sup></p>
</div>

So these are the major concern for a message queue: quick append, notification and persistent storage. The database can be modified as a MQ for moderate usage, but not fit for high demand. Good news is there are several MQs designed for use; and in fact some RDBMS like PostgreSQL also offers native support for asynchronous notification<sup>[3]</sup>. 
<br><br>
One thing to point out is using MQ in the system may add some complexity to it. So it should be used only when necessary<sup>[1]</sup>. Now, let's see some of the coolest MQs.

### ZeroMQ

1. Different from the other MQ structure (Producer-Broker-Consumer), ZeroMQ does not have a broker. It offers users the freedom (and also responsibility) to implement more details of it. Thus it has the best performance among others<sup>[5]</sup>.
2. There are 2 processes in ZeroMQ model<sup>[4]</sup>: prompt and display. Prompt receives the input (producer), may pass it to E (Exchange, a local queue), then sends to the global/central Q (Queue, a global queue that receives input from all local Es). Display receives message from global/central E (a global queue that distributes message to local Qs) through local Q, then excutes them.

### RabbitMQ and ActiveMQ

1. Different from ZeroMQ, these two are easier to use (while sacrificing some raw speed). Their difference is very tiny, i.e. ActiveMQ is built in Java on the JMS (Java Message Service) and is very frequently used within applications on the JVM (Java, Scala, Clojure, et al), while RabbitMQ is more widely used in Ruby or Python web application<sup>[2]</sup>.

### Kafka

1. Kafka use model similar with producer-consumer<sup>[8]</sup>, and messags are sorted based on **topic**. And each topic partition is an ordered log of immutable messages, which is append-only.
2. Consumers can be in consumer group, where the individual consumer can subscribe to different partition. A consumer group serves for a generic function. This allows the cluster to scale more easily.
3. In short, Kafka allows messages to be produced in order, and consumed in order (due to the ordered log). Topics are distributed and replicated.

<div style="text-align: center">
<img style="width: 50%" src ="{{site.url}}/images/2017-05/Kafka.png" />
<p class='imageNotation'>Consumer group in Kafka<sup>[8]</sup></p>
</div>

### References
1. [Asynchronous Processing in Web Applications, Part 1: A Database Is Not a Queue](http://blog.codepath.com/2012/11/15/asynchronous-processing-in-web-applications-part-1-a-database-is-not-a-queue/)
2. [Asynchronous Processing in Web Applications, Part 2: Developers Need to Understand Message Queues](http://blog.codepath.com/2013/01/06/asynchronous-processing-in-web-applications-part-2-developers-need-to-understand-message-queues/)
3. [Asynchronous Notification](https://www.postgresql.org/docs/9.2/static/libpq-notify.html)
4. [ZeroMQ-Instant Messaging (Tutorial)](http://zeromq.org/code:examples-chat)
5. [Message Queue Shootout!](http://mikehadlow.blogspot.com/2011/04/message-queue-shootout.html)
6. [Laundry Robot Picture](http://www.willowgarage.com/blog/2010/06/09/pr2-beta-sites-spotlight-uc-berkeley)
7. [Mailbox Picture](http://ole-wielebinski.de/Ole1/index.php/oles-familienrezepte/2009?view=archive&month=2)
8. [I Can't Believe It's Not a Queue: Using Kafka with Spring by Joe Kutner](https://www.infoq.com/presentations/kafka-spring?utm_campaign=rightbar_v2&utm_source=infoq&utm_medium=presentations_link&utm_content=link_text)