---
layout: post
category: uva-problem-set
title: 124 - Following Orders
---

Given a partial order, this
[problem](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=3&page=show_problem&problem=60)
asks you to enumerate all orderings of variables that satisfy the partial order.

Before we outline an algorithm solves this problem, let us define the terms:

* At any given stage, a variable is either *emitted* or *not emitted*;
  emitted variables are put in the output list.
* A constraint \\(x < y\\) is said to *bind* \\(y\\).
* A variable \\(y\\) is *free* if it is not bound by any constraints,
  or for all constraints \\(x < y\\) that bind it, \\(x\\) is emitted;
  otherwise \\(y\\) is *bound*.

We solve this problem recursively.
Each node \\(n\\) of the recursive tree is a pair of lists \\((F_n, O_n)\\)
where \\(F_n\\) is the list of free variables,
and \\(O_n\\) is the list of output variables.
At the root node,
the free-variable list are variables not bound by any constraints,
and output-variable list is empty.
For each node \\(n\\), it has \\(|F_n|\\) child nodes;
each child node is constructed by emitting one of the free variable of node \\(n\\).
Note that when a variable is emitted, one or more variables could become free,
and join other free variables of the child.

A reference solution is
[here](https://github.com/clchiou/uva-problem-set/blob/master/solved/124/124.cc).
