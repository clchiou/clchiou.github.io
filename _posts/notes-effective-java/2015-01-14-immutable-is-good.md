---
layout: post
category: notes-effective-java
title: Immutable is Good
---

Make a class immutable when you can
(and as immutable as possible when you can't).

Be careful:
Getters must not return a mutable reference to an internal state.

**You should not need to provide a copy-constructor on an immutable class.**
(Don't copy; just share.)

Don't provide an `initialize` method;
constructors or factory methods should fully initialize an object.

Don't provide a `reset` method for reusing an object;
just create a new object.
