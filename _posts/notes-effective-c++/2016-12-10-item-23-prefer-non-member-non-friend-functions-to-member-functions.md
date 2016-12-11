---
layout: post
category: notes-effective-c++
title: "Item 23: Prefer Non-Member Non-Friend Functions to Member Functions"
---

To increase encapsulation.

* Least member functions = minimum interface
* Encapsulate more = easier to change implementation
* Use `namespace` to help you organize classes and non-member non-friend functions
  * You may split stuff of the same `namespace` into pieces, but a class must be defined in its entirety
