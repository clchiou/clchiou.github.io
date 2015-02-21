---
layout: post
title: Python "Smart" Module
---

How to make a "smart" module that its attributes are generated during runtime?

I found this trick in
[`plumbum`](https://github.com/tomerfiliba/plumbum/blob/master/plumbum/__init__.py)
for just that.
`plumbum.cmd` is a dynamically generated module that
its attributes are executable programs found from your `$PATH`.
And it is inserted into `sys.modules` and so you may import it as if it is a normal module.
I am not aware of (bitten by) any bad side effect of this trick yet.

By the way, you may actually insert any object, not just of `ModuleType`, into `sys.modules`,
but I am not sure the consequences though.
