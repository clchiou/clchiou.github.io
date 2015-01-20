---
layout: post
category: notes-effective-java
title: Abstract Class Is Less Needed
---

Java 8 introduces interface default method,
making the most common use of abstract classes, skeletal implementation, less needed.

By the way, a widely used public interface was very hard to change
(because you would break all clients implementing it).
The default method makes adding methods to an interface easier
(changing an existing method is still hard).
