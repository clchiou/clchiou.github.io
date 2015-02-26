---
layout: post
category: notes-effective-java
title: Notes on the Strategy Pattern
---

The lambda expression introduced in Java 8 makes writing the strategy pattern easier.

Pitfalls of writing the strategy pattern (aka function objects):

* Use inner class too casually, resulting in memory leak.
* Create too many function objects (ideally, if it is stateless, you should create one statically and reuse it).

A lambda expression is tied to a functional interface (interface with only one method declaration).
So writing function objects of this kind (which is the majority of function objects, by the way) is a lot easier in Java 8.
