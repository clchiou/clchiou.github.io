---
layout: post
category: notes-effective-modern-c++
title: "Introduction"
---

C++11's most pervasive feature is probably **move semantics**,
and the foundation of it is **distinguishing expression that are rvalues from those that are lvalues**.

* To show how confusing rvalue vs lvalue could be, a parameter of rvalue reference type is itself an lvalue

```c++
// bar is an lvalue,
// though it has an rvalue reference type.
void foo(Bar&& bar);
```

* C++11 makes distinction between parameter and argument
  * (See perfect forwarding)
  * Function's parameter is an lvalue
  * Argument passing to a function could be either rvalue or lvalue

```c++
// Parameter is an lvalue
void foo(Bar bar);

// bar is a copy-constructed copy of x
foo(x);

// bar is a move-constructed copy of x
foo(std::move(x));
```

* Perfect forwarding is to preserve the original argument's rvalueness or lvalueness
* Regrettable, no terminology yet that distinguishes between copy-constructed copy and move-constructed copy
* (vs declaration) Definition provides the storage locations or implementation details
* Meyers defines a function's signature = return type + parameter types
  * `noexcept` or `constexpr` are excluded from signature
  * C++ standard's definition is slightly different
* Prefer `nullptr` over `0` or `NULL`
* Prefer alias declaration over `typedef`s
