---
layout: post
category: notes-effective-modern-c++
title: "Item 2: Understand auto Type Deduction"
---

Same as template type deduction, except for `std::initializer_list`.

Special type deduction rule for `auto`: When the initializer for an `auto`-declared variable is enclosed in braces, the deduced type is a `std::initializer_list`.

```c++
auto x1 = 27;    // x1: int
auto x2(27);     // x2: int
auto x3 = {27};  // x3: std::initializer<int>
auto x4{27};     // x4: std::initializer<int>
```

(By the way, under N3922 (not C++11 nor C++14), when using direct initialization syntax, (i.e., `auto x4{27}`), the type of `x4` is `int`, not `std::initializer_list`.)

In other words, `auto` type deduction **assumes** that a brace initializer represents a `std::initializer_list`, but template type deduction doesn't.
(Scott Meyers doesn't know the rationale behind this special rule, either.)

```c++
// This compiles due to the special rule for auto
auto x = {1, 2, 3};

// This doesn't compile
template<typename T> void f(T param);
f({1, 2, 3});  // Error! Can't deduce type for T

// This compiles
template<typename T> void f(std::initializer_list<T> initList);
f({1, 2, 3});
```

C++14: You may put `auto` in function's return type and lambda's parameter declaration.
**While C++ reuses the `auto` keyword, it is template type deduction rule that applies in these places, not `auto` type deduction rule.**
(This is so confusing...)

```c++
// C++14
auto foo() {
  return {1, 2, 3};  // Error! Can't deduce type for {1, 2, 3}
}

// C++14
auto foo = [&v](const auto& newValue) { v = newValue };
foo({1, 2, 3});  // Error! Can't deduce type for {1, 2, 3}
```
