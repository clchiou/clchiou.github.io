---
layout: post
category: notes-effective-java
title: Tips for Implementing Comparable
---

`Comparable` has a contract similar to
[`equals`'s](/notes-effective-java/2014-12-28/the-contract-of-equals/).

* `Comparable.compareTo() == 0` should usually be consistent with `equals`,
  but if it is not, you should explicit comment about this.
* Like `equals`, it is not always possible to obey both
  the contract and object-oriented principles while extending a class.
  Use composition instead.
* **Be very careful about (signed) integer overflow** when you implement `Comparable`.
  The difference between a positive 32-bit signed integer and a negative one
  could be greater than what a 32-bit signed integer can hold.
  (And this bug is very hard to locate.)

{% highlight java %}
public int compareTo(Blob b) {
  // Danger: This might overflow!
  return intValue - b.intValue;
}
{% endhighlight %}
