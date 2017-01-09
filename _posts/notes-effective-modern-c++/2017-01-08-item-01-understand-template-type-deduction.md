---
layout: post
category: notes-effective-modern-c++
title: "Item 1: Understand Template Type Deduction"
---

Three general cases: 1. Reference or pointer but not universal reference; 2. Universal reference; 3. Neither reference nor pointer.
Edge cases: Array arguments and function arguments.

We will use this example through out the note:

```c++
template<typename T>
void f(ParamType param);
f(expr);  // Deduce ParamType and T from expr.
```

### Case 1: `ParamType` is a reference or pointer, but not a universal reference

Rule to deduce `T`:

1. If `expr`'s type is a reference, ignore reference part
2. Then pattern-matching `expr`'s type against `ParamType` to determine `T`

```c++
template<typename T>
void f(T& param);

int x = 27;
const int cx = x;
const int& rx = x;

// cx's and rx's const-ness is preserved in T
// rx's reference-ness is stripped from T
f(x);   // T: int        ParamType: int&
f(cx);  // T: const int  ParamType: const int&
f(rx);  // T: const int  ParamType: const int&

template<typename T>
void f(const T& param);
// cx's and rx's const-ness is still respected
// (ParamType is reference-to-const)
// though not preserved in T
f(x);   // T: int  ParamType: const int&
f(cx);  // T: int  ParamType: const int&
f(rx);  // T: int  ParamType: const int&
```

### Case 2: `ParamType` is an universal reference

Note: `T&&` is an universal reference but `int&&` is just an rvalue reference (they use the same syntactic form, don't be confused)

Rule to deduce `T`:

* If `expr` is an lvalue, both `T` and `ParamType` are deduced to be lvalue reference
  * That's **doubly unusual**
  * This is the **only** place `T` is deduced to be a reference
  * `ParamType` is declared using **rvalue reference syntax** but is deduced to be an **lvalue reference**
* If `expr` is an rvalue, go to case 1

```c++
template<typename T>
void f(T&& param);

// expr is an lvalue
f(x);   // T: int&        ParamType: int&
f(cx);  // T: const int&  ParamType: const int&
f(rx);  // T: const int&  ParamType: const int&

// expr is an rvalue
f(27);  // T: int  ParamType: int&&
```

### Case 3: `ParamType` is neither a pointer nor a reference

Rule to deduce `T`:

1. If `expr`'s type is a reference, ignore reference part
2. Then, if `expr` is `const` and/or `volatile`, ignore them, too

```c++
template<typename T>
void f(T param);

f(x);   // T: int  ParamType: int
f(cx);  // T: int  ParamType: int
f(rx);  // T: int  ParamType: int

const char* const ptr = "...";
f(ptr);  // T and ParamType: const char* (note: not char*)
```

### Edge case: Array arguments

In many contexts, array **decays** into a pointer to its first element (that's why array looks like a pointer but it isn't)

Note: This is C-legacy: `void foo(int param[])` is treated the same as `void foo(int *param)`, meaning:

```c++
template<typename T> void f(T param);
f(name);  // T: const char*
```

So you can't declare a true array-typed argument, but you can declare a reference to array.

```c++
template<typename T> void f(T& param);
f(name);  // T: const char[13]
```

Note that the dimension is preserved.
You could do tricks like this:

```c++
template<typename T, std::size_t N>
const_expr std::size_t arraySize(T (&)[N]) noexcept {
  return N;
}
```

### Edge case: Function arguments

Function types can **decay** into function pointers.
So like arrays, if it's passed by value, it decays, but if it's passed by reference, it doesn't.
Although the difference between reference to function and pointer to function doesn't seem to be matter in practice.
