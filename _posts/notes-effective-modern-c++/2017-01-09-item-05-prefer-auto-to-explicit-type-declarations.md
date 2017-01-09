---
layout: post
category: notes-effective-modern-c++
title: "Item 5: Prefer auto to Explicit Type Declarations"
---

`auto` saves typing, prevents incorrect or less-performant (eliminate unnecessary temporary objects) usages, and sometimes is a must.

Benefits of using `auto`:

* You can't forget to initialize a variable, unlike `int x;`
* You won't type long type names, such as `typename std::iterator_traits<It>::value_type`
* Declare closure-typed variable
  * Closure type is only known to compiler and **there is no syntax to declare a closure**
  * So in this case, you must use `auto` (unlike the former two which are just niceties)
* Prevent mistakenly using the incorrect type
  * And be more resilient against interface changes

```c++
std::vector<int> v;

// unsigned is incorrect
// The correct type is std::vector<int>::size_type
unsigned s = v.size();

// Deduce the correct type
auto s = v.size();

std::unordered_map<std::string, int> m;

// std::string is incorrect (it should be const std::string).
// Worse, compiler will make temporary copies for each pair,
// which is slower and sometimes is a bug.
for (const std::pair<std::string, int>& p : m) {
  ...
}

// Deduce the correct type std::pair<const std::string, int>
for (const auto& p : m) {
  ...
}
```
