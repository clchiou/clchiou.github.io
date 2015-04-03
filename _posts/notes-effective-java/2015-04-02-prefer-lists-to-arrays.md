---
layout: post
category: notes-effective-java
title: Prefer Lists to Arrays
---

Lists are invariant whereas arrays are covariant; prefer lists to arrays.
(You can't use generics with arrays, by the way.)

* Covariant: `Sub[]` is a subtype of `Super[]`.
* Invariant: `List<Sub>` is neither a sub- nor super- type of `Super[]`.

{% highlight java %}
// Covariant is bad!
Object[] array = new Long[1];
array[0] = "Boom!";  // Throw ArrayStoreException.
{% endhighlight %}

Also, because arrays are covariant,
you are not allowed to create generic arrays
(otherwise you may get rid off the generic type information from a generic array by assigning it to a `Object[]`).
As a result, generics also doesn't work with `varargs`.

If you absolute have to use arrays in a generic class (say, you are implementing a `Stack<T>`),
use casting and suppress warnings instead.
