---
layout: post
category: notes-effective-c++
title: "Item 35: Consider Alternatives to Virtual Functions"
---

Through NVI (non-virtual interface) idiom or the strategy pattern.

The core of NVI is: Base class defines **private** virtual function.

  * That's right, private; C++ allows derived class to redefine private virtual function (even though derived class cannot call it)
