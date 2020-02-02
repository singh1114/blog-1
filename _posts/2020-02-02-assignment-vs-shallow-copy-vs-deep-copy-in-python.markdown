---
title: Assignment vs Shallow Copy vs Deep Copy in Python
date: 2020-02-02 22:57:00 +05:30
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
---

Today, we will be discussing the copy in Python. There are three ways we can do it. In this article, you will get to know what each operation does and how they are different.

1 - Assignment Operator `(=)`
2 - Shallow Copy
3 - Deep copy

## **Assignment Operator (=)**
```python
>>> a = [1, 2, 3, 4, 5]
>>> b = a
```
In the above example of the assignment operator, It does not make a copy of the Python objects instead it copying a memory address (or pointer) from `a` to `b`, `(b=a)`. Which means both `a` & `b` pointing to the same memory address.

Here we can use the `id()` method to get the address of the object in memory and check if both lists are pointing the same memory.

```python
>>> id(a) == id(b)
True

>>> print('id of a - {}, id of b - {}'.format(id(a), id(b)))
id of a - 140665942562048, id of b - 140665942562048

```
So, here if you would edit the new list, It’ll get updated in the original list also - 
```python
>>> b.append(6)
>>> a
[1, 2, 3, 4, 5, 6]
>>> b
[1, 2, 3, 4, 5, 6]
```
Because there is only one instance of that list in the memory.

## **Shallow Copy**

A shallow copy constructs a new compound object and then (to the extent possible) inserts references into it to the objects found in the original.

we have 3 different ways to create a shallow copy -
```python
nums = [1, 2, 3, 4, 5]      

>>> import copy

>>> m1 = copy.copy(nums)       # make a shallow copy by using copy module
>>> m2 = list(nums)    # make a shallow copy by using the factory function
>>> m3 = nums[:]       # make a shallow copy by using the slice operator
```
Here all the above lists contains the same values as the original list -
```python
>>> print(nums == m1 == m2 == m3)
True
```

but the memory address of each is different - 
```python
>>> print('nums_id - {}, m1_id - {}, m2_id - {}, m3_id = {}'.format(id(nums), id(m1), id(m2), id(m3)))
nums_id - 140665942650624, m1_id - 140665942758976, m2_id - 140665942759056, m3_id = 140665942692000
```
This means that this time, each list's object has its own, independent memory address.

Now move to the more interesting part. If the original list is a compound object (e.g. a list of lists), then after shallow copy new list elements are still referencing the original elements. 
So, if you modify the mutable elements like lists, the changes will be reflected on the original elements. Let's look at the below example to get a better understanding -

```python
>>> import copy

>>> a = [[1, 2], [3, 4]]            
>>> b = copy.copy(a)

>>> id(a) == id(b)
False

>>> b[0].append(5)
>>>
>>> b
[[1, 2, 5], [3, 4]]
>>> a
[[1, 2, 5], [3, 4]]      # changes reflected in original list also

```
As you see in the above example while we are modifying the internal list elements in the new list, it’s getting updated in the original list also, because `a[0]` and `b[0]` are still pointing to the same memory address(original list).
```python
>>> print('a[0] - {} , b[0] - {}'.format(id(a[0]), id(b[0])))       
a[0] - 140399422977280 , b[0] - 140399422977280
>>>

>>> id(a[0]) == id(b[0])
True
>>> id(a[1]) == id(b[1])
True
```
So the new list `b` has its own memory address but its elements do not.  because in shallow copy instead of copying the list's elements to the new object, It simply copies the references to their memory addresses, Therefore while we are making changes to the original object it's reflecting in copied objects and vice versa.

This is a characteristic of shallow copy.

## **Deep copy**
>A deep copy constructs a new compound object and then, recursively, inserts copies into it of the objects found in the original.    

Creating a deep copy is slower because you are making new copies for everything. In this rather than just copy the address of the compound objects, It Simply makes a full copy of all the list's elements (simple and compound objects) of the original list and allocates different memory address for the new list and then assigns them the copied elements.

To achieve the deep copy we have to import the `copy` module. And use `copy.deepcopy()`.
```python
>>> import copy
>>> a = [[1, 2, 3], [4, 5, 6]]  
>>> b = copy.deepcopy(a)                     

>>> id(a) == id(b)
False

>>> id(a[0]) == id(b[0])      # memory address is different      
False

>>> a[0].append(8)            # modify the list                                                                                                                                            


>>> a
[[1, 2, 3, 8], [4, 5, 6]]
>>> b
[[1, 2, 3], [4, 5, 6]]        # New list’s elements didn’t get affected
```
As you see above the original list does not get affected.

>The difference between shallow and deep copying is only relevant for compound objects (objects that contain other objects, like lists or class instances).

Hope you like the explanation of Assignment Operator, Shallow Copy and Deep Copy. Still, if any doubt or improvement regarding Copy in Python, ask in the comment section.

References:
https://docs.python.org/2/library/copy.html
