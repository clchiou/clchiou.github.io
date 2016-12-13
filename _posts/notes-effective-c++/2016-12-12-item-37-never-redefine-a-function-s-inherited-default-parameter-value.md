---
layout: post
category: notes-effective-c++
title: "Item 37: Never Redefine a Function's Inherited Default Parameter Value"
---

Because default parameter values are **statically** bound, **even for virtual functions** (surprise!).

* Meaning, you may invoke a virtual function defined in derived class with default parameter value from a base class.

So if you really really want to use default parameter for a virtual function, consider use NVI as an alternative:

* Define a non-virtual public function with default parameter
* Define a virtual private function without default parameter
* The public function calls the private function, passing the default parameter value
  * Because you should not redefine non-virtual function, the default parameter is not redefined
  * Because the (private) virtual function does not have default parameter, you don't have to worry about redefining default parameter when you redefine the virtual function
  * So everyone is happy!
