---
layout: post
category: notes-effective-c++
title: "Item 36: Never Redefine an Inherited Non-Virtual Function"
---

Non-virtual functions are **statically bound** (no lookup of vptr table at runtime).
So redefining it is almost never right (you are breaking the core principle that public inheritance models "is-a" relation).
