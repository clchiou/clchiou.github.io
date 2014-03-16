---
layout: post
category: notes-effective-java
title: Overloading Methods with Varargs
---

Don&rsquo;t do it. It is a bad idea.

While item 41 "Use overloading judiciously" and item 42 "Use varargs judiciously"
of Effective Java should have warned you about this issue,
I stumbled upon how confusing the combination of two could be.
Consider this program:

{% highlight java %}
public class Overload {
    public void method(Parent obj) {
        System.out.println("method(Parent obj)");
    }

    public void method(Child obj) {
        System.out.println("method(Child obj)");
    }

    public static void main(String[] args) {
        Overload overload = new Overload();
        overload.method(new Parent());
        overload.method(new Child());
    }
}
{% endhighlight %}

Given that `Parent` is the super class of `Child`,
this program will print

    method(Parent obj)
    method(Child obj)

The overload resolution prefers specialization over generalization.
Intuitive, right?

What if we add varargs to the second overloaded method?

{% highlight java %}
public class Overload {
    public void method(Parent obj) {
        System.out.println("method(Parent obj)");
    }

    public void method(Child obj, String... extras) {
        System.out.println("method(Child obj)");
    }

    public static void main(String[] args) {
        Overload overload = new Overload();
        overload.method(new Parent());
        overload.method(new Child());
    }
}
{% endhighlight %}

This time it will print

    method(Parent obj)
    method(Parent obj)

Counter-intuitive?
[JLS 15.12.2](http://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.12.2)
states clearly that compiler performs overload resolution while allowing varargs in the last phase.
So the method invocation
{% highlight java %}
    overload.method(new Child());
{% endhighlight %}
will be resolved to
{% highlight java %}
    public void method(Parent obj)
{% endhighlight %}
before
{% highlight java %}
    public void method(Child obj, String... extras)
{% endhighlight %}
even gets considered by compiler.
If you want to call the second overloaded method,
you have to make it explicit at the caller site
{% highlight java %}
    overload.method(new Child(), (String[])null);
{% endhighlight %}
Now you have to hunt down every client of your class and make this change.
So it turns out that the seemingly harmless method signature change
is effectively a backward-incompatible API change.

This problem happened in a class maintained by multiple people,
each making patches to the class.
Although their changes looked fine separately,
when their changes were finally all merged,
the problem emerged.
The nasty aspect of this problem is that it is context-dependent.
Now a patch is not guilty only if we also examine all other patches.
In a team, this kind of problem will prevent it from scaling.

The bottom line is, no matter how innocent overloaded or varargs methods might look,
add them to your classes judiciously.
