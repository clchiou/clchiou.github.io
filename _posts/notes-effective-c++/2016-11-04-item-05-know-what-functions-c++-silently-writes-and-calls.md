---
layout: post
category: notes-effective-c++
title: "Item 5: Know What Functions C++ Silently Writes and Calls"
---

The 4 bread-and-butter functions: default constructor, copy constructor, copy assignment operator, and destructor.

* Default constructor is generated if no constructor is declared.
* Copy constructor and copy assignment operator are generated when it makes sense/unambiguous (e.g., no reference-typed member and no const member).
* Destructor is generated (and declared `virtual` if needed) if no destructor is declared.
