---
layout: post
category: notes-effective-java
title: Don't Write Tagged Class
---

What's worse than a class hierarchy?
Tagged class.

{% highlight java %}
// Don't do this!
public class Figure {
    enum Shape { RECTANGLE, CIRCLE };
    final Shape shape;
    ...
}
{% endhighlight %}
