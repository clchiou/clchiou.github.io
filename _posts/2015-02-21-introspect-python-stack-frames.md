---
layout: post
title: Introspect Python Stack Frames
---

Python is great at introspecting itself.
But could a function introspectively know its caller?
Answer: Yes, at least for CPython.

`inspect.currentframe()` will do just that,
which simply calls `sys._getframe(1)`.
`sys._getframe` take an optional integer argument `i`,
and returns the `i`-th stack frame from the top.
(So `0`-th is yourself, `1`-th is your caller, and so on.)

Nevertheless, `sys._getframe` is a CPython implementation detail,
and should not be expected to be supported in all Python implementations.
`logging` module uses `sys._getframe` for finding caller function's name,
and it has a comment warning you that
in IronPython `sys._getframe` might return `None`.
In that case, `logging` uses a trick for retrieving a stack frame:
It throws an exception and then immediately catches it,
and the traceback object will include a stack frame.
