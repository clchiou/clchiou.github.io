---
layout: post
title: Python Queue and Executor
---

It is probably best to explain how to use Python's `Queue.task_done` method from digging into `ThreadPoolExecutor`.

At first glance, `Queue.task_done` seems to be weird.
A few questions pop out of my head:

* If you are not using `Queue` as a task queue, should you call `task_done`?
* If you do use `Queue` as a task queue, when should you call `task_done`?
  When a task is dequeued?
  When a task is done?
  If a task failed, should you call `task_done` anyway?

I found that digging into `ThreadPoolExecutor` helps me answer the above questions.

The `ThreadPoolExecutor` is basically a first-in-first-out queue pulled by a group of worker threads.
Then a natural question is,
how does the caller, or specifically, the `Executor.shutdown` method,
know when there is no more work need to be done?

The caller cannot simply wait for queue to be empty
because a worker thread might put more work items into the queue
while it is processing an work item.
So the caller has to wait for **both** queue to be empty **and** all worker threads to be idle.

To let the caller wait for both conditions,
`Queue` implements this strange method `task_done`.
Whenever `Queue.put` is called, an internal counter is incremented.
And whenever `Queue.task_done` is called, the internal counter is decremented.
Then `Queue.join` simply waits for this counter to reach zero.

Since you want the internal counter to reach zero when both conditions are met,
worker threads have to call `task_done`
after they have done an work item
(regardless it succeeded or failed)
**and** is about to wait for the next item.
Because at this point,
a worker thread is certainly not going to put more work items into the queue,
and is also about to be idle again.

An related question is, how do worker threads know there is no more work to do?
(And then they may terminate themselves, releasing all resources.)
Polling on `Queue.join` is certainly not optimal.
Instead, the `shutdown` method put a sentinel object (`None`) into the queue.
And when a worker thread get the sentinel object,
it knows there is no more work to do.

Personally,
I feel this solution too complicated for implementing an executor.
I believe a
[closable queue](https://github.com/clchiou/garage/blob/master/garage/threads/queues.py)
is both easier to understand and more intuitive to use.

Then what is a closable queue?
If you treat a queue like a Unix pipe,
then you certainly should be able to close the queue, right?
After you close a queue, the queue enters into a "closed" state.
All writers should not be able to put any item into the queue
(doing so would raise a `Closed` exception),
but readers may still get items from the queue until it is empty
(trying to get more would raise a `Closed` exception).
Note, however,
when a queue is not in the "closed" state,
reading from an empty queue should not raise a `Closed` exception
but should simply block the reader.

To implement executors with a closable queue,
the `shutdown` method simply calls `Queue.close` and then joins on all worker threads.
And worker threads should terminate themselves when reading from the queue raises a `Closed` exception.

(What if when `Executor.submit` calls `Queue.put`, it raises a `Closed` exception?
Well, this means that `shutdown` has been called,
and by the contract of `Executor.submit`,
a `RuntimeError` will be raised.)

The key here is that,
executors actually do not need an API as generic as `task_done`
since we require that when `shutdown` is called,
no more work items are allowed to be submitted.
So after this point,
when the queue is empty,
we are certain that no more work need to be done.
