---
layout: post
category: notes-effective-c++
title: "Item 46: Define Non-Member Functions Inside Templates When Type Conversions Are Desired"
---

Recall: [Item 24](../2016-12-10/item-24-declare-non-member-functions-when-type-conversions-should-apply-to-all-parameters)
tells that you should define non-member function when type conversions are desired.
In templatized version, you have to define **non-member** function **inside** a class.

Because implicit type conversion functions are **never** considered during template argument deduction.
(Template argument deduction happens before compiler handles type conversions.)
A workaround is to define a friend function inside a template class
(template argument deduction only applies to function templates, not class template).

```c++
template<typename T>
class Rational {
 public:
  // Declare as a friend.
  // Here, operator* is just a function that lives in
  // a class template, not a function template; so the
  // usual type conversion rules apply.
  // (By the way, instead of writing Rational<T>, you
  // may omit T here.)
  friend const Rational operator*(
    const Rational& lhs, const Rational& rhs);
}

// (But you may not omit T here.)
template<typename T>
const Rational<T> operator*(
    const Rational<T>& lhs, const Rational<T>& rhs) {
  ...
}
```

(Honestly, I don't really understand this part of C++ syntax rules.)
