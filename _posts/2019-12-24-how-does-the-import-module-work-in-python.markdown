---
title: How does the import module work in Python?
date: 2019-12-24 01:52:00 +05:30
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

The objective of writing this article is to develop a better understanding of how the `import` statement works and different forms of importing.

In Python, Whenever you want to import a module, you write as following -
	
```
import module_name
```

When you call `import` in the Python, the interpreter searches the module through a set of directories for the name provided and runs all of the code in the module file. The list of directories that it searches is stored in `sys.path` and can be modified during run-time.

Python’s documentation about `sys.path` as -

>A list of strings that specifies the search path for modules. Initialized from the environment variable `PYTHONPATH`, plus an installation-dependent default.

Now let's move to the practical, I have created a module file `dev.py` and written a few lines of code-

```python
language = 'Python'
framework = 'Django'


def hello_world():
    print('Hey !!! welcome back')


class ModuleTest:
    def get_language(self):
        print('My favroite language is - {}'.format(language))
```

In the below example I am accessing the dev.py module by importing it in the current working directory path, as below - 

```python
>>> import dev
>>> dev.language
'Python'

>>> dev.framework
'Django'

>>> dev.hello_world()
Hey !!! welcome back

>>> obj = ModuleTest()
>>> obj.get_language()
My favroite language is - Python

```

### How import works

As I told earlier when interpreter execute `import dev` statement, it searches sequentially in specific locations until it finds it - 	

**01**. It looks in the current directory where the input script was run.
**02**. PYTHONPATH - If `PYTHONPATH` is set, then Python will include the directories in `sys.path` for searching. You can set the: `PYTHONPATH` as below - 
```
export PYTHONPATH='/some/extra/path'
```
and check like as below-
```
python -c "import sys; print(sys.path)"

o/p - ['', '/usr/lib/python36.zip',
'/usr/lib/python3.6', '/usr/lib/python3.6/lib-dynload', '/home/prashant/.local/lib/python3.6/site-packages', '/usr/local/lib/python3.6/dist-packages', '/usr/lib/python3/dist-packages']
```
**03**. An installation-dependent list of directories configured at the time Python is installed.

Additionally, we can put the module file in any of the directory and then modify `sys.path` at run-time so that it contains that directory. For example, 
I can put my module `dev.py` in directory `/home/prashant/Desktop` and then update as following:

```python
>>> sys.path.append('/home/prashant/Desktop')
>>> sys.path
['', '/usr/lib/python36.zip', '/usr/lib/python3.6', '/usr/lib/python3.6/lib-dynload', '/home/prashant/.local/lib/python3.6/site-packages',
'/usr/local/lib/python3.6/dist-packages',
'/usr/lib/python3/dist-packages', '/home/prashant/Desktop']
>>> 
>>> import dev
>>> dev.language
'Python'
```

### If a module is imported twice?
The module is only loaded the first time the import statement is executed and there is no performance loss by importing it again. You can examine sys.modules to find out which modules have already been loaded.

```python
>>> import sys
>>> sys.modules.keys()
dict_keys(['builtins', 'sys', '_frozen_importlib', '_imp', '_warnings', '_thread', '_weakref', '_frozen_importlib_external', '_io', 'marshal', 'posix', 'zipimport', 'encodings', 'codecs', '_codecs', 'encodings.aliases', 'encodings.utf_8', '_signal', '__main__', 'encodings.latin_1', 'io', 'abc', '_weakrefset', 'site', 'os', 'errno', 'stat', '_stat', 'posixpath', 'genericpath', 'os.path', '_collections_abc', '_sitebuiltins', 'sysconfig', '_sysconfigdata_m_linux_x86_64-linux-gnu', '_bootlocale', '_locale', 'types', 'functools', '_functools', 'collections', 'operator', '_operator', 'keyword', 'heapq', '_heapq', 'itertools', 'reprlib', '_collections', 'weakref', 'collections.abc', 'importlib', 'importlib._bootstrap', 'importlib._bootstrap_external', 'warnings', 'importlib.util', 'importlib.abc', 'importlib.machinery', 'contextlib', 'backports', 'zope', 'sitecustomize', 'apport_python_hook', 'readline', 'atexit', 'rlcompleter', 'dev'])
>>> 
>>> sys.modules['dev']
<module 'dev' from '/home/prashant/Desktop/dev.py'>
>>>
```

If intentionally you want it to be loaded/parsed again, you'd have to [reload()](https://docs.python.org/3/library/importlib.html#importlib.reload) the module.

```python
For Python2.x
reload(module)

For above 2.x and <=Python3.3
import imp
imp.reload(module)

For >=Python3.4
import importlib
importlib.reload(module)
```

### Different forms of the import statement

### 1. **import `<module_name>`**
We can import the module using import statement and access the variables, methods, and classes inside it using the dot operator.

```python
>>> import dev
>>> dev.language
'Python'
```

### 2. **from `<module_name>` import `<name>`**
We can also import the specific variable, methods, classes from the module without importing it as a whole as below- 

```python
>>> from dev import language
>>> language
'Python'
```

While we are using this form of import if any objects that already exists with the same name will be overwritten -

```python
>>> language='java'
>>> from dev import language
>>> language
'Python'
```

Because this form of import overwritten the object id into memory:
As you can differ in the below example - 

```python
>>> language='java'
>>> id(language)
139635543372736
>>> from dev import language
>>> id(language)
139635542592128
```
### 3. **from `<module_name>` import `<name>` as `<new_name>`**
We can also import a specific object from module with an alternative names, This can avoid conflict with previously existing names:
```python
>>> language='java'
>>> from dev import language as lang
>>> lang
'Python'
```

### 4. **import `<module_name>` as `<new_name>`**
We can also import the entire module with an alternate name:
```python
>>> import dev as dev2
>>> dev2.language
'Python'
```

### 5. **from `<module_name>` import `*`**
We can import everything from a module as following-

```python
from dev import *
```

However, this is a highly discouraged practice. Since you are importing modules without control, some functions may get overwritten.

I hope that you now have a fair understanding of the How does the import module work in Python.

If you have any suggestions on your mind, please let me know in the comments.
