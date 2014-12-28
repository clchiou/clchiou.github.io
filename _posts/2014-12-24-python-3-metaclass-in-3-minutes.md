---
layout: post
title: Python 3 Metaclass in 3 Minutes
---

Python metaclass is a mechanism for
[customizing class creation](https://docs.python.org/3.3/reference/datamodel.html#customizing-class-creation).

The default class creation is by calling:
{% highlight python %}
type(name, bases, namespace)
{% endhighlight %}

You may a customize a class creation by:
{% highlight python %}
class SomeClass(metaclass=metaclass, **kwds): pass
{% endhighlight %}
...or by inheriting from a metaclass-created class.

Basically this is how a class is created:

1. An appropriate metaclass is determined.
   (If it is not unique, a `TypeError` is thrown.)

2. If you implement `__prepare__`,
   a dict-like object (usually called `namespace`) is created by:

   ```
   metaclass.__prepare__(name, bases, **kwds)
   ```

   Note: `__prepare__` is usually a class method (less often a static method).

3. The class body is executed approximately as:

   ```
   exec(body, globals(), namespace)
   ```

4. The class object is created by calling:

   ```
   metaclass(name, bases, namespace, **kwds)
   ```

   - The metaclass is usually inherited from `type`,
     and you usually implement `__new__` (less often `__init__`).
   - (As a matter of fact, `metaclass` can be any callable object.)

5. The class object is passed to any class decorators.

By the way, `__new__()` is a static method
(special-cased so that you need not to declare it as such).
