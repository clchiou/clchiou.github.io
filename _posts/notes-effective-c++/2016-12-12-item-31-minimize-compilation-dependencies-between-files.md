---
layout: post
category: notes-effective-c++
title: "Item 31: Minimize Compilation Dependencies Between Files"
---

General idea: Depend on declaration, not definition.
You might need to alter class design to accommodate this (see handle class pattern or interface class pattern).

Tips:

* Avoid using objects when object references and pointers will do
* Depend on class declaration instead of class definitions whenever you can (note: you don't need class definition to declare a function using that class)
* Provide a separate header files for declarations and definitions (this looks like an overkill)
