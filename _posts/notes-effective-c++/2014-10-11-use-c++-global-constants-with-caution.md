---
layout: post
category: notes-effective-c++
title: Use C++ Global Constants with Caution
---

What are the ramifications of C++ global constants?

Item 2 of the Effecitve C++ recommends against `#define`s.
This is very good advice in general; however...

The execution order of static initializers is undefined; see [`static` initialization order fiasco](http://www.parashift.com/c++-faq/static-init-order.html).

A ramification in program start-up time is discussed in this [thread](https://groups.google.com/a/chromium.org/forum/#!topic/chromium-dev/p6h3HC8Wro4)
of Chromium development--even seemingly innocent constant definitions may affect the start-up time.
Consider this snippet:

{% highlight c++ %}
extern const int b;
const int a = b + 1;
{% endhighlight %}

In this case, compiler still needs to generate static initializers for `a` because
the value of `b` is unknown at compile time.

Should we consider banning global constants entirely?
