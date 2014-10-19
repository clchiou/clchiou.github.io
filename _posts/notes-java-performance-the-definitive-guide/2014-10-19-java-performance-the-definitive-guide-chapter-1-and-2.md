---
layout: post
category: notes-java-performance-the-definitive-guide
title: 'Java Performance: The Definitive Guide, Chapter 1 and 2'
---

Death by 1,000 cuts, "Thrashing", Occam's Razor, mesobenchmarks, warm-up period, and think time.

Chapter 1
---------

* Specify the platform you are optimizing for; e.g., Java 7 update 40.
* Show JVM flags with `-XX:+PrintFlagsFinal`.
* Java ergonomics: JVM chooses default flag values based on machine classes; i.e., client vs server.
* Better algorithm and less code almost, including removing dead code, always yields better performance.
* Death by 1,000 cuts: We may win every battles but will ultimately lose the war.
  Chrome releases exhibit this behavior---Performance regresses little by little until a "grand improvement" fixes it, and then it regresses again.
* Don't prematurely optimize, but also don't write bad code that is known to be bad for performance.
* "Thrashing": Throwing more load into an already overburdened system only makes that system worse.
  If you optimize a non-bottleneck sub-system, that sub-system will generate more load for the rest of the system,
  and thus makes overall performance worse.
  (Thrashing is usually not used in this context, but I can't think of a better word for this phenomenon.)
* Occam's Razor: Start investigating a performance issue from simpler hypothesis (is database slow?)
  and eventually move to complex one (is it a bug in JVM?).
* Optimize for the common case.

Chapter 2
---------

* Test a real application: There is a continuum from microbenchmarks to macrobenchmarks (mesobenchmarks).
* Microbenchmarks are usually unhelpful.
* JIT optimizer may cause microbenchmark results useless; so be careful when writing a microbenchmark.
* Reserve warm-up period for class loader, JIT, memory cache, etc.
* Measurements:
  * Elapsed time: Time required to perform a certain task, say, sort 1,000 integers.
  * Throughput: Measured by requests per second. Use a *zero think time* when measure this.
    Be careful that you might actually measureing the benchmark tool's throughput.
  * Response time: Use a *non-zero think time* (usually a constant time?) when measuring this.
* Give both average and 90% quantile (or 99% quantile if you want) when reporting benchmark results.
* Use load generator: faban, etc.
* Use Student's t-test.
* Insignificant (inconclusive) but important result vs significant but trivial result---focus on the former.
* Test early, test often, automate everything, measure everything, and run on the target system.
