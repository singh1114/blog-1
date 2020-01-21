---
title: Pickling and Unpickling in python Explained
date: 2020-01-22 01:44:00 +05:30
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

Pickling allows you to serialize and de-serializing Python object structures. In short, Pickling is a way to convert a python object into a character stream so that this character stream contains all the information necessary to reconstruct the object in another python script.

To successfully reconstruct the object, The Pickled byte stream contains instructions to the unpicker to reconstruct the original object structure along with instruction operands, which help in populating the object structure.

## **Pickle protocols:** 

There are currently 6 different protocols that can be used for Pickling. The higher the protocol used, the more recent the version of Python needed to read the Pickle produced. To know more about [Pickle Protocol](https://docs.python.org/3/library/pickle.html).

Pickle has two main methods. The first one is a `dump`, which dumps an object to a file object and the second one is `load`, which loads an object from a file object.

```
pickle.dump(object, file_obj, protocol)
```

This function takes three arguments - 

1. Python object to serialize.
2. File object in which the serialized python object must be stored.
3. Protocol (If a protocol is not specified, protocol 0 is used. If the protocol is specified as a negative value or `HIGHEST_PROTOCOL`, the highest protocol version available will be used.)

So let's continue to practical:
* First of all, we have to import it through the following command:
```python
import pickle
```

Below is the example of pickling and unpickling -
### Pickling

```python
import pickle

def pickle_data():
    data = {
                'name': 'Prashant',
                'profession': 'Software Engineer',
                'country': 'India'
        }
    filename = 'PersonalInfo'
    outfile = open(filename, 'wb')
    pickle.dump(data,outfile)
    outfile.close()
    
pickle_data()
```
To Open the file we used `open` function, The first argument should be the name of your file and the second argument is `wb`, `wb` refers the writing in binary mode. This means that the data will be written in the form of byte objects.

### Unpickling

```python
import pickle

def unpickling_data():
    file = open(filename,'rb')
    new_data = pickle.load(file)
    file.close()
    return new_data


print(unpickling_data())
```
Here `r` stands for reading mode and the `b` stands for binary mode. You'll be reading a binary file.
The output of the Unpickling function is the same as we pickled.
```python
{'name': 'Prashant', 'profession': 'Software Engineer', 'country': 'India'}
```
## **What can be pickled?**
The following data types can be pickled:

* Booleans,
* Integers,
* Floats,
* Complex numbers,
* (normal and Unicode) Strings,
* Tuples,
* Lists,
* Sets, and Dictionaries that contain pickable objects.
* Built-in functions defined at the top level of a module
* Classes that are defined at the top level of a module

## **Common UseCase of Pickling**

1. To save a program's state data to disk so that it can carry on where it left off when restarted (persistence)
2. Sending Python data over a TCP connection in a multi-core or distributed system (marshalling)
3. Storing Python objects in a database

## **Dangers of Pickling**

The documentation of the Pickle module states:

>The Pickle module is not secure against erroneous or maliciously constructed data. Never unpickle data received from an untrusted or unauthenticated source.

## **Safe implementation of Pickle**
* The Pickle should never be used between unknown parties.
* Ensure the parties exchanging Pickle have an encrypted network connection.
* In case of unsecured connection, any alteration in Pickle can be verified by using a cryptographic signature. Pickle can be signed before the transmission, and its signature can be verified before loading it on the receiver side.

I hope that you now have a fair understanding of Pickling/Unpickling in python.

If you have any suggestions on your mind, please let me know in the comments.