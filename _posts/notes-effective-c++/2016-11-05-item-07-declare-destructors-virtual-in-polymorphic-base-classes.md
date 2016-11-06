---
layout: post
category: notes-effective-c++
title: "Item 7: Declare Destructors Virtual in Polymorphic Base Classes"
---

On the other hand, classes not designed to be base class or be used polymorphically should not declare destructor virtual.

* Virtual function incur costs (space and speed) and so don't declare destructor virtual when you don't need to.
  * Not all base classes need a virtual destructor, e.g., the `Uncopyable` from previous item doesn't need it (because they are not intended to be used polymorphically).
* Aside, **don't inherit a class that is not designed for inheritance.**
  * Unfortunately C++ doesn't have `final class` as Java so we can't disallow that.
  * You might accidentally "slice" it and cause trouble; see example below.

{% highlight c++ %}
// Bad!  Inherit from std::string which is not
// designed for inheritance.
class MyString : std::string { ... };

MyString *pms = new MyString();

// Later you do this (which seems innocent, right?).
std::string *ps = pms;

// Boom! std::string destructor is non-virtual!
// You just "sliced" the MyString object.
delete ps;
{% endhighlight %}

* What if you want to define an abstract class without virtual functions?
  Answer: **Define** a pure virtual destructor (that's right, not simply declare, but define it).
  * Any class with a pure virtual function is an abstract class.
  * An abstract class cannot be instantiated.

{% highlight c++ %}
class Awov {  // Abstract w/o virtual functions.
 public:
  // Declared as pure virtual - this makes Awov
  // an abstract class.
  virtual ~Awov() = 0;
};

// Remember: You have to define it (otherwise
// derived class destructor cannot find it and
// this results in an linker error).
Awov::~Awov() {}
{% endhighlight %}
