---
layout: post
category: notes-effective-c++
title: "Item 49: Understand the Behavior of the new-Handler"
---

`operator new` will **repeatedly(!)** call `new`-handler until it can find enough space.

An `new`-handler could do one of the following:

* Make more space available
* Install a different `new`-handler
* Deinstall the `new`-handler (`operator new` will throw an exception when there is no `new`-handler)
* Not return

Nice trick: Use templatized base class with static data member so that each derived class has different instantiated base class and **its own static data member**
(this is called **curiously recurring template pattern (CRTP)**).

(By the way, what if memory must be dynamically allocated when you are writing error message to `cerr`...)

History: Until 1993 C++ requires `operator new` returns null when it can't allocate space,
but now it should throw `bad_alloc` exception.
To preserve this legacy, C++ provides a nothrow form `new`:

```c++
new (std::nothrow) Foo;
```

But note that it doesn't mean it provides the nothrow exception guarantee,
it only means the allocation part (`operator new`) doesn't throw,
but doesn't mean the constructor won't throw.
