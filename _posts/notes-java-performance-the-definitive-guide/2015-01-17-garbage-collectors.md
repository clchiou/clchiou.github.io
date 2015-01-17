---
layout: post
category: notes-java-performance-the-definitive-guide
title: Garbage Collectors
---

GC tuning is probably the second most important thing after tuning your algorithm.

GC performance is dominated by three basic operations:

* Find unused objects.
* Make memory available.
* Compact the heap.

Normal workload might be interrupted when GC is moving objects around
(i.e., doing above three operations).
It is called a stop-the-world pause.
Minimizing these pauses is the key consideration when tuning GC.

All GCs are generational (splitting the heap into generations):

* Old/tenured generation.
* Young generation (smaller portion of the heap).
  - Eden space (most of the young generation).
  - Survivor space.

**Minor GC** is when GC is processing the eden space.
Unused objects are discarded and live objects are moved to either
the survivor space (and thus the young generation is compacted) or the old generation.
Application threads are stopped during a minor GC.
Minor GCs are fast (good) and frequent (bad) because the young generation is smaller.
You generally prefer frequent small pauses than rare long pauses.

**Full GC** is when GC is processing the old generation (long pauses).
Simple GCs stop application threads, but complex GCs (CMS and G1) don't.

JVM implements four garbage collectors:

* The serial collector (used for single-CPU machines).
  - The only single-threaded GC.
  - Stop the world during both minor and full GCs.
  - Fully compact the old generation during full GCs.
* The throughput (parallel) collector.
  - Stop the world during both minor and full GCs.
  - Fully compact the old generation during full GCs.
* The concurrent (CMS) collector.
  - Stop the world during only minor GCs.
  - Process the old generation in background.
  - It needs more CPU for background GC thread(s) than throughput collector.
  - Don't compact the old generation (and revert back to serial collector when the heap becomes too fragmented).
* The G1 (Garbage First) collector.
  - Designed for large heaps (greater than 4 GB).
  - Process the old generation in background.
  - It needs more CPU for background GC thread(s) than throughput collector.
  - It divides the heap into multiple regions.
    When copying live objects to another region (in background GC threads), it also compacts the target region.

Choosing GC:

* Do you have extra CPU for background GC threads (of CMS and G1)?
* Do you care response time or throughput?
* Do you care average response time or 90th% or 99th% response time?
