---
layout: post
title: "Java Notes: Enhanced For Loop"
date: 2017-06-04
comments: true
tags: [Java]
---

<div class="post-teaser">  </div>
<!-- more -->

<hr/>

I found an interesting things when using the enhanced for loop. When running the following codes, JVM throws an exception: 
```
	StringBuilder[] strBlds = new StringBuilder[10];

	for (StringBuilder strBld : strBlds) {
	    strBld = new StringBuilder();
	}

	for (int i = 0; i < 10; i++) {
	    strBlds[i].append("1"); // java.lang.NullPointerException 
	}
```
So the **strBlds** array is actually not instantiated. But if I change the enhanced for loop to a normal for loop using index, it shows no run-time error:
```
	StringBuilder[] strBlds2 = new StringBuilder[10];

	for (int i = 0; i < strBlds2.length; i++) {
	    strBlds2[i] = new StringBuilder();
	}

	for (int i = 0; i < 10; i++) {
	    strBlds2[i].append("1");
	}
```

These two versions seem to be the same in logic, but/Users/xiaocongchen/Documents/Github Personal Page/cxcollie.github.io/_posts/2017-06-04-Java-Enhanced-For-Loop.md behave differently. Why?

Let's first see what does the first enhanced for loop actually look like (when using <a href="https://stackoverflow.com/questions/9305632/java-for-each-loop-and-references">iterator</a><sup>[1]</sup>):

```
	StringBuilder[] strBlds = new StringBuilder[10];

	Iterator<MyObject> it = strBlds.iterator();
	while (it.hasNext()) {
	   StringBuilder strBld = it.next(); // a reference-copy
	   strBld = new StringBuilder();
	}
```

So the enhanced for loop gives us a reference-copy of the array element, whose reference is not pointing to any instance yet. Consider this as a "pointer" (which is not strictly true), when we modify the element (instantiate, delete), the reference-copy will point to other instance, but not update the original reference; but when we access/update the variable field in element object, we can change them (i.e. **strBld.setVar()** can change the var field in **strBld**). In a word, **don't instantiated elements with enhanced for loop**.

### Other Notes

1. Thanks for my friend's (Peng Sun) help! Can't figure this out before having his guide.
2. First week at RMN! Excited and little bit tired. Looking forward to the new week!

### References
1. [Java: For-Each loop and references on StackOverflow](https://stackoverflow.com/questions/9305632/java-for-each-loop-and-references)
