---
layout: post
title: Google Java Style
---

[Google Java style](http://google-styleguide.googlecode.com/svn/trunk/javaguide.html)
defines some hard-and-fast rules, no non-enforceable advices.

* Import ordering:
  1. All static import.
  2. First-party packages.
  3. Third-party packages.
  4. java imports.
  5. javax imports.
* Class member ordering: It should have _some_ logical order, but no hard and fast rule here.
* Overloads (and constructors) should appear sequentially, with no intervening members.
* Always use braces, even when the body of a block contains only one statement.
* Egyptian brackets.
  - _No_ line break after the brace if it is followed by a comma.
  - A few exceptions for enum classes and empty block `{}`.
* Some exceptions for column limit:
  - A long URL in Javadoc.
  - Command lines in a comment that may be cut-and-pasted into a shell.
  - package and import statements.
* Line wrapping:
  - Prefer to break at a higher syntactic level.
  - At least +4 spaces.
* Space rules:
{% highlight java %}
@SomeAnnotation({a, b})
String[][] x = {{"foo"}};
new int[] {5, 6};
for (x : collection) {}
(X) x; // Space in casting.
enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
{% endhighlight %}
* Horizontal alignment: Never required.
* One variable per declaration.
  Declare when needed, initialized as soon as possible.
{% highlight java %}
int a;
int b;
{% endhighlight %}
* Array initializers can be block-like:
{% highlight java %}
new int[] {
  0, 1,
  2, 3,
};
{% endhighlight %}
* Fall-through in a switch case must be commented `// fall through`.
* `default` statement must be present, even if it contains no code.
* Modifier ordering:
  `public protected private abstract static final transient volatile synchronized native strictfp`
* `long`-valued literals use an uppercase `L` suffix, never lowercase.
* Naming rules:
  - Generally, don't use underscores.
  - `ThisIsClass`
  - `thisIsMethod`
  - Underscores may appear in JUnit test method names: `test<MethodUnderTest>_<state>`
  - `CONSTANT_CASE`
  - `nonConstantFieldNames` (static or otherwise) with no 'm' or 's' prefix.
  - `XmlHttpRequest // Acronyms.`
  - `customerId // Id.`
  - `supportsIpv6OnIos // IPv6 and iOS.`
  - `YouTubeImporter, AdWords // YouTube and AdWords.`
* You should always use `@Override` unless when the parent method is `@Deprecated`.
* Exception for empty `catch` block in tests (use special name `expected`):
{% highlight java %}
try {
  emptyStack.pop();
  fail();
} catch (NoSuchElementException expected) {
}
{% endhighlight %}
* At-clauses ordering in Javadoc: `@param`, `@return`, `@throws`, `@deprecated`.
* When an at-clause doesn't fit on a single line, continuation lines are indented 4+ spaces from the position of the @.
* Javadoc summary fragments are not a (imperative) sentence.
  They are usually just a noun phrase or verb phrase.
