---
layout: post
category: notes-effective-modern-c++
title: "Item 11: Prefer Deleted Functions to Private Undefined Ones"
---

Delete functions replace the private-undefined-function trick
and there are some nice tricks that only delete functions can do.

Deleted functions should be **declared public** because compiler checks accessibility before deleted status.

(vs private-undefined-function trick) Deleted functions err during compile time, not until link time.

**Trick 1**: Disable certain implicit parameter type conversion:

```c++
bool isLucky(int number);
bool isLucky(char) = delete;
bool isLucky(bool) = delete;
bool isLucky(double) = delete;
// Why no isLucky(float)?
// When present a float,
// C++ prefers convertint it
// to a double than int.
```

**Trick 2**: Disable certain template instantiations:

```c++
template<typename T>
void foo(T* ptr);

template<>
void foo<void>(void*) = delete;

template<>
void foo<const void>(const void*) = delete;
```
