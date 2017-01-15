---
layout: post
category: notes-effective-modern-c++
title: "Item 12: Declare Overriding Functions override"
---

Because it is easy to miss the 6 conditions for overriding to occur.

* The base class function must be virtual
* The function name must be identical (except for destructor)
* The parameter types must be identical
* The `const`ness must be identical
* The return type and exception specifications must be **compatible** (not necessarily identical)
* (C++11) The function's reference qualifiers must be identical
