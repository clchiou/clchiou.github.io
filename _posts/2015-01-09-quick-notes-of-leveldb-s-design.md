---
layout: post
title: "Quick Notes of LevelDB's Design"
---

[LevelDB](https://github.com/google/leveldb)
is a sorted key-value storage library
that implements
[LSM-Tree](http://en.wikipedia.org/wiki/Log-structured_merge-tree)
algorithm.

Simply put, LSM-Tree (Log-Structured Merge-Tree) is a leveled storage method.
To achieve good read and write performance,
it "caches" data and **log records** in a memory-resident tree (the $C_0$ tree),
and regularly flushes the log records to the disk-resident trees (the $C_1$ tree).
The disk-resident trees form a hierarchy ($C_1, C_2,$ etc.);
when the trees of a lower level are full,
some of them are **merged** into a higher level tree.

LevelDB stores disk-resident trees in SSTable (Sorted-String Table) files,
which are fast to read, but slow to insert or delete.
To offer fast insert and delete,
LevelDB organizes these SSTable files into a LSM-Tree.
