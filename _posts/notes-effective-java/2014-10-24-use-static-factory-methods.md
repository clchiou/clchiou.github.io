---
layout: post
category: notes-effective-java
title: Use Static Factory Methods
---

Static factory methods let you implement object pool
or return sub-types,
which is useful in a service-provider scenario.
However, be careful about the
[open-and-init](/2014-04-14/open-and-init-anti-pattern/)
anti-pattern.

A static factory method should immediately return an object to its caller
once the object is constructed to a "close-able" state.
After that,
the caller may manage the object with a `try`-with-resources statement
and continues initializing the object.
Should the object raises any exception during the initialization,
it is closed properly.
