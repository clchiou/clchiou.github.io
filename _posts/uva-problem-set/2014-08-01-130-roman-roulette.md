---
layout: post
category: uva-problem-set
title: 130 - Roman Roulette
---

This
[problem](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=66)
ask you to simulate Roman Roulette.

At first I simulated Roman Roulette by actually inserting/deleting elements of a cyclic queue.
This turned out to be a bad idea: It made my code complicated due to C++ standard container's iterator invalidation rules.
I decided to
[solve](https://github.com/clchiou/uva-problem-set/blob/master/solved/130/130.cc)
this by marking dead people with -1.
With hindsight, I probably should write my cyclic queue that does not invalidate iterators.
