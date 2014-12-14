---
layout: post
category: notes-effective-java
title: Singleton Classes
---

The most common way is using private constructor with static final member or
factory method.

{% highlight java %}
public class Elvis {
    public static final INSTANCE;
    private Elvis() { ... }
}
{% endhighlight %}

Or you may use an enum type with one element.

{% highlight java %}
public enum Elvis {
    INSTANCE;
}
{% endhighlight %}

Note that singleton classes make their clients harder to test
(you cannot easily mock them out).
