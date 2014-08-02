---
layout: post
category: uva-problem-set
title: 129 - Krypton Factor
---

This
[problem](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=65)
asks you to generate the _i_-th string of a list of strings (in alphabetical order) that do not contain identical, adjacent substrings.

I could not derive an algorithm that generate the _i_-th string directly.
Instead, I enumerated all strings until the _i_-th string (fortunately this did not yield a TLE).
A reference solution is
[here](https://github.com/clchiou/uva-problem-set/blob/master/solved/129/129.cc).
