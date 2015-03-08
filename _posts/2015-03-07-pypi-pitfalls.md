---
layout: post
title: PyPI Pitfalls
---

Say you just wrote a shining new Python package
that you would like to submit to [PyPI](https://pypi.python.org/).
Here are some pitfalls you might step on.

* There is a [testing repository](https://testpypi.python.org/pypi)
  you may try before submitting to the real repository.

* No [Markdown](https://bitbucket.org/pypa/pypi/issue/148/support-markdown-for-readmes)
  for now.
  Stick with [reStructuredText](http://docutils.sourceforge.net/rst.html)
  for your README instead.

* I accidentally put a comma at the end of `__author_email__` and turned it into a `tuple`.
  And `distutils.command.upload` was quite upset by this.

* You must chain `upload` with a distribution command (`sdist`, etc.).
  Yup, you cannot execute them separately.
  You must execute them together.

* Unlike standard commands, extra commands of `setuptools` don't seem to be chain-able?

{% highlight sh %}
# upload will complain about
#   error: No dist file created in earlier command
python setup.py sdist
python setup.py upload

# Instead, you must execute them together.
python setup.py sdist upload

# sdist will not be executed because
# test is an extra command.
python setup.py test sdist
{% endhighlight %}
