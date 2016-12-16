---
layout: post
category: notes-effective-c++
title: "Item 50: Understand When It Makes Sense to Replace new and delete"
---

Probably never - it's very hard to write a conformant and performant memory allocator
(for just the tip of the iceberg, a conformant allocator must return aligned memory addresses).
