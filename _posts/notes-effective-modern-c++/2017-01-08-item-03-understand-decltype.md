---
layout: post
category: notes-effective-modern-c++
title: "Item 3: Understand decltype"
---

`decltype` rules are sometimes preferred in type deduction.
Primary use case of `decltype`: When declaring function templates, make function's return type depends on its parameter type.

Let's examine `decltype` with a series of `get()` function.

### 1. Use the trailing return type syntax

C++11's trailing return type syntax **has nothing to do with `auto` type deduction**.
Advantage: You may use parameter types in specifying return type.

```c++
// Trailing return type syntax
template<typename C, typename I>
auto get(C& c, I i) -> decltype(c[i]) {
  return c[i];
}
```

### 2. Use the trailing return type syntax with type deduction

C++11 permits return type of single-statement lambdas to be deduced (again, nothing to do with `auto` type deduction though it reuses `auto` keyword).
C++14 extends this feature to all lambdas and all functions.

```c++
// C++14
// Trailing return type syntax with return type deduced
template<typename C, typename I>
auto get(C& c, I i) {
  // New problem: Deduced type is not a reference type
  return c[i];
}
```

The problem is that, with normal type deduction rules, the reference-ness is ignored, and the deduced return type is not a reference type.
So this won't compile: `get(container, 5) = 10`.

### 3. Use `decltype` rules in type deduction

To fix this, C++14 introduces the `decltype(auto)` specifier.
`auto` means using type deduction, and `decltype` means using `decltype` rules in type deduction, not the normal rules.

```c++
// C++14
template<typename C, typename I>
decltype(auto) get(C& c, I i) {
  return c[i];
}

// Now assignments work
get(container, 5) = 10;
```

### 4. Use universal reference

To support passing rvalue to `get`, we would use universal reference.
(This is not strictly `decltype` related, but it's nice to know.)
(Check Item 25 for more about universal reference and `std::forward`.)

```c++
// C++14
template<typename C, typename I>
decltype(auto) get(C&& c, I i) {
  return std::forward<C>(c)[i];
}

// C++11
template<typename C, typename I>
auto get(C&& c, I i) -> decltype(std::forward<C>(c)[i]) {
  return std::forward<C>(c)[i];
}

// Now rvalues work
auto x = get(makeContainer(), 7);
```

### `decltype` pitfall

* When `decltype` is applied to names, it yields the type of that name
* When `decltype` is applied to lvalue expressions, it generally ensures that the type reported is an lvalue **reference**
* Now the surprise: `x` is a name, but **`(x)` is lvalue expression**:

```c++
decltype(auto) f1() {
  int x = 0;
  return x;  // decltype(x) is int, so f1 returns int
}

// Surprise! Return reference to a local variable!
decltype(auto) f2() {
  int x = 0;
  return (x);  // decltype((x)) is int&, so f2 returns int&
}
```
