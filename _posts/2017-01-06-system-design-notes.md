---
layout: post
title: "System design: scalability, fault tolerance and consistent hashing"
date: 2017-01-06
---

# h1 Heading
## h2 Heading
### h3 Heading
#### h4 Heading
##### h5 Heading
###### h6 Heading

<!--
 This is a comment
 -->

table (but where is the line...)

| Option | Description |
| ------ | ----------- |
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |

___

---

***

**rendered as bold text**

_rendered as italicized text_

~~Strike through this text.~~

> Donec massa lacus, ultricies a ullamcorper in, fermentum sed augue.
Nunc augue augue, aliquam non hendrerit ac, commodo vel nisi.
>> Sed adipiscing elit vitae augue consectetur a gravida nunc vehicula. Donec auctor
odio non est accumsan facilisis. Aliquam id turpis in dolor tincidunt mollis ac eu diam.

* valid bullet
- valid bullet
+ valid bullet

![Minion]({{ site.url }}/images/mountietocat.png)

# Table of Contents
* [Chapter 1](#h1 Heading)
* [Chapter 2](#chapter-2)
* [Chapter 3](#chapter-3)

{% highlight ruby %}
def show
@widget = Widget(params[:id])
respond_to do |format|
format.html # show.html.erb
format.json { render json: @widget }
end
end
{% endhighlight %}

Testing the email address. Neat thing about it - powered by [Jekyll](http://jekyllrb.com) and I can use Markdown to author my posts. It actually is a lot easier than I thought it was going to be.

```js
grunt.initConfig({
assemble: {
options: {
assets: 'docs/assets',
data: 'src/data/*.{json,yml}',
helpers: 'src/custom-helpers.js',
partials: ['src/partials/**/*.{hbs,md}']
},
pages: {
options: {
layout: 'default.hbs'
},
files: {
'./': ['src/templates/pages/index.hbs']
}
}
}
};
```

``` markup
Sample text here...
```


<div class="class">
</div>


