---
title: Garbage Collection in Python
date: 2019-11-29 01:39:00 +05:30
categories:
- python
- tutorial
- productivity
- beginners
tags:
- python
- beginners
- productivity
- tutorial
category: blog
---



In languages like C or C++, the programmer is responsible for dynamic allocation and deallocation of memory on the heap. But in python programmer does not have to preallocate or deallocate memory.

Python uses following garbage collection algorithms for memory management.

- Reference counting

- Cycle-detecting algorithm (Circular References)

### Reference counting

Reference counting is a simple procedure where referenced objects are deallocated when there is no reference to them in a program.
In short, When the reference count becomes `0`, the object is deallocated(frees its allocated memory).

Let's have a look at below example -
```python
def calculate_sum(num1, num2):
    total = num1 + num2
    print(total)
```

In the above example, We have three local references `num1`, `num2` and `total`. Here `total` is different from `num1` and `num2` because it only has reference inside the block thus its reference count is 1, `num1` and `num2` referenced outside the block so maybe their reference count more than one.

So, here when the function has finished execution the referenced count of `total` reduced to `0`. Since it tracked by the garbage collector.

The garbage collector finds out that the `total` is no longer referenced(reference count field reaches `0`) and frees its allocated memory.

The Variables, which are declared outside of functions, such variables do not get destroyed even after function has finished execution.

We can do the manual deletion also using the `del` statement. `del` statement removes a variable and its reference. When the reference count reaches `0`, it will be collected by the garbage collector.

The reference counting algorithm has some issues also, such as circular references -

### Circular References

A reference cycle occurs when one or more objects are referencing each other.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/wd5do4dybewld70sqjko.jpg)

As you can see in the above image, the `list` object is pointing to itself, and `object1` and `object2` are pointing to each other. The reference count for such objects is always at least `1`.

Let's go to the practical -

```python
import gc

gc.set_debug(gc.DEBUG_SAVEALL) 

lst = []
lst.append(lst)

lst_address = id(lst)

del lst

object_1 = {}
object_2 = {}
object_1['obj2'] = object_2
object_2['obj1'] = object_1

obj_address = id(object_1)

del object_1, object_2
```
In the above example, the `del` statement removes the variable and its reference to the objects.

Let's check-it deleted variables using `gc.collect`, `gc.collect` saves to `gc.garbage` instead of deleting.

```python
>>> gc.collect()
3
```
When we delete a variable, we only delete the `__main__` reference. Now we don’t have access to `lst`, `object_1` and `object_2` at all, but these variables still have 1 reference, it means reference count is 1, the reference count algorithm will not collect it.

Check the reference count as below-

```python
import sys
print(sys.getrefcount(obj_address))
print(sys.getrefcount(lst_address))

2
2
# 1 from the variable and 1 from getrefcount
```
Multiply this number by 1 Million objects and you may have absolutely a serious memory leak issue.

For this kind of Reference cycle, Python has another algorithm specially dedicated to discovering and destroying circular references. It is also the only controllable part of [Python’s GC](https://docs.python.org/3.6/library/gc.html)

### Summary
Python has 2 Garbage Collection algorithms. One for dealing with reference count, When the reference count reaches `0`, it removes the object and frees its allocated memory. The other is the Cycle-detecting algorithm which discovers and destroys circular references.

I hope that you now have a fair understanding of the Garbage Collection algorithms in python.

If you have any suggestions on your mind, please let me know in the comments.