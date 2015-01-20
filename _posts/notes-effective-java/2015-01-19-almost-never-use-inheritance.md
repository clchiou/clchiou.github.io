---
layout: post
category: notes-effective-java
title: Almost Never Use Inheritance
---

Inheritance is safe only within a package or when a class is explicitly designed for extension.

Think twice before going for inheritance.
And if you do decide to go for it.
Firstly, inheritance must strictly follow a "is-a" relationship.

Inheritance breaks encapsulation in the sense that
a subclass depends on the implementation details of its superclass.
(So it is relatively safe within a package because it is considered private within a package.)
So when a class is designed for extension,
you must document throughly not only _what_ a method does but also **how** a method does it
so that a subclass can be properly implemented.
