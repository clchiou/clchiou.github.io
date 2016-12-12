---
layout: post
title: Notes of HIC++ Coding Standard
---

Notes of the [High Integrity C++ Coding Standard](http://www.codingstandard.com/section/index/) by PRQA.  These notes are by no means exhaustive.  (Sometimes I feel HIC++ is pretty paranoid.)

* ODR: one-definition rule
* NSDMI (non static data member initializer): C++11 syntax let you write initializer of a member at where you declare it

## Notes

* Use at least C++11
* No non-reachable statements (including missing else and default clauses
  * Or compiler might implicitly remove them
* Don't `++` on bool type (this "feature" is deprecated)
* Don't use `register` keyword (it is deprecated)
* Don't use throw exception specifications (it is deprecated)
  * New syntax is `noexcept` or `noexcept(true)`
* Digraphs or trigraphs might surprise you

  ```
  "??/??/"  // ??/ becomes \
  ::std::vector<::std::pair>  // <: becomes [
  ```

* Don't use C comment `/* ... */` (this is sorta drastic...)
* Don't comment out code - remove them
* Don't hide declarations (i.e., declare variables of the same name), which could result in nasty bugs when code is changed
* Don't declare functions at block scope, but declare them at the (more usual) `namespace` scope so that every one sees the same function signature
* Don't use global variable (partly because its initialization order is unspecified or undefined?)
  * And don't use the static singleton pattern (HIC++ states that its destructor is called at program termination, which might not be compatible with other uses, like shared library)
* Don't assign the address of a variable to a pointer with a greater lifetime
  * C++ defines 4 kinds of storage duration
  * static - until program termination
  * thread - until thread termination
  * automatic - upon exiting the enclosing block
  * dynamic
* Don't assume memory layout of values or objects, i.e.,
  * Don't use `union`
  * Don't assume `sizeof(int)` (or better, use `int32_t` or `int64_t`)
  * Don't mix bitwise and arithmetic on the same variable - this assume certain endianness
  * Don't use relational operators on pointers (as `p < q`)
  * Don't use `offsetof` macro
* Avoid array-to-pointer conversion on function argument - use `std::array` to preserve array dimension
* Add `U` suffix to a literal when an unsigned value is used - to avoid implicit conversion (as `2U`) (Could I use `u` instead of `U`?)
* Be aware and avoid data loss:
  * Implicit conversion
  * Shift operation
  * Overflow in signed arithmetic (all of `+ - * /` ...)
  * Wraparound in unsigned arithmetic (all of `+ - * /` ...) (sometimes you could be paranoid)

  ```
  uint32_t safe_inv_mult(uint32_t a, uint32_t b
  {
    if (b != 0 && a > UINT_MAX / b) {
      throw std::range_error("overflow");
    }
    // Wraparound is not possible
    return 0 == a || 0 == b ? UINT_MAX : 1000 / (a * b);
  }
  ```

* Don't depend on the evaluation order of operands (as in `i + i++`) except for `&&`, `||`, and `?:`.
  * The standard even goes as far as to ban pre-/post-increment in subexpressions (as in `x = i++`)
* Don't capture variables implicitly in a lambda
* When use recursion, place safe guards to prevent deep recursion
* Don't negate unsigned value (as in `-1u`), use `~` instead (as `~0u`)
* Don't use bitwise operators with signed operands
* Use `std::numeric_limits<float>::epsilon()` or `std::numeric_limits<double>::epsilon()` when comparing equality of floating-point number (search it before use it!)
* Pointer to a virtual member function cannot **only** be compared with `nullptr` (comparison with other (function) pointer is unspecified)
  * (Why?)
  * (Could it compare to self?)
* A long chain of if-else-if should be followed by a else block (which throws an exception) - better safe than sorry
* A switch on enumerated values should either have a default clause (which throws an exception) or be a full enumeration (in this case, compiler will warn you and so you should **not** add a default clause)
* Try not to alter loop counter variable more than once in the loop body (because it's harder to understand and maintain)
* **Only one** identifier per declaration (for clarity) (except in for loop initialization statement)
* Use `const` whenever possible, but **not** at by-value return type of a function (otherwise will prohibit move semantics)
* Don't specify return type of a lambda (and let compiler deduce the type - to avoid implicit type conversion)
* Use explicit enumeration base like `enum E : int8_t` and `enum class E : int32_t`
  * Except in an `extern "C"` block of course
  * Also, `enum class` is preferred than plain `enum`
* Don't define unnamed namespace in a header file (because it is unique to the translation unit)
  * Use unnamed namespace to define objects/functions/types to be used from a **single** translation unit in the source file (not header file)
* (Adhere to ODR) inline function, function template, and type are define in a single header file if they are going to be shared among multiple translation units
  * Object and function should be **declared** in a single header file to be shared
* (Readability) Don't use more than one level of pointer (i.e., don't use `int * const * const p`) or nested containers
* Don't pass `std::unique_ptr` by const reference (but non-const reference is fine)
* Don't use default argument (hard for maintenance and refactoring)
  * If must, use (inline) overloaded function
* Define `=delete` functions with parameters of type rvalue reference to const (!!)
  * A simple model for rvalue reference is that it allows for modification of a temporary.  A const rvalue reference thus defeats that purpose.  But for `=delete` function, this will disallow the use of an rvalue as an argument to that function.
* Inheritance from multiple non-interface class is probably a wrong design
  * Interface class:
    * All (public) member functions are virtual or are getters
    * At most one integral or enum data member (this rule is strange...)
* Base class destructor should be declared as either: (public) virtual, protected, or private
  * If delete via pointer to base: declare (public) virtual destructor
  * If not: declare (non-virtual) protected or private destructor
    * This way, it's a compiler error when delete via pointer to base
* Use delegating constructor to reduce code duplication (i.e., call another constructor in member initialization list as `Foo() : Foo(0) {}`)
* Don't do any other thing than copy/move members in a user-defined copy/move constructor (because compiler may optimize away unnecessary copies or moves; so your extra things may not be called)
* Move constructor and move assignment should be `noexcept` (this should not be hard to achieve since you don't do any other thing than move members)
  * This is greatly useful in providing strong exception guarantee (and friendly to the standard library containers, too)
* Use ref-qualifier `&` to make "modifiable rvalue" a compiler error
  * E.g., `Foo& operator*=(float) &;` (notice the last `&`)
* Universal reference (template parameter `T &&`) may result in confusion when used with overloaded functions - be careful (see [here](http://www.codingstandard.com/rule/13-1-2-if-a-member-of-a-set-of-callable-functions-includes-a-universal-reference-parameter-ensure-that-one-appears-in-the-same-position-for-all-other-members/))
* Don't overload `&&`, `||`, and `,` because they have special evaluation order for their arguments (but when overloaded, they are just functions and this special semantics does not apply)
* Don't overload `&` (unary operator) as it will result in undefined behavior when the overloaded operator is not visible from the call site (what!?) - see [here](http://www.codingstandard.com/rule/13-2-1-do-not-overload-operators-with-special-semantics/)
* Favor variadic template (type safe) over ellipsis (not type safe)
* You can declare a explicitly instantiated template as `extern`?!
* In constructor's or destructor's catch block, don't access non-static data member (will result in undefined behavior)
* Don't rethrow (an empty "`throw;`" statement) when there is not current exception to be rethrown
* Don't use `std::vector<bool>` (this specialization is not standard conformant)
* You can't move `const` or `const &` type (using `std::move` on them will not work)
* What is `std::forward`? (To forward universal reference? What did you say?)
* Don't create an rvalue reference of `std::array` (i.e., move it)
  * Because `std::array` is just a wrapper for a C-style array, moving it takes linear time
* C++11 allows for perfect forwarding, thus you may write:

  ```
  // The constructor arguments (the 0 here) will be
  // forwarded to the constructor of int32_t object
  std::shared_ptr<int32_t> p = std::make_shared<int32_t>(0);
  ```

* `std::remove`, `std::remove_if`, and `std::unique` swaps elements and returns the "new" last location; so you should call `std::erase` on that
* `std::mutex` is non-reentrant (don't acquire it twice in the same thread, it's undefined behavior)
* Nesting of locks should form a DAG (to avoid deadlock)
* Don't use `std::recursive_mutex` (it's a sign of bad design)

## Misc

* Pre-increment/-decrement may be more efficient than post-increment/-decrement (and thus preferred)
  * This is because the post form requires a copy to be made
* Don't use `rand()` and friends because they alter global state (the seed) and are not very random anyway
  * `std::mt19937` should be a better option
