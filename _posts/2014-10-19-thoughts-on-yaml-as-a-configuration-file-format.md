---
layout: post
title: Thoughts on YAML as a Configuration File Format
---

YAML is simple and human-readable,
but surprises you at unwelcome places when used as a configuration file format.

YAML defines itself as a data serialization language;
it tries to create a textual representation of data structures.
YAML does not define itself as a format for configuration files.
Although we can use YAML that way, it will surprise you, like what SaltStack has
[found](http://docs.saltstack.com/en/latest/topics/troubleshooting/yaml_idiosyncrasies.html).

The number one idiosyncrasy is the visual confusion when using 2-space indentation.
We want to express sibling as well as nested relationships in YAML,
but it sometimes confuses people about you intention when you use 2-spece indentation:

{% highlight yaml %}
# Is this a nested dict? (No.)
confused:
  - context:
    custom_var: 'override'

# This is nested dict.
nested:
  - context:
      custom_var: 'override'
{% endhighlight %}

More importantly,
I lean toward that PyYAML should not interpret scalar values at all
when YAML is used as a configuration file format.
I would say that most of the idiosyncrasies that SaltStack has found in YAML
(and then "fixed" with string quotations)
could be avoided if PyYAML does not interpret scalar values at all.

{% highlight python %}
# What PyYAML is doing...
yaml.load('0700') == 448
yaml.load('on') == yaml.load('yes') == True
yaml.load('2014_10_10') == 20141010

# What I think PyYAML should do when YAML is
# used as a configuration file format...
yaml.load('0700') == '0700'
yaml.load('on') == 'on'
yaml.load('yes') == 'yes'
yaml.load('2014_10_10') == '2014_10_10'
{% endhighlight %}

If PyYAML does not interpret scalar values at all,
a configuration file will simply be a graph of strings where non-terminal nodes are either a sequence or a mapping.
If we go further and ban anchors, YAML will be reduced to a tree.
I believe this tree-of-strings model is both easier to understand and harder to write it wrong.

Lastly, I have an even more radical idea:
PyYAML `load` and `dump` should preserve as much textual structure as possible.
Naively speaking, this means that `yaml.dump(yaml.load(string)) == string`.
Preserving textual structure should yield
a smaller "diff" when a YAML document is processed by a program.

Consider this: A program reads a configuration file, adds an entry to a mapping,
and writes to a new configuration file.
It will be desirable that...

{% highlight bash %}
diff config.yaml config-new.yaml
{% endhighlight %}

...only prints the added entry.
However, since PyYAML does not preserve the order of keys,
it is very likely that the `diff` will print lots of irrelevant "diff"s.

I know that YAML spec (and thus PyYAML)
[rejects](http://pyyaml.org/ticket/29)
preserving key order in mappings,
but I will argue that
one of YAML's primary goals is to be human-readable,
and textual structure is very important to human readability;
thus preserving key order should be more important than the dogma that mappings are unordered.

To sum up, I think when using YAML as a configuration file format, we should...

* Use it as simply a tree of strings (don't interpret scalar values and don't use anchors), and
* Preserve as much textual structure as possible (preserving key order is a good start).
