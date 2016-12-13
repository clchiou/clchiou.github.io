---
layout: post
category: notes-effective-c++
title: "Item 33: Avoid Hiding Inherited Names"
---

Names in derived class hides names in base class (`virtual` won't changes this, it only tells runtime to lookup vptr table), which is usually undesirable.
To make hidden names visible again, employ `using` declarations or employ forwarding functions.
(This is especially annoying when base class defines a group of overloaded functions.)

Name lookup order:

* Local
* This class
* Base class
* Namespace(s) containing this class
* Global

Note: If base class has overloaded function, and you want to redefine or override only some of them, you **must** include a `using` declaration to make base class' names visible again.

Template brings a whole new different story.
