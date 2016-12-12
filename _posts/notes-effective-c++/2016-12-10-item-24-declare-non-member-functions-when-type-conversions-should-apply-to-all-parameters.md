---
layout: post
category: notes-effective-c++
title: "Item 24: Declare Non-Member Functions When Type Conversions Should Apply to All Parameters"
---

If you need type conversion on all parameters (including `this`), that function must be non-member.

For example, to implement `Rational` that supports:

```c++
Rational x, y, z;
y = x * 2;
z = 4 * y;
```

You need:

* Non-explicit constructor

```c++
Rational(int numerator = 0, int denominator = 1);
```

* Type conversion on **all** parameters (including `this`)

```c++
const Rational operator*(const Rational& lhs, const Rational& rhs);
```

* That is because **only** parameters are eligible for implicit type conversion

* Note: Templates follow a different rule
