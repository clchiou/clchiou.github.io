---
layout: post
category: notes-effective-c++
title: "Item 14: Think Carefully About Copying Behavior in Resource-Managing Classes"
---

Copying resources is not always the behavior you want (you probably want to prohibit copying and support transferring ownership).

Here are four resource-management object's copying behavior:

* Exclusive ownership and unique resource: Prohibit copying (say, inherit from `Uncopyable`).

* Shared ownership and unique resource: Reference-count the underlying resource when make a copy.

* Exclusive ownership but non-unique resource: Make a deep copy (less common approach).

* Exclusive ownership and unique resource: Transfer ownership (in C++11 and beyond it is called "move").

Note: In modern C++ (C++11 and beyond) you should mostly be moving rather than copying resources.
