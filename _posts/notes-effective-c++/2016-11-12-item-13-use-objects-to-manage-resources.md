---
layout: post
category: notes-effective-c++
title: "Item 13: Use Objects to Manage Resources"
---

Just use RAII - and don't let exception leave destructor.

* The newly acquired resource should be **immediately** turned over to a resource-management object - so that it won't leak.

* `auto_ptr` and `shared_ptr` doesn't call `delete[]`, meaning, that **they cannot manage array objects**.  (But the code will compile.  So if you are not careful and turns an array over to a smarter pointer, your program will have undefined behavior.)

* Like Java, modern C++ should generally avoid using array because array doesn't mix well with the modern C++ features.

Note: Effective C++ 3rd ed is not up-to-date to C++11 `std::unique_ptr` that supersedes `auto_ptr`.
