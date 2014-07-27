---
layout: post
category: uva-problem-set
title: 111 - History Grading
---

This is a very simple dynamic programming
[problem](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=47)
with very puzzling description.

Basically "ranking" is just index.  Given an input string:

    3 1 2 4 9 5 10 6 8 7

It means the first event is at index `3`, the second event is at index `1`, etc.  Let us denote events with letters to avoid confusion.

| Event: | `a` | `b` | `c` | `d` | `e` | `f` | `g`  | `h` | `i` | `j` |
| Index: | `3` | `1` | `2` | `4` | `9` | `5` | `10` | `6` | `8` | `7` |

<p></p>

So the ordering of events is: `b c a d f h j i e g`.

Here is a
[reference solution](https://github.com/clchiou/uva-problem-set/blob/master/solved/111/111.cc).
