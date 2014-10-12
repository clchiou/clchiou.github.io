---
layout: post
title: Python 2-to-3 Cheat Sheet
---

A short reference of new Python 3 features.

Python 3.4
----------

  * [PEP 446](http://www.python.org/dev/peps/pep-0446): Newly created file descriptors non-inheritable by default.
  * `min(..., default=...)` and `max(..., default=...)`.
  * `asyncio`. Status: Provisional.
  * `enum`.
  * `pathlib`. Status: Provisional.

Python 3.3
----------

  * `yield from` statement:
    * Receive sent and thrown values from the calling scope.
    * Return value to the outer generator.
  * `venv` module and `pyvenv` script.
  * `raise exc from None` to suppress exception context.
  * `collections.ChainMap`.

Python 3.2
----------

  * <del>`optparse`</del> `argparse`.
  * `str.format_map()`.
  * `@functools.lru_cache(maxsize=...)`.
  * `@functools.total_ordering`.
  * `collection.OrderedDict.move_to_end(key, last=True)`.
  * `threading.Barrier`.
  * `io.BytesIO.getbuffer()` returns an editable view of the internal data.
  * `@reprlib.recursive_repr()` for recursive structure.
  * `with subprocess.Popen(...)`.
  * `with tempfile.TemporaryDirectory()`.

Python 3.1
----------

  * `collections.OrderedDict`, which is also used by `configparser`, `collections.namedtuple`, `json`, etc.
  * `collections.Counter`.
  * `format(1234567.89, ',.2f') == '1,234,567.89'`.
  * `n.bit_length()`.
  * Multiple context managers:

{% highlight Python %}
with open('input') as infile, open('output', 'w') as outfile:
{% endhighlight %}

  * `collections.namedtuple(..., rename=False)`.
  * `logging.NullHandler`.
  * `importlib`.

Python 3.0
----------

  * `print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)`.
  * `exec()` is a function.
  * `dict.keys()`, `dict.items()`, and `dict.values()` return views.
  * <del>`dict.iterkeys()` `dict.iteritems()` `dict.itervalues()`</del>
  * `map()` and `filter()` return iterators.
  * `map()` stops when the shortest iterator is exhausted.
  *  <del>`xrange()`</del> `range()`.
  * `zip()` returns an iterator.
  * You cannot make comparison of `None < None`, `1 < ''`, etc.
  * `list.sort()` and `sorted()`: <del>`cmp`</del> `key`.
  * <del>`object.__cmp__()`.</del>
  * <del>`long`</del> `int`.
  * **`1 / 2 == 0.5` and `4 // 2 == 1`.**
  * <del>`sys.maxint`</del> `sys.maxsize` (since "maxint" makes not sense anymore).
  * Octal literal: `0o720`. No `0720` anymore. `oct(16) == '0o20'`.
  * Binary literal: `0b101`. `bin(8) == '0b1000'`.
  * `str` is a sequence of Unicode code points.
  * `str(byte_string, encoding=...)` vs `bytes(text_string, encoding=...)`.
  * `'Unicode string literal'` vs `b'byte string literal'`.
  * <del>`basestring`</del> (since there is not `unicode` and `str` now).
  * **Remember to pass `encoding` to `open(..., encoding=None, ...)` when opening in text mode.**
  * **`sys.stdin` et al. are now Unicode-only text files (`io.TextIOBase`). Use `io.TextIOBase.bufer` for byte data.**
  * **File names and paths are now Unicode strings, but:**
    * **Some APIs accept byte strings, too.**
    * **Some APIs return byte strings when passing in byte strings, too.**
  * **`os.environ`, `sys.argv`, etc. might not be interpretable using the current encoding.**
  * <del>`StringIO`</del> <del>`cStringIO`</del> `io.StringIO` and `io.BytesIO`.
  * [PEP 3107](http://legacy.python.org/dev/peps/pep-3107/): Argument and return value annotations.

{% highlight Python %}
# func.__annotations__ == {'return': 'world', 'arg': 'hello'}
def func(arg: 'hello') -> 'world':
    pass
{% endhighlight %}

  * [PEP 3102](http://legacy.python.org/dev/peps/pep-3102/): Keyword-only arguments.

{% highlight Python %}
# Not required to have a default value.
def func(*args, keyword_only):
  pass

def func(*, keyword_only):
  pass
{% endhighlight %}

  * [PEP 3104](http://www.python.org/dev/peps/pep-3104): `nonlocal`, lexical scoping.
  * [PEP 3132](http://legacy.python.org/dev/peps/pep-3132/): `a, b, *rest = any_iterable` or `*rest, c = stuff`.
  * [PEP 0274](http://www.python.org/dev/peps/pep-0274): Dictionary comprehensions `{k : v for k, v in stuff}` and set comprehensions `{e for e in stuff}`.
  * Set literal `{1, 2}`. **`{}` is an empty dictionary.**
  * [PEP 3109](http://www.python.org/dev/peps/pep-3109) and [PEP 3134](PEPhttp://www.python.org/dev/peps/pep-3134): Exception chaining:

{% highlight Python %}
# Re-raise the active exception.
raise
# Implicit exception chaining.
# e.__context__
raise E(V)
# Explicit exception chaining.
# e.__cause__ == C
raise E(V) from C
{% endhighlight %}

  * `True`, `False`, and `None` are reserved words.
  * `except exc as var`.
  * <del>`object.__metaclass__`</del> `class C(metaclass=M)`.
  * Ellipsis `...` is a built-in constant.
  * Classic classes are gone.
  * All import forms not starting with `.` are interpreted as absolute imports.
  * [PEP 3129](http://legacy.python.org/dev/peps/pep-3129/): Class decorators.
