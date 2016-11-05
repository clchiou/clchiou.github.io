---
layout: post
category: notes-effective-c++
title: "Item 4: Make Sure That Objects Are Initialized Before They're Used"
---

Avoid initialization order problem across translation units! (Or just don't use globals?)

* Sometimes an object is guaranteed to be initialized and sometimes not.
  The exact rules are very complicated, but a (naive?) rule of thumb is that:
  * C part of C++: If initialization might incur runtime cost, it's not guaranteed to take place.
  * non-C part of C++: Things sometimes change.
  * So array (C part) is not guaranteed to be initialized but `std::vector` (non-C part) is.
  * Or just **always** initialize everything (don't memorize any rule).

* Use member initialization list instead of assignments.
* Meyers even suggests that you put **all** non-static members on the list.
  * This way, you don't have to memorize which member should be put on the list.
  * But you might duplicate the list if you overload constructors (in this case, you might not want to put all members on the list).
* Members should be listed in the same order as they are declared in the class (to avoid confusion).

* **The order of initialization of non-local static objects defined in different translation units is undefined.**
  * Don't use non-local static object.  Just don't.  When constructing a non-local static object, its constructor could (indirectly) refers to another non-local static object (and results in undefined behavior).
  * If you really want global objects, use this singleton pattern:
{% highlight c++ %}
Foo& getFoo() {
  static Foo foo;
  return foo;
}
{% endhighlight %}
  * This pattern is not thread-safe (in fact, any non-const static object is potentially not thread-safe).
