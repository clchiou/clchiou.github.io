---
layout: post
title: Java Functional Programming
---

Java 8 introduces lambda expression,
allowing non-verbose, no-memory-leak functional programming.

[Guava](https://code.google.com/p/guava-libraries/wiki/FunctionalExplained)
documentation warns you about the verboseness of using anonymous classes to approximate functional programming.
Anonymous classes are also often guilty of unnecessary object retention (memory leak)
because they **always** hold a strong reference to their enclosing instance.

Java 8 lambda expression is unlikely to suffer from these problems
([quote:](http://cr.openjdk.java.net/~briangoetz/lambda/lambda-state-final.html)
"while inner class instances always hold a strong reference to their enclosing instance,
lambdas that do not capture members from the enclosing instance do not hold a reference to it.").

Additionally, lambda expression consumes less memory than anonymous class
because it is implemented with
[`invokedynamic`](http://docs.oracle.com/javase/7/docs/technotes/guides/vm/multiple-language-support.html#invokedynamic)
introduced since Java 7.
And lambda expression is comparable in performance to anonymous class
(but JVM seems to need longer time to warm up lambda expression?).

[Here](http://www.infoq.com/articles/Java-8-Lambdas-A-Peek-Under-the-Hood)
is a good explanation of lambda under the hood.
