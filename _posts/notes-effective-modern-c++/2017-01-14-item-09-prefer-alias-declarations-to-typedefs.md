---
layout: post
category: notes-effective-modern-c++
title: "Item 9: Prefer Alias Declarations to typedefs"
---

Because alias declarations may be templatized but `typedef`s can't.

```c++
template<typename T>
using MyAllocList = std::list<T, MyAlloc<T>>;

MyAllocList<Foo> lw;

template<typename T>
class Foo {
  MyAllocList<T> list;
};
```

The equivalent code with `typedef` is:

```c++
template<typename T>
struct MyAllocList {
  typedef std::list<T, MyAlloc<T>> type;
};

MyAllocList<Foo>::type lw;

template<typename T>
class Foo {
  typename MyAllocList<T>::type list;
};
```

In C++14, type traits are migrated to alias declarations:

```c++
#include <type_traits>
std::remove_const<T>::type  // C++11
std::remove_const_t<T>      // C++14
```
