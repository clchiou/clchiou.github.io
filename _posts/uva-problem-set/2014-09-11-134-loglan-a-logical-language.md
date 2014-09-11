---
layout: post
category: uva-problem-set
title: 134 - Loglan-A Logical Language
---

Write a parser to solve this [problem](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=70).

There are may choices of parser family: LL, LALR, SLR, etc.
I rewrote the grammar slightly and
[implemented](https://github.com/clchiou/uva-problem-set/blob/master/solved/134/134.cc)
a simple (but inefficient) back-tracing parser.
Keep in mind that although the problem states:
"You can assume that all words will be correctly formed."
This statement is not 100% right.
Input data have illegal strings which should be treated as Loglan names.
