---
layout: post
category: notes-effective-java
title: Use Private Constructor in Utility Classes
---

To prevent someone instantiating an utility class, use private constructor.

{% highlight java %}
public class Utilities {
    // Enforce noninstantiability.
    private Utilities() {}
}
{% endhighlight %}

This also prevents someone sub-classing it.
