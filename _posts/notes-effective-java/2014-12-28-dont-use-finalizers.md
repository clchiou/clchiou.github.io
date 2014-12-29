---
layout: post
category: notes-effective-java
title: "Don't Use Finalizers"
---

It is almost certainly wrong to use finalizers.

Finalizers...

* are not promptly executed,
* can arbitrarily delay reclamation (and result in `OutOfMemoryError`),
* are not guaranteed to be executed,
* ignore exceptions during finalization
  (and leave corrupted object **not** reclaimed),
* add severe performance penalty.

The bottom line is, don't use finalizers;
use an explicit `close()` method instead.
You should maintain a `isClosed` boolean state
(and usually throw `IllegalStateException` on multiple `close()` calls).

The probably only legitimate use of finalizer is when you have native peers
(JVM does not know how to release these native resources).
However, if the native resources must be released promptly,
you should provide an explicit `close()` method.

Note: Don't forget to call superclass' finalizer unconditionally:
{% highlight java %}
@Override
protected void finalize() throws Throwable {
  try {
    ... // Finalize subclass state.
  } finally {
    super.finalize();
  }
}
{% endhighlight %}

...or use finalizer guardian.
