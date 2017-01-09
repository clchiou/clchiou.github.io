---
layout: post
category: notes-effective-modern-c++
title: "Item 6: Use the Explicitly Typed Initializer Idiom When auto Deduces Undesired Types"
---

General rule: "Invisible" proxy classes don't play well with `auto`.
Use explicitly typed initializer idiom to work around it.

```c++
// This might not be a great way of using std::vector,
// but this is just an example...
std::vector<bool> foo();

// Surprise! x is not bool-typed,
// but std::vector<bool>::reference -typed
// that reference to a temporary vector object
// (dangling pointer!)
auto x = foo()[5];
```

This example might be artificial, but accessing a proxy object that is referencing to a temporary object is not rare.

```c++
// Explicitly typed initializer idiom
// Emphasize you make this cast deliberately
auto x = static_cast<bool>(foo()[5]);
```
