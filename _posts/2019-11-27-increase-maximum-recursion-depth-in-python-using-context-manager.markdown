---
title: Increase maximum recursion depth in python using Context Manager
date: 2019-11-27 23:15:00 +05:30
tags:
- python
- tutorial
- beginners
- productivity
category: blog
---

A context manager is an object that defines the runtime context to be established when executing a **with** statement. The context manager handles the entry into, and the exit from, the desired runtime context for the execution of the block of code.

Let's have a look at below example, suppose I want to calculate the Fibonacci of a number - 
```python
def fib_cal(fib_num, memo):
    if memo[fib_num] is not None:
        return memo[fib_num]
    elif fib_num == 1 or fib_num == 2:
        result = 1
    else:
        result = fib_cal(fib_num-1, memo) + fib_cal(fib_num-2, memo)
    memo[fib_num] = result
    return result


def get_fibonacci(fib_num):
    memo = [None] * (fib_num+1)
    return fib_cal(fib_num, memo)

print(fibonacci(100))
```
***Output-*** 354224848179261915075
- Let suppose if my number if bigger like - 3000
```
print(fibonacci(3000))
```
- In the above case, it’ll throw an error- 
```
RecursionError: maximum recursion depth exceeded in comparison
```
because the maximum recursion limit of the os has been exceeded. you can check recursion limit as follow - 
```python
import sys
sys.getrecursionlimit()
```
So, In this kind of situation, we can use context manager which will allow us to allocate and release resources precisely when we want to.
```python
import sys

class RecursionLimit:
    def __init__(self, limit):
        self.limit = limit
        self.cur_limit = sys.getrecursionlimit()

    def __enter__(self):
        sys.setrecursionlimit(self.limit)

    def __exit__(self, exc_type, exc_value, exc_traceback):
        sys.setrecursionlimit(self.cur_limit)


MAX_LIMIT = 10000

with RecursionLimit(MAX_LIMIT):
    print(fibonacci(3000))
```
**Output**-  4106158863079712603335683787192671052201251086373692524........6000

The `__enter__()` returns the resource that needs to be managed and the `__exit__()` does not return anything but performs the cleanup operations.

Context-managers can be used for other purposes also like- simple file I/O, like opening and closing sockets, implementing set-up and tear-down functionality during testing.

I hope you enjoyed reading this article, If you have any suggestions on your mind, please let me know in the mail. 