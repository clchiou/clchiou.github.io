---
layout: post
category: notes-effective-modern-c++
title: "Item 4: Know How to View Deduced Types"
---

In additional to IDE support,
you may use compiler diagonostics (e.g., use declared but undefined templates).

```c++
// Declared but undefined
template<typename T>
class TypeDisplayer;

// Compiler will report an error with x's type
TypeDisplayer<decltype(x)> xType;
```

Note that not all sources of type information are reliable.

* `std::type_info::name` is not reliable (the standard requires it to be unreliable)
* Sometimes IDE-reported type info is not reliable, too

If you don't mind runtime costs, `Boost.TypeIndex` is a nice choice.
