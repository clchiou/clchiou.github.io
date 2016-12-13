---
layout: post
category: notes-effective-c++
title: "Item 40: Use Multiple Inheritance Judiciously"
---

When diamond inheritance is an issue, public inheritance should probably always be virtual inheritance.
(But remember, virtual inheritance incurs extra costs - size, speed, and complexity of code you need to write.)
