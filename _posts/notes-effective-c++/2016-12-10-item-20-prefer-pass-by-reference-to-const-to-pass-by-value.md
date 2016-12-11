---
layout: post
category: notes-effective-c++
title: "Item 20: Prefer Pass-by-Reference-to-const to Pass-by-Value"
---

Because it's (most likely) more efficient and avoids the slicing problem.

```
void foo(const Bar& bar);
```

* Pass-by-value needs 1 copy constructor and 1 destructor call (usually less efficient)
  * But built-in types are more efficient to pass it by value
* References are usually implemented as pointers
* A small object doesn't mean its copy constructor/destructor is fast
* Compilers usually treat objects (even small one with only one integer member) different from a built-in type, and refuse to store it in register
* Exception: STL iterator (which is modelled after pointer types) and STL function object types should be passed by value
