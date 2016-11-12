---
layout: post
category: notes-effective-c++
title: "Item 16: Use the Same Form in Corresponding Uses of new and delete"
---

`delete`-for-`new` and `delete[]`-for-`new[]`.

* **All constructors** should use the same form of `new` so that destructor may use the correct form of `delete`.

* **Don't `typedef` array type**, which may result in mismatched `new` and `delete` form if you aren't careful.
{% highlight c++ %}
typedef Foo FooArray[4];
Foo *p = new FooArray;  // Implicitly call new[].
delete p;  // Call delete, not delete[].
{% endhighlight %}

Aside: Arrays just don't mix well with modern C++.  You should avoid them as much as you can.
