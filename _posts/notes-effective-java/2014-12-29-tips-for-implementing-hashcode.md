---
layout: post
category: notes-effective-java
title: Tips for Implementing hashCode
---

You must override `hashCode` in every class that overrides
[`equals`](/notes-effective-java/2014-12-28/the-contract-of-equals/).

Tips for converting primitive types to `int`:

* `boolean`: `v ? 1 : 0`.
* `byte`, `char`, or `short`: `(int) v`.
* `long`: `(int) (v ^ (v >>> 32))`.
* `float`: `Float.floatToIntBits(v)`.
* `double`: `Double.doubleToLongBits(v)` and then convert the result as above.
* `null`: Usually use `0` here.
* Object reference:
  - If `equals` is defined as a recursive function, compute `hashCode` recursively, too.
  - Otherwise, compute a "canonical representation" and then invoke `hashCode` on it.
* Array: Treat it as if each element was a separate member field.

An okay hash function:
{% highlight java %}
// Initial value of code is usually non-zero, say 17.
code = 31 * code + hashValue;
{% endhighlight %}

Write unit tests to verify that equal instances have equal hash codes.

You may cache the hash code of immutable objects.
