---
layout: post
category: uva-problem-set
title: 106 - Fermat vs. Pythagoras
---

This
[problem](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=42)
asks you to generate all primitive Pythagorean triples less than an upper bound.

I use
[Euclid's formula](http://en.wikipedia.org/wiki/Pythagorean_triplet#Generating_a_triple)
to generate all primitive Pythagorean triples.
To do so, I generate all coprime pairs $(m, n)$ with one of them is an even number.
If you use Euclid's formula,
remember that you also need a boundary condition of $(m, n)$ ---
I mean when to stop generating them.
[Here](https://github.com/clchiou/uva-problem-set/blob/master/solved/106/106.cc)
is a reference solution.
