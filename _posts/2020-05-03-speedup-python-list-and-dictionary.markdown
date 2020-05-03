---
title: SpeedUp Python List and Dictionary
date: 2020-05-03 22:42:00 +05:30
categories:
- python
- productivity
- tutorial
- beginners
tags:
- python
- productivity
- tutorial
- beginners
category: blog
---

Today, we will be discussing the optimization technique in Python. In this article, you will get to know to speed up your code by avoiding the re-evaluation inside a list and dictionary.

Here I have written the decorator function to calculate the execution time of a function.

```python
import functools
import time

def timeit(func):
    @functools.wraps(func)
    def newfunc(*args, **kwargs):
        startTime = time.time()
        func(*args, **kwargs)
        elapsedTime = time.time() - startTime
        print('function - {}, took {} ms to complete'.format(func.__name__, int(elapsedTime * 1000)))
    return newfunc
```

let's move to the actual function


## Avoid Re-evaluation in Lists

**Evaluating `nums.append` inside the loop**

```python
@timeit
def append_inside_loop(limit):
    nums = []
    for num in limit:
        nums.append(num)

append_inside_loop(list(range(1, 9999999)))
```
In the above function `nums.append` function references that are re-evaluated each time through the loop. After execution, The total time taken by the above function
```python
o/p - function - append_inside_loop, took 529 ms to complete
```

**Evaluating `nums.append` outside the loop**
```python
@timeit
def append_outside_loop(limit):
    nums = []
    append = nums.append
    for num in limit:
        append(num)

append_outside_loop(list(range(1, 9999999)))
```
In the above function, I evaluate `nums.append` outside the loop and used `append` inside the loop as a variable. Total time is taken by the above function
```python
o/p - function - append_outside_loop, took 328 ms to complete
```
As you can see when I have evaluated the `append = nums.append` outside the `for` loop as a local variable, it took less time and speed-up the code by `201 ms`.

The same technique we can apply to the dictionary case also, look at the below example

## Avoid Re-evaluation in Dictionary

**Evaluating `data.get` each time inside the loop**
```python
@timeit
def inside_evaluation(limit):
    data = {}
    for num in limit:
        data[num] = data.get(num, 0) + 1

inside_evaluation(list(range(1, 9999999)))
```
Total Time taken by the above function - 
```python
o/p - function - inside_evaluation, took 1400 ms to complete
```

**Evaluating `data.get` outside the loop**
```python
@timeit
def outside_evaluation(limit):
    data = {}
    get = data.get
    for num in limit:
        data[num] = get(num, 0) + 1


outside_evaluation(list(range(1, 9999999)))
```
Total time taken by the above function - 
```python
o/p - function - outside_evaluation, took 1189 ms to complete
```
As you can see we have speed-up the code here by `211 ms`.

I hope you like the explanation of the optimization technique in Python for the list and dictionary. Still, if any doubt or improvement regarding it, ask in the comment section. Also, don't forget to share your optimization technique.