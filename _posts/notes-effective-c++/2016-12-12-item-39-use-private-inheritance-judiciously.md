---
layout: post
category: notes-effective-c++
title: "Item 39: Use Private Inheritance Judiciously"
---

Private inheritance means "is-implemented-in-terms-of" (never "has-a" nor "is-a").

Some rare use cases for private inheritance:

* Access to protected members
* Prevent derived class from redefining a private virtual function
  * For example, you inherit this private virtual function from private inheritance of another class and you don't want your derived class to redefine it
  * Modern C++ has `final` keyword for just that
* EBO (empty base optimization)
  * Note that C++ forbids zero-size freestanding objects
  * But EBO might not work under multiple inheritance
