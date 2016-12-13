---
layout: post
category: notes-effective-c++
title: "Item 29: Strive for Exception-Safe Code"
---

Exception safe means: Leak no resources and don't allow data structure to become corrupted (i.e., object invariances must be preserved).

* Basic guarantee: No leak and the state is valid but unpredictable (could be in any of the valid states)
* Strong guarantee: No leak and the state is the state before the call (this operation is **atomic**)
* Nothrow guarantee: This function always succeeds (all operations on built-in types are nothrow)

Exception safety is determined by implementation, not by declaration or its interface (this is unfortunate).
(That is, C++'s exception specification doesn't tell you anything about exception safety.  In fact, it doesn't event guarantee that this function won't throw exception.)

General rule: Don't change object state to indicate something has happened (say, file closed) until something actually has.

A common strategy: Copy and swap.

* Make a copy
* Modify the copy
* Swap copy with self

Side effect makes strong guarantee harder to achieve because it's hard to roll back to previous state.

Also, a strong guarantee function (say, implemented with copy-and-swap) might not be efficient.

It is hard (or impossible) to wraparound an exception unsafe (i.e., providing no guarantee) function and make it exception safe.
