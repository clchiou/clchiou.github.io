---
layout: post
category: notes-effective-c++
title: Stick to a Clearly Specified Subset of C++
---

It is often considered a good project management practice that
only a clearly specified subset of C++ is allowed in the codebase.

As Item 1 of the Effective C++ states, C++ is a federation of languages.
A related, well-known statement is that C++ is enormously complicated;
practically no one can master the entire federation.
So we all code a (different) subset of C++.
The point is, a team should stick to only use a clearly specified subset of C++,
a common denominator, that present and future members may grasp.

This all sounds good; however, C++ features are intertwined and
sometimes force you to use either all or none of them.
Take RAII (Resource Acquisition Is Initialization) for example.
Since the only practical way in C++ to signal an error in constructor is through exception,
if you want RAII, you are required to use exception, too.
This is not an easy choice.
[Google](http://google-styleguide.googlecode.com/svn/trunk/cppguide.html)
chooses not to use both RAII and exception on the ground that
writing exception-safe code is exceptionally hard,
especially when they have an existing non-exception-safe codebase.

Also, it is hard to draw a clear borderline.
We all agree that excessive template meta-programming is unmaintainable,
but how much template witchcraft is considered excessive?

I don't have a good guideline of choosing a C++ subset.
