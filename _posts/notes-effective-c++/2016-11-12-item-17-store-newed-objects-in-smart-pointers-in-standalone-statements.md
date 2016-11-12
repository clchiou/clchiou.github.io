---
layout: post
category: notes-effective-c++
title: "Item 17: Store newed Objects in Smart Pointers in Standalone Statements"
---

Because the evaluation order of **function designator, arguments, and subexpression within arguments** is unspecified.

This statement
{% highlight c++ %}
bar(std::shared_ptr(new Foo), baz());
{% endhighlight %}

...could be evaluated in this order:

  * `new Foo`
  * `baz()`
  * `std::shared_ptr(...)`
  * `bar(...)`

And if `baz()` throws an exception, the `new`ed `Foo` object is leaked.
