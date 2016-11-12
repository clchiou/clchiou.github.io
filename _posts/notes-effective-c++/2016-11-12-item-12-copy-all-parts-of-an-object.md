---
layout: post
category: notes-effective-c++
title: "Item 12: Copy All Parts of an Object"
---

The two copying functions: copy constructor and copy assignment operator.
You must pay special attention if you override the default ones.

* When you add a data member, you must update **all** non-default copying functions.

* You must call **all base classes'** respective copying functions in your copying functions (doubly true when you add new base classes to a class).

* Having one copying function call the other is usually wrong (although it may seems to reduce code duplication).  It usually doesn't make sense to call copy assignment operator in copy constructor or vice versa.  **Because initialization (copy constructor) and copying (copy assignment operator) are fundamentally different.**

* If code duplication among copying functions is an issue, you should probably create a third function (say, `init`) that all copying functions call.
