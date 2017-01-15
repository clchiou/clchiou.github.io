---
layout: post
category: notes-effective-modern-c++
title: "Item 8: Prefer nullptr to 0 and NULL"
---

Because `0` and `NULL` doesn't work well with function overloads and template type deduction.

**`0` (an int) and `NULL` (an int or a long) are not pointers.
But if C++ finds itself looking at `0` in a context where only a pointer can be used,
it will interpret `0` as a null pointer.**

What's `nullptr`?

* `nullptr`'s type is `std::nullptr_t` and `std::nullptr_t` is defined to be the type of `nullptr`
  * Yup, this is a circular definition
  * That is, `nullptr` is not an integral type
* The type `std::nullptr_t` implicitly converts to all raw pointer types

### Problem with function overloads

```c++
void f(int);
void f(bool);
void f(void *);

// Call f(int), not f(void *)
f(0);

// It might not compile, but when it does,
// it calls f(int), never f(void *)
f(NULL);
```

### Problem with template type deduction

The re-interpretation of `0` as a null pointer is **after** template type deduction, and that causes problem:

```c++
void foo(std::unique_ptr<Foo> ptr);

template<typename F, typename T>
void call(F f, T x) {
  f(x);
}

// Error!  T is deduced to int, but
// std::unique_ptr ctor doesn't take int or long
// Note that foo(0) and foo(NULL) are okay though
call(foo, 0);
call(foo, NULL);

// Okay
call(foo, nullptr);
```
