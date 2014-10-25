---
layout: post
title: enum since Python 3.4 and Java 1.5
---

Using `enum` in both languages is usually preferred than a group of named `int` constants.

Python 3.4 adds the `enum` module that provides very similar feature set with Java `enum` type added in Java 1.5.
Python's `enum` makes heavy use of metaclass machinery
whereas Java's `enum` is more or less syntactic sugar.
They both offer you:

* Defining classes with a finite set of members.
* Referring to members by name (and Python allows you to refer to members by value as well).
* Iterating over members.
* Members are unique and can be compared by identity.
* Members are unordered.
* You are disallowed to compare members with integers.

Java `enum` cannot be extended
because every `enum` type implicitly extends `java.lang.Enum` and Java dose not support multiple inheritance.

{% highlight java %}
// This is illegal.
enum MyEnumType extends Whatever { ... }
{% endhighlight %}

In Python,
you may add members to a sub-`enum`
if the parent does not define any members:

{% highlight python %}
class Color(Enum):
    red = 10
    green = 11
    blue = 12

# TypeError: Cannot extend enumerations
class MoreColor(Color):
    black = 0
    white = 1
{% endhighlight %}

It is sometimes a very useful trick to extend a `enum`:

{% highlight python %}
# This is how enum module implements IntEnum.
class IntEnum(int, Enum):
    pass
{% endhighlight %}
