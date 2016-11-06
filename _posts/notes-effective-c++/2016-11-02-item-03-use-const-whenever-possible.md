---
layout: post
category: notes-effective-c++
title: "Item 3: Use const Whenever Possible"
---

You may add `const` to almost anything.

* "STL iterators are modeled on pointers."
* `const iterator` is like `T * const` and `const_iterator` is like `const T *`.

* `operator*` should return const.
{% highlight c++ %}
const Rational operator*(const Rational& lhs, const Rational& rhs);
// This helps compiler catch bug like:
if (a * b = c) { ... }
{% endhighlight %}

* "Member function differing **only** in their constness can be overloaded."
{% highlight c++ %}
// Note: You should not return a writable reference
// from a const member function.
const char& operator[](std::size_t position) const { ... }
char& operator[](std::size_t position) { ... }
{% endhighlight %}

* const member functions: Compiler checks **bitwise constness** but you usually want **logical constness**.
* Logical constness: You may modify the object, but "only in ways that clients cannot detect" (like caching stuff internally).

* You may implement non-const version from const member function using `static_cast` and `const_cast` (but not const-version from non-const member function).  This is not pretty but better than duplicating code (especially when the member function is not just one-liner).
{% highlight c++ %}
return const_cast<char&>(static_cast<const Foo&>(*this)[position]);
{% endhighlight %}
