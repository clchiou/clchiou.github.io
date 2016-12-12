---
layout: post
category: notes-effective-c++
title: 'Item 28: Avoid Returning "Handles" to Object Internals'
---

Handles include pointers, reference, and iterators (ways to get at other objects).
It decreases encapsulation and might result in dangling pointer.
You should only prefer using handles in certain specific scenarios (like containers).
