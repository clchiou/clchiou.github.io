---
layout: post
category: notes-effective-java
title: Constant Interface Anti-Pattern
---

Don't do this and don't implement it for your class.

{% highlight java %}
// Don't do this.
public interface Constants {
    public static final int SOME_CONSTANT = 1;
}

// Don't implement it.
public class YourClass implements Constants { ... }
{% endhighlight %}

It is bad because:

* A public constant is a part of public API and a (serious) commitment.
* Implementing random interfaces pollutes your class' and sub-classes' name space
  (use static import instead).
* In general, interface should carry an intent or information of type hierarchy.
