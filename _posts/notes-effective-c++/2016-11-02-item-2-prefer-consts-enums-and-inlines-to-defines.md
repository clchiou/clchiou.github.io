---
layout: post
category: notes-effective-c++
title: "Item 2: Prefer consts, enums, and inlines to #defines"
---

Use as less preprocessor as possible (or even better, use as less non-zero globals as possible).

* You should use constant pointer:
{% highlight c++ %}
const char * const message = "Hello, World!";
// Or const object:
const std::string message("Hello, World!");
{% endhighlight %}

* Class-specific constant can be declared wit value **but not defined** (only for integral types).
{% highlight c++ %}
class Foo {
  // You don't need to define SIZE unless you take
  // address from it.
  static const int SIZE = 5;
  int array[SIZE];  // You may use its value.
}
{% endhighlight %}
