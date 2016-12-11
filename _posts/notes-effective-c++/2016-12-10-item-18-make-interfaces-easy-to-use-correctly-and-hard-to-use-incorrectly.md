---
layout: post
category: notes-effective-c++
title: "Item 18: Make Interfaces Easy to Use Correctly and Hard to Use Incorrectly"
---

Be consistent, and use type system as your primary ally in preventing undesirable code from compiling (i.e., prefer static checking).

* Have your types behave consistently with the built-in types
* Any interface that requires that clients remember to do something is prone to incorrect use
