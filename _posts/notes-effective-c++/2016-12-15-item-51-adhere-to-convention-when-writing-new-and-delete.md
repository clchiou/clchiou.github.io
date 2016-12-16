---
layout: post
category: notes-effective-c++
title: "Item 51: Adhere to Convention When Writing new and delete"
---

Here are the conventions (tricker than you thought):

To write a conformant `operator new` at least you have to:

* Return the right value (duh!)
* Call thew `new`-handler function when space unavailable
* Cope when no memory
* Avoid hiding the "normal" `new`
* Return a legitimate pointer even when the requested size is zero

To write a confromant `operator delete` at least you have to:

* Always safe to delete null pointer

Note:

* `operator new` will be inherited be derived classes - which might not be what you want (especially when you are optimizing the allocator for one, fixed size)
* You cannot assume the size parameter passed to `operator new[]` equals to `sizeof(Base) * num_elements` because:
  * It could be called from derived class
  * There could be extra space for recording the number of elements
