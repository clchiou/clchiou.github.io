---
layout: post
category: notes-effective-c++
title: "Item 42: Understand the Two Meanings of typename"
---

Two use cases: To use in template declaration and to give compiler a hint that this name is a type.

Since compiler will assumes a name is **not** a type unless you tell it otherwise (this happens to **nested** dependent names):

```c++
template<typename T>
void foo()
{
  // Tell compiler that T::bar is a type.
  // And this is defining a local variable,
  // not a multiplication.
  typename T::bar* x;
  ...
}
```

Why only to nested dependent names?
Because a class may define a nested class or static data member.
And a nested dependent name `T::bar` could be either one of them (you have to tell compiler it is which one of them).

Note: But you are not allowed to precede `typename` in a list of base classes or in a member initialization list (this is inconsistent, but ...).

```c++
template<typename T>
class Foo: public Bar<T>::Nested {  // typename not allowed
 public:
  Foo(int x): Bar<T>::Nested(x) {}  // typename not allowed
}
```

By the way, there is no difference between using `class` or using `typename` in template declaration.

```c++
// No difference at all; just a matter of taste.
template<class T> class Foo;
template<typename T>class Foo;
```

(But compilers actually aren't very strict about these rules.)
