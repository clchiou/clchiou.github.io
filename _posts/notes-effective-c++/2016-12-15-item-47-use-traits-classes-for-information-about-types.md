---
layout: post
category: notes-effective-c++
title: "Item 47: Use Traits Classes for Information About Types"
---

Traits: **Conventions** (not syntax rules) that allow you to get information about type during compilation.

Most commonly:

* Put type info into a template and one or more specialization of that template (so that traits work on even built-in types)
* Use function overloading to perform compile-time `if-else` tests on types

```c++
// First, define empty tag structs.
// Note the use of inheritance.
struct random_access_iterator_tag: public bidirectional_iterator_tag {};

// Second, declare trait class.
// Convention: They are defined as structs.
template<typename IterT>
struct iterator_traits;

// Third, "tag" your class.
template<T>
class Foo {
 public:
  class FooIterator {
   public:
    // E.g., "tag" FooIterator as a random access iterator.
    // Convention: Name the tag "iterator_category".
    typedef random_access_iterator_tag iterator_category;
  };
};

// Fourth, define the trait class to parrot back the nested typedef.
// For user-defined type:
template<typename IterT>
struct iterator_traits {
  typedef typename IterT::iterator_category iterator_category;
};
// For built-in types, use partial template specialization:
template<typename T>
struct iterator_traits<T*> {
  typedef random_access_iterator_tag iterator_category;
};

// Fifth, implement the behavior for each tag, such as:
template<typename IterT>
void doBar(IterT& iter, std::random_access_iterator_tag) {
  ...
}

// Finally, use function overloading to perform if-else tests.
// This part and the fifth part above together forms the whole "if-else" block.
template<typename IterT>
void bar(IterT& iter) {
  doBar(iter, std::iterator_traits<IterT>::iterator_category());
}
```

By the way, STL is primarily made up of: containers, iterators, and algorithms.
There are 5 categories of iterators:

* Input iterators (access data **only once** sequentially)
* Output iterators (access data **only once** sequentially)
* Forward iterators (access data any number of times sequentially)
* Bidirectional iterators (access data any number of times sequentially in both directions)
* Random access iterators
