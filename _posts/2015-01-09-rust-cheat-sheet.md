---
layout: post
title: Rust Cheat Sheet
---

[Rust](http://www.rust-lang.org)
is like C++ designed by Haskell programmers.
For full reference, see
[here](http://doc.rust-lang.org/reference.html).


#### Ownership, borrowing, and lifetime.

* It's quite like Java's `ReadWriteLock`:
  At any moment, there are either multiple readers or only one writer.
* Borrowing (and further borrowing from a borrower) forms **nested** lifetimes.
* Most likely, rust compiler may infer the lifetimes for you,
  but you will have to annotate them explicitly sometimes.


#### Comments.

{% highlight rust %}
// This is normal comment.

/// This is doc comment, which supports Markdown notation inside.
{% endhighlight %}


#### Rust is expression oriented, not statement oriented.

* Rust has two kinds of statements:
  - Declaration statement.
  - Expression statement.
* Use `let` (declaration statement) for variable binding.
  Bindings are immutable by default.
* You may use `return` for early return from a function.
  Note: Using `return` in the last statement is considered a poor style.
* **You may return early in a "branch" of an expression.**
* A statement returns `()`, which is called unit.

{% highlight rust %}
// You may return early in an expression!
let num = match input_num {
    Some(num) => num,
    None => {
        println!("Please input a number!");
        return;
    }
};
{% endhighlight %}

{% highlight rust %}
fn add_one(x: i32) -> i32 {
    // Don't use return here; it's considered a poor style.
    x + 1
}
{% endhighlight %}

{% highlight rust %}
for var in expression { ...  }
while expression { ...  }
loop { ...  }
{% endhighlight %}


#### Tuple, struct, tuple struct, and enum.

* Struct: `struct Point { x: i32, y: i32, }`
* Tuple struct: `struct Inches(i32)`
  - Use tuple struct as Rust's `typedef` without implicit type conversion.
* `enum OptionalPair<S, T> { Value(S, T), Missing }`
  - Rust's `enum` is quite Haskell-ish.
  - Enum is better when it is used with match.
  - Enum is the foundation of complex data structures (lists and trees)
    because Rust does not have null pointers.


#### `impl` and `trait`.

* Use method call syntax via the `impl` keyword.
  - `impl Struct { ... }`.
* Static methods are just methods that don't take a `self` parameter.
  They are called by `Struct::method()` syntax.
* Rust's trait is like Java's interface.
  - `trait Trait { ... }`.
  - `impl Trait for Struct { ... }`.
* You are required to `use` any traits that you are going to use.
  This protects you from being implicitly affected by some third-party traits.
* Either the type or the trait has to be defined in your crate.
  So, you cannot "extend" a third-party library's type for its trait.
  (This is probably more a compiler-implementation restriction than a safety measure?)


#### Generics.

* Rust's generics is done in compile time (statically dispatched).
* You may add a trait constraint to generic functions;
  it's quite like Java's bounded type parameter.


#### Closure and moving closure.

* `|&: x: i32| -> i32 { 1 + x }`.
* Inside `|...|` is the parameter list, as like in a function,
  **plus an optional bound list (?).**
* Use `move` keyword to indicate that it's a moving closure,
  such as `move || x * x`.
* Moving closures transfer ownership (not borrow from).
  Think of C++'s move semantics.
  They are very useful in threads.


#### Match expression.

* It is also quite Haskell-ish.
* Non-exhaustive match arms will result in a compiler error.
* It supports quite powerful pattern matching:
  - `|` for matching multiple patterns.
  - `...` for a range of values.
  - `_` for catch-all.
  - `@` for binding value to a name.
  - When matching on an enum, use `..` to ignore the value.
  - Matching guards with `if`.
  - Match pointers with `&`.
  - Use `ref` or `ref mut` to get a reference.
  - You may match a struct or a slice of an array, too.


#### Rust has two kinds of strings.

* They are a sequence of UTF-8 bytes.
* `&str` is a string slice.
* `String` is a growable string.
* `"Hello there"` is statically allocated string literal, typed string slice.
* `str.to_string()` creates a new string, which is expensive.
* `string.as_slice()` returns a view of the string, which is cheap.


#### Array, vector, and slice.

* Array is fixed size, and the size is part of the type information.
  - `let a: [i32; 3] = [0; 3]; // Or [0, 0, 0].`
  - It is immutable by default.
  - `a.len()` and `a.iter()`.
* Vector is growable.
  - `let v: Vec<i32> = vec![1, 2, 3];`
* Slice is a view of the underlying data.
  - Its type is `&[T]`.


#### Heap.

* Use heap when you cannot know the amount of memory statically,
  such as in a recursive data structure (lists or trees).
* Use `box` keyword (like C++'s `new`) and
  it returns a `Box<T>`, which is quite like C++'s `unique_ptr`.
* `Rc` and `Arc` are Rust's reference-counting smart pointers.
  - Cyclic reference will cause memory leak.


#### Crates and modules.

* A crate is a unit of independent compilation; it produces an executable or a library.
  - It contains a hierarchy of modules.
  - Import another crate with `extern crate some_crate;`.
* A module is merely a namespace of symbols, which may spread across multiple crates.
  - The default visibility of a module is private.
  - Use `mod` to create a submodule.
  - Use `pub` to make a symbol public.
  - To create a shortcut for that symbol, use
    `use some_create::some_module::...::some_name;`.
  - `pub use` to bring symbols in.
* Put simply, crates group code physically whereas modules group code logically.


#### Threads.

* Use moving closure.
* Use message passing and share nothing.
* `panic!()` and `thread::try()`.


#### Macro.

* `macro!(...)` or `macro![...]` is how meta programming is done in Rust.


#### Attributes.

* Meta-data annotation of Rust.
* `#[test]`
* `#[warn(non_camel_case_types)]`
* `#![crate_name = "projectx"]`
  - Note the `!` mark.
