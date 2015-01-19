---
layout: post
title: Notes on Python asyncio
---

You choose to live with the extra complexity of asynchronous I/O (aka event-driven)
almost always only for scaling up the number of concurrent connections.

Python 3 provides an [`asyncio` package](https://docs.python.org/3/library/asyncio.html) on a provisional basis.
It has a pluggable event loop and separates transport from protocol.
Python's `yield from` makes asynchronous I/O moderately easier to code
(but you will be punished for omitting `yield from` where you shouldn't).

Question: Is it possible (i.e., easier than a rewrite)
for a codebase to start with synchronous I/O and
later switch to asynchronous I/O when it hits scaling issues?
(I guess no.)
