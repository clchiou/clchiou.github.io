---
layout: post
category: notes-effective-c++
title: 'Item 32: Make Sure Public Inheritance Models "Is-A"'
---

Note: Liskov substitution principle (LSP).

* public/protected/private * virtual/non-virtual = 6 combinations in C++ inheritance
* "is-a" relation is trickier than you think; you should be pragmatic and try don't to design a overly complicated class hierarchy
* Private inheritance is something different (has-a or is-implemented-in-terms-of)
* Protected inheritance is something we don't understand what it means, even today
