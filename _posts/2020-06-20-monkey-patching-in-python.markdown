---
title: Monkey Patching In Python
date: 2020-06-20 04:15:00 +05:30
categories:
- python
- tutorial
- beginners
- productivity
- datascience
tags:
- python
- tutorial
- beginners
- productivity
- datascience
category: blog
---

# What is Monkey patching
In Python, the term monkey patch only refers to dynamic modifications of a class or module at runtime, which means monkey patch is a piece of Python code that extends or modifies other code at runtime.

Monkey patching can only be done in dynamic languages, of which python is a good example. In Monkey patching, we reopen the existing classes or methods in class at runtime and alters the behavior, which should be used cautiously, or you should use it only when you really need to. As Python is a dynamic programming language, Classes are mutable so you can reopen them and modify or even replace them.

It is often used to replace or extends a method on the module or class level with a custom implementation.

Let's Implementation monkey patching -

```python
class MonkeyPatch:
    def __init__(self, num):
        self.num = num

    def addition(self, other):
        return (self.num + other)
    
obj = MonkeyPatch(10)
obj.addition(20)
```
Output - 
```python
30
```
As you can see below now only two methods `__init__` and `addition` and are available for the above class object. (You can use `dir(obj)` also to get the members of the object)

```python
import inspect 

inspect.getmembers(obj, predicate=inspect.ismethod)                                                                                                                                       
```
Output - 
```python
[('__init__',
  <bound method MonkeyPatch.__init__ of <__main__.MonkeyPatch object at 0x7f32495d7c50>>),
 ('addition',
  <bound method MonkeyPatch.addition of <__main__.MonkeyPatch object at 0x7f32495d7c50>>)]
```
In the above piece code, we have already defined a `MonkeyPatch` class with an `addition` method, and later on, we are adding a new method to the `MonkeyPatch` class. Suppose the new method is as follows.
```python
def subtraction(self, num2):
    return self.num - num2
```
Now how we add the above method to the `MonkeyPatch` class, simply just place `subtraction` function into `MonkeyPatch` class with an assignment statement, as below -
```python
MonkeyPatch.subtraction = subtraction
```
the newly created `subtraction` function would be available for all existing instances as well as a new instance also, Let's do some practical now - 
```python
import inspect

inspect.getmembers(obj, predicate=inspect.ismethod)
```
Output - 
```python
[('__init__',
  <bound method MonkeyPatch.__init__ of <__main__.MonkeyPatch object at 0x7f28186b8c88>>),
 ('addition',
  <bound method MonkeyPatch.addition of <__main__.MonkeyPatch object at 0x7f28186b8c88>>),
 ('subtraction',
  <bound method subtraction of <__main__.MonkeyPatch object at 0x7f28186b8c88>>)]
```
Did you notice for the existing object `obj` which we have defined earlier contained the newly created function, let's check it out if the newly created function will work for the existing instance -
```python
>>> obj.subtraction(1)               # Working as expected
9
>>> obj_1 = MonkeyPatch(10)          # create some new object
>>> obj_1.subtraction(2)
8
```
`obj.subtraction` work as expected, but Keep in mind if a method already exists with the same name then this will change the behavior of the existing method.

# Things to keep in mind
The best thing is not to monkey patch. You can define child classes for the ones you want to alter. Still, if monkey patch needed then follow these rules -

1. Use if you have a really good reason(like - temporary critical hotfix)
2. Write proper documentation describing the reason for monkey patch
3. Documentation should contain the information about the removal of the monkey patch and what to watch for. Lots of monkey patches are temporary, so they should be easy to remove.
4. Try to make monkey patch as transparent as possible also place monkey patch code in separate files

# Conclusion 
Now we have learned how to do a monkey patch in Python. However, it has its own drawbacks and should be used carefully. Well, this often means the bad architecture of your application, and is not a good design decision, because it creates a discrepancy between the original source code on disk and the observed behavior and can be very confusing when troubleshooting.

# Reference
https://en.wikipedia.org/wiki/Monkey_patch
https://stackoverflow.com/questions/5626193/what-is-monkey-patching