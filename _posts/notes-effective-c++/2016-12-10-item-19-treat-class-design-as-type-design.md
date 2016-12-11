---
layout: post
category: notes-effective-c++
title: "Item 19: Treat Class Design as Type Design"
---

Because a new class is a new type.

Aspects of a new class to consider:

* How should you create/destroy/allocate/deallocate it
* How should object initialization differ from assignment and differ from movement
* Does it support pass-by-value (also see copy constructor)
* Valid range of object values
* Will there be an inheritance graph?
* What explicit/implicit type conversion are allowed
* What operators and member functions make sense
* What standard functions should be **disallowed**?
* Which part is public/protected/private?  Who are `friend`s?
* What is the "undeclared interface"?  (Meaning, guarantees of performance, exception safety, resource usage, etc.)
* How general is it?  (Should you use template instead?)
* Is a new type really needed?
