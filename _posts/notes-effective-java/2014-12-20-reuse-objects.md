---
layout: post
category: notes-effective-java
title: Reuse Objects
---

Reuse is the key to avoid creating unnecessary objects.

* Reuse immutable objects.
* Reuse mutable objects when you can.
  - If you know they are private and are not changed once created.
    * Make them private static final.
    * The impact on memory footprint and start-up time is most likely unobservable.
  - If it is not private but can be shared. (But be very careful!)
    * It is okay to share adapters that are backing the same object.
    * Say, `keySet` method of the `Map` interface.
* Watch autoboxing.
  - Prefer primitives to boxed primitives.
* Modern JVM is good at allocating lots POD-type of objects.
  - **Maintaining your own object pool is generally a bad idea.**
    Trust modern JVM's highly optimized garbage collectors.
  - Unless it is a pool for extremely heavyweight objects, such as database connections.
