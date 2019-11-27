---
title: Global Interpreter Lock (GIL) in Python
date: 2019-11-28 00:55:00 +05:30
tags:
- python
- tutorial
- beginners
- productivity
category: blog
---

Any operation which is executed in the interpreter, GIL ensures that the interpreter is held by a single thread at a particular instant of time. it means that only one thread can be in a state of execution at any point in time.

Let's take an example - 
* Single-Threaded
```python
import time
from threading import Thread

COUNT = 50000000

def countdown(num):
    while num > 0:
        num -= 1

start = time.time()
countdown(COUNT)
end = time.time()

print('Time is taken by single_threaded - {} Sec.'.format(end - start))

```
Output - 
```python
Time is taken by single_threaded - 1.9384803771972656 Sec.
```

* Multi-Threaded
```python
import time
from threading import Thread

COUNT = 50000000

def countdown(num):
    while num > 0:
        num -= 1

t1 = Thread(target=countdown, args=(COUNT//2,))
t2 = Thread(target=countdown, args=(COUNT//2,))

start = time.time()
t1.start()
t2.start()
t1.join()
t2.join()
end = time.time()

print('Time is taken by multi_threaded - {} Sec.'.format(end - start))
```
Output - 
```python
Time is taken by multi_threaded - 1.905367784500122 Sec.
```

As you can see both the example **single-threaded** and **multi-threaded** take almost the same time to finish the execution because GIL restricts CPU to only work with a single thread.
So even if your system could be have multiple cores/processors. And multiple cores allow multiple threads to execute simultaneously. But since the interpreter is held by a single thread.  So Python threads cannot take advantage of multiple cores also.

### How two threads perform both CPU and `I/O` operations during execution -

**01.** The python interpreter creates a process and spawns the threads.
**02.** When Thread 1 starts execution, it'll acquire the GIL and lock it.
**03.** Thread 2 has to wait for GIL to be released by Thread 1.
**04.** In case if Thread 1 is waiting for IO from the client, it releases the GIL and  Thread 2 will acquire it and start the execution.
**05.** Now suppose If Thread 1 gets the IO, Then Thread 1 has to wait for GIL to be release by Thread 2.

A thread waiting for the GIL will do a timed wait on the GIL, with a preset interval that can be modified with [sys.setswitchinterval](https://docs.python.org/3/library/sys.html#sys.setswitchinterval).

Note that Python's GIL is only really an issue for CPython, the reference implementation. Other implementations like Jython and IronPython don't have a GIL, because the platforms they are built on Java for Jython, .NET for IronPython, they handle dynamic memory management differently, and so can safely run the Python code in multiple threads at the same time.


### Why CPython use GIL - 

Python is not get executed directly, it gets compiled into Python bytecode and bytecode is executed. 

Python uses automatic memory management via garbage collection, implemented with a technique called [reference counting](https://dev.to/sharmapacific/garbage-collection-in-python-1d4g).

Python internally manages a data structure containing all object reference count, and when an object reference count reaches 0 it removes the object and frees its allocated memory.
However, race conditions in multithreaded programming made it so that the object reference count could be updated incorrectly, making it so that objects could be erroneously freed or never freed at all. One way to solve this problem is with more granular locking like locks are set on every shared object, but this would create issues such as increased overhead due to a lot of locks acquire/release requests, as well as increase the possibility of deadlock. 

The Python developers instead chose to solve this problem by placing a lock around the entire interpreter, making each thread acquire this lock when it runs Python bytecode.  This avoids a lot of the performance issues around excessive locking but effectively serializes bytecode execution.

### Why use Multithreading when GIL exists -

Suppose a Web application that receives requests from clients, does some query to the database and return the response to the client. While Waiting for IO to complete may take 90% (or more than that) of the time the request is processed. When a single-threaded application is waiting for IO and it just not using the core and the core is available for execution. So such application has a room for other threads to execute even on a single core.

So in this kind of case when one thread waiting for IO, it releases GIL and another thread can continue execution.

### Benefits -
1. It is faster in the single-threaded case.
2. It is faster in the multi-threaded case for IO-bound programs.
3. It is faster in the multi-threaded case for CPU-bound programs that do their compute-intensive work in C libraries.


>[Quote from the Python threading documentation](https://docs.python.org/3.7/library/threading.html)
In CPython, due to the Global Interpreter Lock, only one thread can execute Python code at once (even though certain performance-oriented libraries might overcome this limitation). If you want your application to make better use of the computational resources of multi-core machines, you are advised to use [multiprocessing](https://docs.python.org/3.7/library/multiprocessing.html#introduction) or [concurrent.futures.ProcessPoolExecutor](https://docs.python.org/3.7/library/concurrent.futures.html#processpoolexecutor). 
However, threading is still an appropriate model if you want to run multiple [I/O-bound tasks simultaneously] (https://docs.python.org/3.7/library/threading.html).

The GIL is a problem if, and only if, you are doing CPU-intensive work in pure Python. Many Python libraries solve this issue by using C extensions to bypass the GIL.

I hope that you now have a fair understanding of the Global Interpreter Lock in python.

If you have any suggestions on your mind, please let me know in the comments.
