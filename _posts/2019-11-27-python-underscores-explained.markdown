---
title: Python Underscores Explained
date: 2019-11-27 23:12:00 +05:30
categories:
- blog
tags:
- python
- productivity
- beginners
- tutorial
category: blog
---

Single and double underscores have a meaning in Python variable and method names. In this article, we’ll discuss the five underscore patterns and how they affect the behavior of Python programs. Understanding these concepts will help a lot especially when writing advanced code.


- Single Leading Underscore: `_var`
- Single Trailing Underscore: `var_`
- Double Leading Underscore: `__var`
- Double Leading and Trailing Underscore: `__var__`
- Single Underscore: `_`


#### **Single Leading Underscore**: `_var`

The underscore prefix before the variable/method name indicates to a programmer that It is for internal use only. Here name prefix by an underscore is treated as non-public.
Python does not have a strong note between private and public variables/function like Java. It’s just like a programmer put up an underscore warning sign that verbalizes:

> Hey, this isn’t meant to be a part of the public interface of this class. Best to leave it alone.

Let's look at the below example - 

```python
class Person:
    def __init__(self):
        self.name = 'Prashant'
        self._age = 26
```
Let's try to access the `name` and `_age` -
```python
>>> p = Person()
>>> p.name
'Prashant'
>>> p._age
26
```
So, a single underscore prefix in Python does not impose any restrictions on accessing a variable.


#### **Single Trailing Underscore**: `var_`

Python has some default keywords which we can not use as the variable name. To avoid such conflict with Python keywords or built-ins. we use underscore after the name.
Let's look at the below example -
```python
def method(name, class='Classname'):
                     ^
SyntaxError: invalid syntax
```
`SyntaxError` because `class` is a keyword in python, So here we can put `_` after the `class` - 
```python
def method(name, class_='Classname'):
    pass
```
Single Trailing Underscore allows us to use something that would otherwise be a Python keyword. like - 
```python
As_
With_
For_
In_
```


#### **Double Leading Underscore(Name Mangling)**: `__var`

A Double Leading Underscore causes the Python interpreter to rewrite the attribute name to avoid conflicts of attribute names between classes. The interpreter replaces the double prefix underscore name with `_Employee__id`(as per the below example) as a way to ensure that the name will not overlap with a similar name in subclasses.

For better understanding, look at below example - 
```python
class Employee:
    def __init__(self):
        self.name = 'Prashant'
        self._age = 26
        self.__id = 11
```
Let's try to access the `name`, `_age` and `__id` -

```python
>>> p = Employee()
>>> p.name
'Prashant'
>>> p._age
26
>>> p.__id
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Employee' object has no attribute '__id'
```
Why did we get that `AttributeError` here when accessing `__id`. because `Name mangling` turns out `__id` object. Let’s get the attributes list using built-in `dir()`.
```python
>>> dir(p)
['_Employee__id', '__class__', ..........., '_age', 'name']

>>> p._Employee__id
11

```
As you can see `__id` is mangled to `_Employee__id`. This is the **Name Mangling** that has been applied by the Python interpreter. It avoids the variable from getting overridden in subclasses.
Now let’s define a subclass and define the same variable name as we took in Employee.
```python
class Employer(Employee):
    def __init__(self):
        Employee.__init__(self)        
        self.__id = 25
```
here we created a subclass(`Employer`) of `Employee`
```python
>>> emp = Employer()
>>> dir(emp)
['_Employee__id', '_Employer__id', '__class__',.........,'_age', 'name']
```
we can’t easily override Employee’s __id variable.
```python
>>> emp.__id
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Employer' object has no attribute '__id'
>>> emp._Employee__id
11
>>> emp._Employer__id
25
```
its actual meaning is just to name mangle to prevent accidental access.


#### **Double Leading and Trailing Underscore**: `__var__`

Names surrounded by a double underscore prefix and postfix are called as **Magic methods** or **dunder**`("Double UNDERscore")`.
Like - `__init__` method for object constructors, or `__call__` method to make object callable. 
```python
class Magic():
    def __init__(self):
        self.__num__ = 11

>>> obj = Magic()
>>> obj.__num__
11
```
Built-in classes in Python define many Magic methods. You can use the `dir()` function to see the number of magic methods inherited by a class.
For example, the following lists contain all the attributes and methods defined in the **int** class.
```python
dir(int)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
``` 
As you can see above **int** class include various magic methods. Let’s take a example -
```python
>>> num=10
>>> num+5
15
>>> num.__add__(5)
15
```


#### **Single Underscore**: `_`

Per convention, a single standalone underscore is sometimes used as a name to indicate that a variable is temporary or insignificant.
For example, in the following loop, we don’t need access to the running index and we can use `“_”` to indicate that it is just a temporary value:
```python
for _ in range(2):
    print('Hey there...')

Hey there...
Hey there...
```
Again, this meaning is `per convention` only and there’s no special behavior triggered in the Python interpreter.
Apart from use as a temporary variable, The underscore is returning the last value that was evaluated by the interpreter. This means you can access some computation after the execution and later store it into a variable and use it for something else:
```python
>>> 10+20
30
>>> _
30
>>> _+10
40
```


I hope that you now have understood how and where to use which pattern of underscore in our naming convention.

If you have any suggestions on your mind, please let me know in the mail.