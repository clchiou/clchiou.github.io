---
layout: post
title: Open-And-Init Anti-Pattern
---

Open and initialize a resource in one method may leak the resource.

I have seen this anti-pattern common enough,
and yet I could not find a name of it.
So I took the liberty of naming it Open-And-Init anti-pattern.

The Open-And-Init anti-pattern is quite simple:
In a method, you open and initialize a resource,
and then you return it to the caller.
Doing so you risk leaking the resource
whenever the initialization code path throws an exception.
Whether the caller manages the resource with
a `try-finally` block or `try`-with-resources statement is irrelevant
since the caller does not even have a chance to get the reference of the resource.

Let us take the following codes for example.
You have a `Connection` class, and you think it would be convenient
(and reduce code duplication) to open and initialize a `Connection` object
in one method.
The caller `someMethod` employs a `try`-with-resources statement to close
the `Connection` object after use.
Everything looks great,
except that you are doomed to leak a `Connection` object
whenever `conn.init()` throws an exception.

{% highlight java %}
// DONT DO THIS: Open-And-Init is an anti-pattern.
public Connection openAndInit() {
    Connection conn = Connection.open();
    conn.init();
    return conn;
}

public void someMethod() {
    try (Connection conn = openAndInit()) {
        // Use the conn object...
    }
}
{% endhighlight %}

You might think of wrapping around `conn.init()` with a `try-catch` block:

{% highlight java %}
// Is it yet another anti-pattern?
public Connection openAndInit() {
    Connection conn = Connection.open();
    try {
        conn.init();
    } catch (Exception e) {
        conn.close();
        throw e;
    }
    return conn;
}
{% endhighlight %}

This approach solves the issue, but I would argue that
excessive use of it is an anti-pattern of its own.
You and I should agree now that
it is better to write open and initialize in separated methods.
Wait, not so fast!
Is it possible to write an exception safe Open-And-Init method
without resorting to wrapping codes around with a `try-catch` block?
Actually, yes, but not in Java as I could think of.
In C++, you may write Open-And-Init as follows:

{% highlight c++ %}
std::unique_ptr<Connection> OpenAndInit() {
  std::unique_ptr<Connection> conn(new Connection());
  conn->init();
  return conn;
}
{% endhighlight %}

`std::unique_ptr` releases the `Connection` object,
should an exception be raised in `init()`---an
advantage of a definitive order of object destruction
that C++ enjoys.
Meanwhile, we will have to keep writing open and initialize
in separated methods in Java.
