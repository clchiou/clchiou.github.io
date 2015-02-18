---
layout: post
title: Java Constructor Happens Before
---

If an object is constructed in one thread and used in another,
could another thread sees uninitialized or partially initialized states of the object?
Answer:
[No](http://docs.oracle.com/javase/specs/jls/se7/html/jls-17.html#jls-17.4.5).

JLS specifies a happens-before edge from the end of a constructor.
In this sense, a constructor is special when compared to an ordinary method.
It is like all constructors are automatically guarded by a `synchronized` block.
