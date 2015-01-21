---
layout: post
category: notes-java-performance-the-definitive-guide
title: Garbage Collectors, Part 2
---

(The default settings are usually good enough?)

Some tips:

* Understand the trade-off among parameters (heap sizes, etc.)
* Understand the characteristics of a GC algorithm.
* Try adaptive tuning first (you set a goal and JVM adjusts parameters to meet the goal).

Ask yourself:

* Are some full GC pauses tolerable?
  - If so, choose the throughput collector.
  - If not, choose a concurrent collector (CMS or G1).
* Do the default settings yield the performance you need?
  - If not, is GC the bottleneck (check GC logs)?
    3% CPU time on GC is a good threshold.
  - Even if GC is not the bottleneck, you still might want to reduce outliers
    (extreme long puases).
* Do the pause times close to your goal?
  - If so, you might also want to adjust maximum pause time.
  - If not, you might trade some throughput for shorter pause times
    by reducing the size of the young generation.
* Do you need higher throughput even though the pause times are short?
  - Increase the size of the heap (or at least the size of the young generation).
* Are full GCs due to concurrent-mode failures?
  - Ask for more GC threads or sooner background sweep.
* Are full GCs due to promotion failures?
  - Fragmentation!  What can we do about it?
