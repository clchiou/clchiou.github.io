---
layout: post
category: notes-effective-java
title: Class Accessibility Guide
---

Make each class or member as **inaccessible** as possible.

Accessibility:

* `private`: code from the same class or nested classes.
* Package-private: `private` plus code from the same package.
* `protected`: package-private plus code from sub-classes.
* `public`: anywhere.

`private` and package-private are considered implementation
whereas `protected` and `public` are considered exported API.

`protected` is rarely useful.

Package-private is useful for writing tests.
