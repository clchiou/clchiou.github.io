---
layout: post
category: notes-effective-c++
title: "Item 27: Minimize Casting"
---

Avoid casts whenever practical.
There are 5 casts: 1 C-style and 4 C++ style casts.

C-style cast:

* `(T)expression`
* `T(expression)`

These two forms are equivalent (just a matter of taste).

C++-style casts:

* `const_cast` - to cast away `const`-ness (use it occasionally)
* `dynamic_cast` - to perform safe downcasting, which has significant runtime cost (don't use it!)
* `reinterpret_cast` - to perform low-level casts, like casting a pointer to an int (only use it in low-level code)
* `static_cast` - to force implicit conversion, like `void *` to typed pointers, but can't cast away `const`-ness (use it occasionally)

Call explicit constructor:

```c++
class Foo {
 public:
  explicit Foo(int);
};

void bar(const Foo&);

// Create a temporary Foo object from int
// with function-style cast.
bar(Foo(42));

// Or with C++-style cast.
bar(static_cast<Foo>(15));

// These two casts do exactly the same thing though.
// Just a matter of taste.
```

Notes:

* A cast does not just inform compiler, it also incurs runtime cost (as the example above, **it creates a temporary object**)
  * Even `int`-to-`double` conversion incurs runtime cost
* Because casts might create temporary objects, you should be extra careful
  * You might actually be calling member function of a temporary object and the results are lost immediately
* Pointer-to-base and pointer-to-derived **might not be the same** (usually but not limited to when multiple inheritance is in use)
