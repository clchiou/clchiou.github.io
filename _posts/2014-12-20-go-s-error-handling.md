---
layout: post
title: "Go's Error Handling"
---

[Go's error handling](https://golang.org/doc/effective_go.html#errors)
is like a Hollywood blockbuster plus an unnecessary and dumb sequel.

The blockbuster:
When error happens,
you return an out-of-band error code.
Or when the error is unrecoverable or the program simply cannot continue,
you bring down the whole program with a built-in `panic` function.
It is simple and effective.
Good!

The unnecessary and dumb sequel:
However,
you may use a built-in `recover` function to intercept a `panic`.
Go is a highly opinionated language---Exceptions are
[deliberately rejected](http://blog.golang.org/error-handling-and-go)
by design because Go favors explicit error handling.
`panic` and `recover` together effectively forms a half-baked exception mechanism.
To me,
Go's arguments against exceptions are a slap in face for itself.

Could Go simplify error handling in deeply nested function call stack
without the use of `recover`?
I guess Go could add a version of `panic` (say, `panic_goroutine`) that
only terminates the current goroutine, not the whole program.
Then we will delegate the work (deeply nested function call stack)
to a worker goroutine,
and will have a monitor goroutine wait for the result.
If the worker goroutine finds a unrecoverable error or simply cannot continue,
it sends an error code to the monitor and `panic_goroutine`.

I think using goroutine for error handling is simpler and more consistent with Go's design
than `recover`.
What do you think?
