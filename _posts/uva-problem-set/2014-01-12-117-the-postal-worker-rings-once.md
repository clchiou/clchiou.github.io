---
layout: post
category: uva-problem-set
title: 117 - The Postal Worker Rings Once
---

This
[problem](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=3&page=show_problem&problem=53)
asks you to find the weight of the shortest cycle that visits all edges at least once.

The problem states that there will be *at most two* nodes have odd degree,
which should remind you about Euler path.
Having this in mind,
if none of the nodes has odd degree in the input,
an Euler *cycle* exists,
and the output is simply the sum of weights.
Otherwise, there are two nodes with odd degree in the input,
an Euler *path* exists.
We may construct a shortest cycle that visits all edges at least once by first following the Euler path,
which starts at one odd-degree node and ends at the other,
and then following the shortest path that gets us back to the starting node.
A reference solution is
[here](https://github.com/clchiou/uva-problem-set/blob/master/solved/117/117.cc).
