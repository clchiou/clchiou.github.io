---
layout: post
category: notes-effective-java
title: Never Export POD
---

POD (plain-old data) classes should never be exported (`public` or `protected`);
use getter/setter instead.

However, it is fine to use POD classes for implementation (`private` or package-private).
