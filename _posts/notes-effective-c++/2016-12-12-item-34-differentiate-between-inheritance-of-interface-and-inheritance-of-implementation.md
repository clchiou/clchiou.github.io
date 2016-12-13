---
layout: post
category: notes-effective-c++
title: "Item 34: Differentiate Between Inheritance of Interface and Inheritance of Implementation"
---

Consider: Pure virtual (with or without implementation), simple (impure) virtual, and non-virtual function.

* Pure virtual: Inherit interface only (and if it comes with an implementation, the derived class may explicitly choose to inherit the implementation)
  * Pure virtual must be redeclared in (direct) derived class (you may use this to force derived class explicitly choose the inherit implementation)
* Simple virtual: Inherit interface and implicitly inherit implementation
* Non-virtual: Inherit interface and a **mandatory implementation**
  * You may think of this as an invariance over specialization (all derived class will maintain this invariance)
