---
layout: post
category: notes-effective-java
title: Use Builders
---

Use a build for breaking down constructor parameters,
or as a closure of a constructor.

Java does not support methods as a first-class citizen.
So if you want to pass a constructor to another method,
you need a builder.
