---
layout: post
title: "Markdown syntax notes"
date: 2017-01-06
---

# h1 Heading
## h2 Heading
### h3 Heading
#### h4 Heading
##### h5 Heading
###### h6 Heading

<div id="anchor">
</div>

<!--
 This is a comment
 -->

Table (but where is the line...)

| Option | Description |
| ------ | ----------- |
| r1 | row 1. |
| r2 | row 2. |
| r3 | row 3. |

___

---

***

**bold text**

_italicized text_

~~Strike through~~

> Quote 1
>> Embedded quote 2.

* valid bullet 1.1
* valid bullet 1.2
- valid bullet 2.1
- valid bullet 2.2
+ valid bullet 3.1
+ valid bullet 3.2

![Minion]({{site.url}}/images/mountietocat.png)

* [name anchor](#anchor)


Hyper link [Jekyll](http://jekyllrb.com)

{% highlight java linenos %}
package com.company;
import java.util.*;
import java.text.*;

public class Main {
public static void main(String[] args) {
int a = 1;
System.out.println("123" + a);
}

}
{% endhighlight %}

```java
package com.company;
import java.util.*;
import java.text.*;

public class Main {
public static void main(String[] args) {
int a = 1;
System.out.println("123" + a);
}

}
```

``` markup
Sample text here...
```

footnote [^1].

[^1]: To say down here.

This PR looks great - it's ready to merge! :shipit:
