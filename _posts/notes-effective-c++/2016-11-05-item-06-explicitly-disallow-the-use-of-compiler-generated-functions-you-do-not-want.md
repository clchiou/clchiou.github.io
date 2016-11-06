---
layout: post
category: notes-effective-c++
title: "Item 6: Explicitly Disallow the Use of Compiler-Generated Functions You Do Not Want"
---

Declare (**but don't define**) what you don't want and compiler won't generate it (default constructor, copy constructor, copy assignment operator, and destructor).

For example, you want to disallow assignment:
{% highlight c++ %}
class Foo {
 private:
  Foo(const Foo&);  // Declare but don't define it!
  Foo& operator=(const Foo&);
};
{% endhighlight %}

Or define a uncopyable base class:
{% highlight c++ %}
class Uncopyable {
 protected:
  Uncopyable() {}
  ~Uncopyable() {}  // Note it's not virtual!
 private:
  Uncopyable(const Uncopyable&);
  Uncopyable& operator=(const Uncopyable&);
};

// Use private inheritance.
class Foo : private Uncopyable { ... };
{% endhighlight %}

* Note that `Uncopyable`'s destructor needs **not** to be virtual.
* The empty base class optimization might not work in the presence of multiple inheritance, but in general you should not worry about this (and other C++ subtleties).
* Boost has it and calls it `noncopyable`.
