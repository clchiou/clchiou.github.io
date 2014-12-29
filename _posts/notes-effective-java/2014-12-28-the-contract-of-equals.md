---
layout: post
category: notes-effective-java
title: The Contract of equals
---

If you need logical equality (instead of object identity),
override `equals()`,
and obey the contract when overriding it
(think of **value classes**).

Given non-null references `x`, `y`, and `z`, the contract includes:

* _Reflexive_: `x.equals(x)`.
* _Symmetric_: `x.equals(y) == y.equals(x)`.
* _Transitive_: `x.equals(y) && y.equals(z) == x.equals(z)`.
* _Consistent_: Multiple calls of `x.equals(y)` should yield the same result.
* _Non-Nullity_: `x.equals(null) == false`.

And you should always also override `hashCode()` when overriding `equals()`.

**Nevertheless, it is not always possible to obey both
the contract and object-oriented principles while extending a class.**

{% highlight java %}
public class Point {
  private final int x;
  private final int y;

  @Override
  public boolean equals(Object o) {
    if (!(o instanceof Point)) {
      return false;
    }
    Point p = (Point) o;
    return x == p.x && y == p.y;
  }

  ...
}

public class ColorPoint extend Point {
  private final Color color;

  // Violate symmetry.
  @Override
  public boolean equals(Object o) {
    if (!(o instanceof ColorPoint)) {
      return false;
    }
    return super.equals(o) && color == ((ColorPoint) o).color;
  }

  // Violate Liskov substitution principle.
  @Override
  public boolean equals(Object o) {
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    Point p = (Point) o;
    return x == p.x && y == p.y;
  }

  ...
}
{% endhighlight %}

(So think twice when you want to make a "trivial" extension; use composition instead.)
