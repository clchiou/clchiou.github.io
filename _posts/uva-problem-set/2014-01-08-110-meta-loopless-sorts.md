---
layout: post
category: uva-problem-set
title: 110 - Meta-Loopless Sorts
---

This is a quite challenging recursive function
[problem](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=46),
in which you generate a decision tree of sorting \\(n\\)-variables.

A reference solution is
[here](https://github.com/clchiou/uva-problem-set/blob/master/solved/110/110.cc).
It generates a decision tree that resembles the insertion sort algorithm.
The root of the decision tree is of depth \\(1\\), and leaves are of depth
\\(n\\).  Each non-leaf node \\(u\\) has \\(d + 1\\) child nodes \\(v_i\\)
where \\(d\\) is its depth and \\(i = 0 \dots d\\).  The \\(v_i\\) node
represents an insertion of the \\(d + 1\\)-th variable at position \\(i\\).
