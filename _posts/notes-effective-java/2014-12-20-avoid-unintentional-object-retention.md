---
layout: post
category: notes-effective-java
title: Avoid Unintentional Object Retention
---

Unintentional object retention (or informally called memory leaking) can be fixed by
nulling out references once they become obsolete.

* But don't overcompensate by nulling out every object references!
  You usually need to null out references when you manage memory yourself
  (say, implementing a stack).
* The second common source of memory leaking is cache.
  - Use weak references or `WeakHashMap`.
* The third common source (and harder to be noticed) is listeners or callbacks.
  - If a user forgets to deregister his callbacks, these callbacks are leaked.
  - Use weak references for callbacks.
