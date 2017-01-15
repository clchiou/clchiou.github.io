---
layout: post
category: notes-effective-modern-c++
title: "Item 10: Prefer Scoped enums to Unscoped enums"
---

Scoped `enum`s are better because they are scoped, strong-typed, and may be forward-declared out-of-the-box.

```c++
enum Shape {
  rectangle, square, triangle
};

enum class Color {
  black, white, red
};

//// Advantage 1: Scoped

// Error! Conflict with `rectangle` of `Shape`
auto rectangle = false;
// `Shape`'s members are polluting the scope containing it!
Shape s = rectangle;

// `white` won't conflict with Color::white
auto white = false;
// It must be accessed with scope
Color c = Color::white;

//// Advantage 2: Strong-typed

// Okay, implicitly convert to double
if (s < 1.9) {
  ...
}

// Error!
if (c < 1.9) {
  ...
}
```

**Advantage 3**: Scoped `enum`s may be forward-declared out-of-the-box.
The reason is that the underlying type of scoped `enum`s are default to `int`, while for unscoped `enum`s you have to explicitly specify it.
(If you specify the underlying type for an unscoped `enum`, it may be forward declared.)

You may specify underlying type with this syntax form:

```c++
// Specify the underlying type and also forward-declare it
enum class Color: std::uint32_t;
enum Shape: std::uint32_t;
```

**Advantage(?) of unscoped `enum`**:
Unscoped `enum`s are probably only useful in defining lists of integral constants at the global scope.
