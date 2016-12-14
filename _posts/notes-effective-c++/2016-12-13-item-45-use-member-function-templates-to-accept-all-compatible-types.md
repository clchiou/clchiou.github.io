---
layout: post
category: notes-effective-c++
title: 'Item 45: Use Member Function Templates to Accept "All Compatible Types"'
---

Or: How to make smart pointers do type conversion like raw pointers?
(Of course, the trick here doesn't just apply to smart pointers.)

Raw pointers do type conversions just fine:

```c++
class A {};
class B: public A {};
class C: public B {};

A* p = new B;  // Convert B* to A*.
A* q = new C;  // Convert C* to A*.
const A* r = p;  // Convert A* to const A*.
```

To make smart pointers do these:

```c++
template<typename T>
class SmartPtr {
 public:
  // Member function template that accepts
  // all compatible types.
  template<typenaem U>
  SmartPtr(const SmartPtr<U>& other)
  : ptr(other.get()) {}

  T* get() const { return ptr; }
 private:
  T* ptr;
};
```

And this compiles only if `U*` is convertible to `T*`.

And the trick doesn't stop here.
The "all compatible types" could be other smart pointers, raw pointers, or const pointers.

Note: Even if you define a generalized copy constructor (which is a member template), it doesn't mean compiler won't generate its own copy constructor.
If you want to control that behavior, you have to define a (non-templatized) copy constructor, too.
