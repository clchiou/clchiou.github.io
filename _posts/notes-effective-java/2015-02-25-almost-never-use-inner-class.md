---
layout: post
category: notes-effective-java
title: Almost Never Use Inner Class
---

Most likely, static member class or lambda expression is sufficient.

Java has four kinds of nested classes:

* Static member class.
* Non-static member class.
* Anonymous class.
* Local class.

The latter three together are called inner class.
Inner class cannot exist without a reference to its enclosing object,
which is an inconvenient limitation and sometime causes memory leak.
I could only think of one legitimate use of inner class,
which is returning a "view" of a collection, such as an iterator.
