---
layout: post
title: Thoughts on Serialization Formats
---

When evaluating a serialization format,
how easy it is to evolve schemas (adding new fields, etc.)
is as important as storage compactness and encoding/decoding performance,
but is often overlooked.

[This](http://martin.kleppmann.com/2012/12/05/schema-evolution-in-avro-protocol-buffers-thrift.html)
is a very nice comparison on schema evolution for Avro, Protocol Buffers, and Thrift.
In short, Avro schema is harder to evolve than Protocol Buffers and Thrift.
Reason: Protocol Buffers and Thrift objects are essentially a list of tagged values,
where tags carry minimal schema information for a decoder to skip unrecognizable fields.
On the other hand, Avro objects are raw values concatenated _in a schema-defined order_.
So to decode an Avro object,
you need both the current schema and the schema when it was encoded.

However, not all raw-values formats are inherently hard to evolve their schemas.
The trick lies in how the fields are ordered.
[Cap'n Proto](https://capnproto.org/encoding.html)'s design should make it easy to evolve.
(I never used Cap'n Proto, but I would imagine this claim is ture.)

I rarely find Python `pickle` or Ruby `marshal` useful.
If you want to hack something quickly,
plain-text formats like JSON or YAML is easier to work with
(you may validate your date with a text editor).
If you are working on something serious or for production,
you probably should not use `pickle` or `marshal`.
(However, Rails uses `marshal` for serializing `ActiveRecord` and session cookie,
which in my opinion, is a really bad choice.)

JSON's indifference between `int`s and `float`s are sometimes annoying.
But YAML's unintuitive interpretation of scalar values is even worse
(hint: try `yaml.load('[0755, Yes]')` in Python).
