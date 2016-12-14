---
layout: post
category: notes-effective-c++
title: "Item 43: Know How to Access Names in Templatized Base Classes"
---

Via `this->` prefix, via `using` declaration, or via an explicit base class qualification.

Compiler refuses to look into templatized base classes for inherited names
(when we cross from OOP C++ to template C++, inheritance stops working).
To fix this, you could:

* Add `this->` prefix as

```c++
this->nameFromBaseClass
```

* Add `using` declaration

```c++
template<T>
class Foo: public Bar<T> {
  using Bar<T>::baz;
  void foo() {
    baz();
  }
};
```

* Base class qualification (least preferable)

```c++
template<T>
class Foo: public Bar<T> {
  void foo() {
    // Don't do this if baz() if a virtual function!
    Bar<T>::baz();
  }
};
```
