---
layout: post
category: notes-effective-c++
title: "Item 44: Factor Parameter-Independent Code Out of Templates"
---

To reduce code size (when/if output binary size is an issue).
(By the way, member functions are only instantiated only when used.)

* Non-type template parameter could be turned into a function parameter or class data member

* Type template parameter (if you think they actually have the same binary representation, like all pointer types do) could share the same implementation (like using `void *`).
