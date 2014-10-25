---
layout: post
title: Protocol Buffers for Android Apps
---

tl;dr Use
[Wire Protobuf](https://github.com/square/wire)
and don't use Google's
[Nano Protobuf](https://github.com/android/platform_external_protobuf/tree/master/java/src/main/java/com/google/protobuf/nano).

This all starts with Android's version of the Y2K problem:

* Android cannot run an app with more than 65536 methods because
the [Dalvik method invoke instruction](http://source.android.com/devices/tech/dalvik/dalvik-bytecode.html)
use only 16 bits for method reference index.
In theory you might be able to have more than 65536 methods in one DEX file, but Dalvik simply cannot reference them.
* Earlier versions of Android cannot even install an app with
["too many" methods](https://www.facebook.com/notes/facebook-engineering/under-the-hood-dalvik-patch-for-facebook-for-android/10151345597798920)
because `dexopt` reserves a fixed, insufficient amount of buffer for storing information about methods.

The [original Protocol Buffers library](https://code.google.com/p/protobuf/)
is not designed for Android's method number limitation and generates a generous amount of methods---at least nine for each optional or required field and at least eighteen for each repeated field.
To address this problem,
Google developed [Nano](https://github.com/android/platform_external_protobuf/tree/master/java/src/main/java/com/google/protobuf/nano)
and Square developed [Wire](https://github.com/square/wire).
Here I am advocating that Wire is the only sensible solution of the two,
and Nano's design is so fundamentally flawed that you should never use it.
Note: I did not evaluate all Protobuf implementations,
and there could be better alternatives than Wire (and most surely than Nano).

The fatal flaw of Nano is how it handles the "has"/"has-no" state of optional fields:
It uses not-equal-to-zero to denote the "has" state
("zero" here we mean `0` for integers, `""` for strings, etc.).
This means that if an optional field is set to zero, this field is in the "has-no" state.
And Nano will not serialize a zero value to the output byte stream---because a zero value means it does not have a value.

I cannot stress enough how wrong this design is (it should really be considered a bug).
For example, if you have a `Weather` proto that has a `temperature` optional field,
you will not be able to tell whether the temperature is 0, or temperature has not been not measured (and then set).
However, the evil of this design does not stop there.
It also breaks the compatibility among Protocol Buffers implementations.
If a component is changed to use Nano,
every component that exchanges protos with the offending component will likely be broken
because the offending component now has different semantics of the "has" state.

Google tried to save face and added the `reftypes` style for optional fields that implemented the correct semantics.
Unfortunately this style is not default,
and there does not seem to be a plan to remove the semantically incompatible "default" style.

Anyway, let's put the zero-means-has-no issue aside and compare Nano and Wire.

**Number of methods.**
Nano only generates a fixed number of helper methods per message,
such as `clear()` or `equals()`.
Wire supports the chained builder pattern;
every field corresponds to a setter method in the builder class.
So Nano's number of methods is in proportion to the number of message definitions
where as Wire's is in proportion to the number of field definitions.
However,
I think you are unlikely to hit the method number upper bound with Wire,
and even if you do hit the limit,
I don't think switching from Wire to Nano will regain much space for you
(but switching from the original Protocol Buffers library to Wire surely will).

**Ease of use.**
Wire's proto objects are deeply immutable.
I believe Wire is the winner in this part.

**Memory footprint per object.**
Nano is designed to be used like `struct` of C and uses primitive types for primitive values.
Wire handles the "has"/"has-no" state the same way as Nano's `reftypes` style;
it generates a reference for every field---even primitive types---where `null` means "has-no".
So a Wire proto object should occupy more memory than a Nano proto object.

**Memory footprint of meta-data.**
To store meta-data, Nano "unrolls" them in the generated Java code
whereas Wire uses annotation.
The bytecode size of both are in proportion to the number of fields,
but I guess Wire's representation should be more compact than Nano's,
even if we add the meta-data tables that Wire constructs in runtime.
(I have not profiled this; take my guess with a grain of salt.)

**Efficiency.**
I have not benchmarked Wire and Nano either,
but purely based on reading the code,
I guess that because Wire uses annotation for meta-data,
Wire would be slightly less efficient than Nano
(the iteration overhead of going through the meta-data `LinkedHashMap`,
which are constructed in runtime with reflection on annotations).

Overall my recommendation is:

* Use Wire Protobuf.
* If you have to use Nano Protobuf, make sure you use the `reftypes` or `reftypes_compat_mode` style.
