---
layout: post
category: notes-effective-c++
title: "Item 30: Understand the Ins and Outs of Inlining"
---

`inline` is a request to compilers, not a command.
Defining a function inside a class definition is an implicit request.

Most build environment do inlining during compilation (except maybe .NET CLI, which may perform inlining during linking).
This is why inline functions must typically be in header files.

You can't inline a virtual function (because virtual function lookup is, by definition, performed at runtime).

If you take a (inline) function's address, compiler must generate that function out-of-line (this shouldn't surprise you).

Inline constructors and destructors are sometimes generated out-of-line because compiler needs a pointer to them to construct arrays.
Also, constructors and destructors are bad candidates for inlining (because they are bigger than their appearance, like calling base class' constructor or unwinding already-constructed members when an exception is raised).
