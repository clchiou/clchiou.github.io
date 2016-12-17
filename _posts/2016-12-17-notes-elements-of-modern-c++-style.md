---
layout: post
title: "Notes of Elements of Modern C++ Style"
---

Notes of Sutter's [Elements of Modern C++ Style](https://herbsutter.com/elements-of-modern-c-style/).

* Use `auto` whenever possible
  * Especially useful in writing `lambda` expression

```c++
auto const xlimit = config["xlimit"];
auto& s = singleton::instance();
auto x = [](int i) { return i > 42; };
```

* Don't `delete`, use smart pointers
  * Only use raw pointers:
    * When non-owning (and you are sure it's going to outlive you)
    * When implementing data structure
* Use `nullptr`, no more `0` or `NULL`
* Use range `for`
* Use non-member `begin(x)` and `end(x)`
  * And don't write `x.begin()` and `x.end()` anymore
  * Because non-member version `begin` and `end` are extensible and can be adapted to work with even arrays
* `lambda` makes STL algorithm more usable (and many more library are designed around `lambda`)
* Move semantics change the way we design APIs
  * Design return-by-value more often
  * Move can be thought of as an optimization of copy
  * Also enable other things like perfect forwarding
* Prefer uniform initialization and initializer lists
  * That is, prefer `{}` over `()`
  * This prevents:
    * Accidentally narrowing conversions (e.g., `float` to `int`)
    * Uninitialized POD member variables or arrays
    * [Syntax ambiguity](https://en.wikipedia.org/wiki/Most_vexing_parse)
  * But for non-POD and `auto`, continue using `=` syntax

```c++
// Is this a function declaration or a variable definition?
rectangle w(origin(), extents());

// But this is clear.
rectangle w{origin(), extents()};

// Continue using = syntax in these cases.
int a = 42;
auto x = begin(v);
```
