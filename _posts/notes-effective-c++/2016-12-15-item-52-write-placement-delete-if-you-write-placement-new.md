---
layout: post
category: notes-effective-c++
title: "Item 52: Write Placement delete If You Write Placement new"
---

Any `operator new` overload that accepts no just `std::size_t` as parameter is called a "placement `new`"
(yeah, the name "placement `new`" is overloaded, too).
You **must define** a paired overloaded version of `operator delete`, or you are in serious trouble,
because when compiler can't find one, it just **gives up deleting** the memory when a constructor fails (leak!).

Also, since it's just an overloaded member function, it hides all other overloaded functions in the global scope (i.e., `operator new` defined in standard library).
This is usually a problem.

(By the way, the "nothrow" form of `operator new` is a placement `new`, too.  It's one of the three `operator new` defined in the standard library, with the other two be the normal `new` and the "real" placement `new`.)
