---
layout: post
category: notes-effective-c++
title: "Item 8: Prevent Exceptions from Leaving Destructors"
---

If you need to handle release failure in RAII, add a `close()` function.
Just **don't** let exceptions leave destructors.

* When an exception has been thrown and stack is unwinding, if another exception is thrown, the program could either terminate or yield undefined behavior.

* When you catch an exception in a destructor, you have two options:
  * Log and `abort()`.  This is usually the better (best?) option.
  * Log and swallow.  This is usually not recommended (because at this point the RAII-managed resource is probably in a strange state - half-open? half-closed?).  But if your program can proceed with this strange state, you could choose to swallow the exception and proceed.

* If handling release error is required, you should do this:
{% highlight c++ %}
class DBConn {
 public:
  void close() {
    // ...
    closed = true;  // Mark closed at last.
  }
  ~DBConn() {
    if (closed) {
      return;
    }
    try {
      close();
    } catch (...) {
      // ...
    }
  }
  // ...
};

void foo() {
  DBConn conn;  // RAII.

  // Use the conn object...

  try {
    conn.close();
  } catch (...) {
    // Handle close error.
  }
}
{% endhighlight %}
