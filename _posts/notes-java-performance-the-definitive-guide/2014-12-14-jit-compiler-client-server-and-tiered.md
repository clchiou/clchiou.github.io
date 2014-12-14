---
layout: post
category: notes-java-performance-the-definitive-guide
title: 'JIT Compiler: Client, Server, and Tiered'
---

Hot spot compilation.

* Choose compiler: `-client`, `-server`, or `-XX:+TieredCompilation`.
  - Client: Compile sooner, faster startup time.
  - Server: Longer to warm up the compiler, better in the long run.
  - Tiered: Mix of both, reasonable default choice.
  - Show the compiler used with `java -version`.
* 32-bit client `-client`, 32-bit server `-server`, 64-bit server `-d64` (Solaris).
  - In Linux, you have to install 32-bit JVM separately.
* 64-bit vs 32-bit:
  - Larger address space.
  - Larger memory footprint---more bytes in object references.
  - More overhead to manipulate 64-bit references.
  - If a program fits in 32-bit address space, use 32-bit JVM.
* Code cache size: `-XX:ReservedCodeCacheSize=N`.
* Escape analysis (the most sophisticated of the optimization).
  - This may introduce "bug" into improperly synchronized code.
* Deoptimization.
  - Previous optimization is invalid.
  - Reclaim code cache space.
  - Tiered compilation.
