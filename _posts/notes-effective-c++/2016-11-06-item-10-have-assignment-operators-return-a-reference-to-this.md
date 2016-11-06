---
layout: post
category: notes-effective-c++
title: "Item 10: Have Assignment Operators Return a Reference to *this"
---

This is a convention that you should follow (unless you have great reason not to).

{% highlight c++ %}
class Foo {
 public:
  Foo& operator=(const Foo& rhs) {
    // ...
    return *this;
  }

  // And +=, -=, etc.
  Foo& operator+=(const Foo& rhs) {
    // ...
    return *this;
  }

  // And assignment from other types, too.
  Foo& operator=(int rhs) {
    // ...
    return *this;
  }
}
{% endhighlight %}
