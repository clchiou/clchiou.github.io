---
layout: post
category: notes-effective-c++
title: "Item 11: Handle Assignment to Self in operator="
---

Self-assignment could happen when objects are aliased (e.g., `*px = *py`).

Three solutions:

* (Traditional) Test identity: `this == &rhs`.

* Exception-safe copy, which happens to be also self-assignment-safe, too (although not the most efficient self-assignment solution).
{% highlight c++ %}
Foo& Foo::operator=(const Foo& rhs) {
  Bar *orig = bar;
  bar = new Bar(*rhs.bar);  // Make a copy.
  delete bar;
  return *this;
}
{% endhighlight %}

* Copy-and-swap:
{% highlight c++ %}
Foo& Foo::operator=(const Foo& rhs) {
  Foo temp(rhs);  // Use copy constructor in operator=
  swap(temp);  // Then swap
  return *this;
}
{% endhighlight %}
