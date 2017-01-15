---
layout: post
category: notes-effective-modern-c++
title: "Item 7: Distinguish Between () and {} When Creating Objects"
---

Uniform initialization is usually preferred,
but it may surprise you when combined with `std::initializer_list` and constructor overload resolution
(and template makes the situation even more frustrating).

4 syntax choices for object initialization:

* `int x(0);`
* `int y = 0;`
* `int z{0};` - this is the uniform initialization that can be used anywhere and express everything
* `int z = {0};`

### `{}`-initializer can be used anywhere

```c++
// You can't use ()-initializer to specify
// default initialization values for non-static
// data members (what's the rationale?)
class Foo {
  int x{0};   // Okay
  int y = 0;  // Okay
  int z(0);   // Error!
}

// When initializing uncopyable objects,
// you can't use `=`-initializer
// (what's the rationale?)
std::atomic<int> x{0};   // Okay
std::atomic<int> y = 0;  // Error!
std::atomic<int> z(0);   // Okay
```

### `{}`-initializer prohits implicit narrowing conversions among built-in types

```c++
double p, q;
...
int x{p + q};   // Error! Compiler is required to complain this
int y = p + q;  // Okay. Implicitly truncated to int
int z(p + q);   // Okay (ditto)
```

### `{}`-initializer immunes to C++'s most vexing parse

```c++
// Declaring a function, NOT calling Foo's default ctor
Foo foo();

// No ambiguity here
Foo foo{};
```

### `{}`-initializer pitfall

If one of the constructors declare a parameter of type `std::initializer_list`,
calls using `{}`-initializer syntax **strongly** prefer that over any other overloads
if there is **any way for compilers to convert parameter types to match it**.
Even if other overloads are clearly a better match.

This causes confusion; for example:

```c++
// Call std::initializer_list ctor and
// create a vector of 2-elements {10, 20}
stc::vector<int> b{10, 20};

// Call non-std::initializer_list ctor and
// create a vector of 10-elements,
// all of them have value of 20
stc::vector<int> p(10, 20);
```

(By the way, empty `{}` means no arguments, not an empty `std::initializer_list`.
To have an empty `std::initializer_list`, you write `Foo x({});` or `Foo x{ {} };`.)

**Recommendation: Design your class so that the overload called isn't affected by whether clients use `()`-initializer or `{}`-initializer.**

Things get worse for template authors:

```c++
template<typename T, typename... Ts>
void foo(Ts&&... params) {
  // Call std::initializer_list ctor
  T b{std::forward<Ts>(params)...};
  // Call non-std::initializer_list ctor
  T p(std::forward<Ts>(params)...);

  // As a template author, you can't know in advance
  // which of `b` and `p` is intended by your caller :(
}

// Does the caller intend for
// {}-initializer or ()-initializer?
// That you can't know.
foo<std::vector<int>>(10, 20);
```
