---
layout: post
category: notes-effective-java
title: Tips for Overriding toString
---

TL;DR Always override `toString`.

* Always override `toString`; don't use `java.lang.Object`'s version of `toString`.
* Short and informative.
  You should be able to re-construct (most of) the object from the representation.
* You should provide getter methods to all of the member fields related to the representation.
* In my opinion, it is usually not a good idea to "standardize" the format of the representation.
  You should provide explicit methods for generating canonical representations for parsing and/or serialization.
