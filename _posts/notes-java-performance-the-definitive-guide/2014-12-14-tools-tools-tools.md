---
layout: post
category: notes-java-performance-the-definitive-guide
title: Tools, Tools, Tools
---

Performance tuning is all about tools.
Tools make things **visibile**---what is going on inside of an application, and in the application's environment.

* CPU utilization is the higher the better (otherwise CPU is idling, waiting for I/O or locks).
* I/O utilization not always the higher the better.
* Network cannot sustain a 100% utilization rate.
* System monitor tools:
  - vmstat, iostat, nicstat (cpu, disk, network).
* Java monitor tools:
  - jcmd, jinfo, ...
* Profiling tools:
  - Sampling: Less overhead, less accurate, each tool differs.
  - Instrumented: More intrusive.
  - Native: For profiling JVM.
  - Java Mission Control tool.
* `-XX:+PrintFlagsFinal`
