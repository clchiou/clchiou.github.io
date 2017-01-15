---
layout: post
category: notes-effective-modern-c++
title: "Item 13: Prefer const_iterators to iterators"
---

Because in C++11 `const_iterator` is finally usable.

```c++
std::vector<int> values;
...
auto it = std::find(values.cbegin(), values.cend(), 1983);
values.insert(it, 1998);
```

Since we don't intend to modify the element pointed by the iterator but merely want to append an element after it (hence `const_iterator`).
This cannot be done in C++98 (you have to use plain `iterator`).

`const_iterator`s are STL's pointer-to-`const`.

You should call non-member version of `begin`/`cbegin`/`end`/`cend`/etc. in templates
because non-member versions work with non-containers, too.

```c++
template<typename T>
void foo(T& container) {
  // Check Effective C++ Item 25 for the
  // rationale behind `using` declarations
  using std::cbegin;
  using std::cend;

  cbegin(container);
  cend(container);
}
```
