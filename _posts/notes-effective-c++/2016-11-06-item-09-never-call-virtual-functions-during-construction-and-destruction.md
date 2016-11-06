---
layout: post
category: notes-effective-c++
title: "Item 9: Never Call Virtual Functions During Construction and Destruction"
---

During construction and destruction, the base class's constructor and destructor can only see base class's virtual functions (the virtual function table is pointer to the base class) because at that point, derived classes do not exist (either is not initialized yet or has been destroyed).

* This also means that RTTI is the base class.

* If you really want "polymorphic" behavior, a workaround is that in derived class constructor you pass relevant data to base constructor. (But unfortunately this workaround doesn't apply to destructor.)

{% highlight c++ %}
class Base {
 public:
  Base(const std::string& name);
};

class Foo: public Base {
 public:
  Foo(parameters): Base(makeName(parameters)) {}
 private:
  // Tip: Use static member function for complex stuff
  // in constructor's member initialization list.
  static std::string makeName(parameters);
};
{% endhighlight %}
