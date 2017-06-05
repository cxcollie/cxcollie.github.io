---
layout: post
title: "Experiences from Refactoring UI-Drive with Spring"
date: 2017-05-20
comments: true
tags: [Thoughts]
---

<div class="post-teaser">  </div>
<!-- more -->

<hr/>

### Tomcat cacheMaxSize

1. When using Tomcat 8 with project containing many JARs (like Spring), we may see the warning logs:
```
	org.apache.catalina.webresources.Cache.getResource Unable to add the resource at [***] to the cache because there was insufficient free space available after evicting expired cache entries - consider increasing the maximum size of the cache
```
This is commonly seen in Tomcat 8.
2. <a href="https://github.com/jhipster/generator-jhipster/issues/3995">Reason</a><sup>[1]</sup> is:
	* "Tomcat caches classes, JAR files, HTML, JSPs and any other files that contribute to the web application"
	* "The total cache size (cacheMaxSize) is 10240kb and de maximum object size (cacheObjectMaxSize) is 512kb" by default
3. <a href="http://stackoverflow.com/questions/26893297/tomcat-8-throwing-org-apache-catalina-webresources-cache-getresource-unable-to">Solution</a><sup>[2]</sup> is (if you are using **Eclipse**):
	* in the Eclipse workspace, you should be able to find a **Servers** folder; find the Tomcat 8's folder
	* inside it, find the **context.xml** file for Tomcat 8
	* add this before _</context>_ tag
	```
		<Resources cachingAllowed="true" cacheMaxSize="100000" />
	```
	* restart Eclipse, hopefully you should find the problem solved

### Other Notes

1. To access form data (without binding with modelAttribute), @RequestParam("userName") String name

To be continued...

### References
1. [Issue Discussion on Github](https://github.com/jhipster/generator-jhipster/issues/3995)
2. [StackOverflow Discussion](http://stackoverflow.com/questions/26893297/tomcat-8-throwing-org-apache-catalina-webresources-cache-getresource-unable-to)
3. [Understanding Spring MVC Model and Session Attributes](https://www.intertech.com/Blog/understanding-spring-mvc-model-and-session-attributes/)
4. [How do servlets work? Instantiation, sessions, shared variables and multithreading on Stack Overflow](https://stackoverflow.com/questions/3106452/how-do-servlets-work-instantiation-sessions-shared-variables-and-multithreadi/3106909#3106909)
5. [Spring MVC pass same object between controller](https://stackoverflow.com/questions/21426519/spring-mvc-pass-same-object-between-controller)
6. [GUI Architectures](https://martinfowler.com/eaaDev/uiArchs.html)