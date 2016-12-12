---
layout: post
category: notes-effective-c++
title: "Item 25: Consider Support for a Non-Throwing swap"
---

(Or: How to define `swap` the right way so that the compiler may find the correct one.)
There are four swaps to consider: the default `std::swap`, member `swap`, non-member `swap` in the same namespace, and specialized `std::swap`.
You need to assist compiler find the best one to use.
And all call sites needs to tell compiler that it should choose the best one (don't hard-coded `std::swap`).

Why `swap` is important:

* `swap` is important to exception-safe programming
* `swap` is used for coping with assignment-to-self

How to define `swap` for non-template class:

* In general we are not permitted to alter the contents of the `std` namespace
* But we are allowed to totally specialize standard templates (like `swap`) for our types

```c++
class Foo {
 public:
  // Define the member swap.
  void swap(Foo& other) {
    // Make default std::swap available.
    using std::swap;
    // Don't write `std::swap(...)` here.
    // Let compiler figure out which swap to use!
    swap(contents, other.contents);
  }
};

namespace std{
  // Define the totally specialize std::swap.
  template<>
  void swap<Foo>(Foo& a, Foo& b) {
    a.swap(b);  // Call the member swap.
  }
}
```

How to define `swap` for template class:

* C++ doesn't allow partially specialize for function templates (but class templates are allowed (why?))

```c++
// Partially specialize a function template.
// Illegal C++!
template<typename T>
void swap<Foo<T>>(Foo<T>& a, Foo<T>& b);
```

* To overcome this, you use function overload instead (which looks like a hack to me)
  * But you are not allowed to alter `std` namespace!
* To overcome that, you define your overload in your own namespace

```c++
namespace foo {
  template<typename T>
  class Foo { ... };

  // Define the non-member swap that overloads swap.
  // Must be in the same namespace of Foo.
  template<typename T>
  void swap(Foo<T>& a, Foo<T>& b) {
    a.swap(b);  // Call the member swap.
  }
}
```

* This way, the name lookup rule (called **argument-dependent lookup** or **Koenig lookup**) will find the `Foo`-specific version in the `foo` namespace

Finally, note that **all call sites should not hard-coded `std::swap`.**
Instead, it should:

```c++
template<typename T>
void bar(T& a, T& b) {
  // Make default std::swap available.
  using std::swap;
  ...
  // Don't hard-coded std::swap here.
  // Let compiler figure out which one to use!
  swap(a, b);
  ...
}
```
