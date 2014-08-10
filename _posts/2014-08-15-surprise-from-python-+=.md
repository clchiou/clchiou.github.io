---
layout: post
title: Surprise from Python +=
---

Thanks to the augmented arithmetic assignments
[methods](https://docs.python.org/3/reference/datamodel.html#object.__iadd__),
`x += y` is not equivalent to `x = x + y`.

First of all, some backgrounds.
Conceptually a Python variable is different from a C variable:
While a C variable is just a name of a memory location, which you store values,
a Python variable is actually a name of a value (we say the variable is bound to that value).
So in C, `=` is an _assignment_ in the sense that you copy the value to the memory location of the variable.
On the contrary, in Python, `=` is a _re-binding_ in the sense that you bind the variable to a new value.
In other words, a Python variable is like a C pointer,
and "assign to" a Python variable is like making it point to another memory location.
(Note that in C, a variable occupies a memory location, but in Python, it is value that occupies a memory location.)

Let's put the idea into practice:

{% highlight python %}
# x, y, and z are bound to the same value, i.e.,
# id(x) == id(y) == id(z).
x = y = z = '0'

# `y + '1'` evaluates to a new value '01', and
# then we bind y to the new value.  From now on,
# id(x) != id(y).
y = y + '1'

# Because str does not define __iadd__, this is
# equivalent to y = y + '1'.
z += '2'
{% endhighlight %}

Here comes the surprise from `+=`:

{% highlight python %}
# We start with id(x) == id(y) == id(z).
x = y = z = [0]

# `y + [1]` evaluates to a new value `[0, 1]`,
# and then we bind y to the new value.  From
# now on, id(x) != id(y).
y = y + [1]

# This invokes list.__iadd__, which modifies
# the list in-place.  Furthermore, this does
# not bind z to a new value (since no new
# value is created).  So id(x) == id(z).
z += [2]

# At this point, we have
#   x == z == [0, 2]
# and
#   y == [0, 1]
{% endhighlight %}

So be careful about the surprise from `+=`.
