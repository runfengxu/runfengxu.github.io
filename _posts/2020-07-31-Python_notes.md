---
layout:     post
title:      Python Notes
subtitle:   
date:       2020-07-14
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - Python
---

https://docs.python-guide.org/dev/virtualenvs/
## Python Virtual Environment

### virtual environment
`$ pip install virtualenv`

test your installation

`$ virtualenv --version`

>This does a user installation to prevent breaking any system-wide packages. If pipenv isn’t available in your shell after installation, you’ll need to add the user base’s binary directory to your PATH.
- **On Linux and macOS** you can find the user base binary directory by running python -m site --user-base and adding bin to the end. For example, this will typically print ~/.local (with ~ expanded to the absolute path to your home directory) so you’ll need to add ~/.local/bin to your PATH. You can set your PATH permanently by modifying ~/.profile.
- **On Windows** you can find the user base binary directory by running py -m site --user-site and replacing site-packages with Scripts. For example, this could return C:\Users\Username\AppData\Roaming\Python36\site-packages so you would need to set your PATH to include C:\Users\Username\AppData\Roaming\Python36\Scripts. You can set your user PATH permanently in the Control Panel. You may need to log out for the PATH changes to take effect.

create virutalenv

`$ virtual my_name`

Now after creating virtual environment, you need to activate it. Remember to activate the relevant virtual environment every time you work on the project. This can be done using the following command:

`$ source virtualenv_name/bin/activate`

then you can install whatever packages you need into this virtual env. And the pakcage will be isolated from the complete system. and once you done you can deactivate.

`deactivate`

### pipenv  : project level virtual environment

`$ pip install --user pipenv`

INstall packages for your project

`$ cd project_folder`

`$ pipenv install requests`

Pipenv will install the library and create a Pipfile for you in your5 project's directory. The pipfile is used to track which dependencies your project needs 



## Multiprocess
Goal of this program is to download img using urls in a binary json file. Our CPU has 8 cores, We want to deploy all of that to download as fast as possible.

```python
#import pandas as pd
import gzip
import urllib.request
from multiprocessing import Pool
import os
import json

class download():
    def __init__(self,path):
        self.path = path

    def parse(self,size=1024*1024):
        g = gzip.open(self.path, 'rb')
        fileEnd = os.path.getsize(self.path)
        chunkEnd = g.tell()
        while True:
            chunkStart = chunkEnd
            g.seek(size,1)
            g.readline()
            chunkEnd = g.tell()

            yield int(chunkStart),int(chunkEnd-chunkStart)
            if chunkEnd>fileEnd:
                break

    def get_img(self,a):
        try:
            if (('asin' in a) and ('imUrl' in a)):
                asin = a['asin']
                url = a['imUrl']
                urllib.request.urlretrieve(url, 'E:/553/553project/dataset/img/'+asin+".jpg")
                return 0
        except:
            return 1
    def process_wrapper(self,chunkStart,chunkSize):
        print('start chunk ',chunkStart)
        with gzip.open(self.path) as f:
            f.seek(chunkStart)
            lines = f.read(chunkSize).splitlines()
            for line in lines:
                line = json.dumps(eval(line))
                line = json.loads(line)
                self.get_img(line)

    def run_parallel(self,processes=8):
        processes = int(processes)
        pool = Pool(processes)
        # try:
        print(1)
        jobs =[]
        for chunkStart,chunkSize in self.parse():
            jobs.append(pool.apply_async(self.process_wrapper,args=(chunkStart,chunkSize)))
        for job in jobs:
            job.get()
            pool.close()

        # except Exception as e:
        #     print(e)
       
if __name__ =='__main__':
    case = download('E:/553/553project/dataset/meta_Clothing_Shoes_and_Jewelry.json.gz')
    case.run_parallel(8)

```
<hr>

## @property decorator

> Python programming provides us with a built-in @property decorator which makes usage of getter and setters much easier in Object-Oriented Programming.
### Class Without Getters and Setters

```python
class Celsius:
    def __init__(self, temperature = 0):
        self.temperature = temperature

    def to_fahrenheit(self):
        return (self.temperature * 1.8) + 32

# Create a new object
human = Celsius()

# Set the temperature
human.temperature = 37

# Get the temperature attribute
print(human.temperature)

# Get the to_fahrenheit method
print(human.to_fahrenheit())
```
Whenever we assign or retrieve any object attribute like temperature as shown above, Python searches it in the object's built-in `__dict__` dictionary attribute. Therefore, man.temperature internally becomes `man.__dict__['temperature']`.


### USing Getter and Setters

```python
# Making Getters and Setter methods
class Celsius:
    def __init__(self, temperature=0):
        self.set_temperature(temperature)

    def to_fahrenheit(self):
        return (self.get_temperature() * 1.8) + 32

    # getter method
    def get_temperature(self):
        return self._temperature

    # setter method
    def set_temperature(self, value):
        if value < -273.15:
            raise ValueError("Temperature below -273.15 is not possible.")
        self._temperature = value
```

As we can see, the above method introduces two new `get_temperature()` and `set_temperature()` methods.

Furthermore, temperature was replaced with `_temperature`. An underscore _ at the beginning is used to denote private variables in Python.

However, the bigger problem with the above update is that all the programs that implemented our previous class have to modify their code from obj.temperature to obj.get_temperature() and all expressions like obj.temperature = val to obj.set_temperature(val).

This refactoring can cause problems while dealing with hundreds of thousands of lines of codes.

All in all, our new update was not backwards compatible. This is where @property comes to rescue.

### property Class

```python
# using property class
class Celsius:
    def __init__(self, temperature=0):
        self.temperature = temperature

    def to_fahrenheit(self):
        return (self.temperature * 1.8) + 32

    # getter
    def get_temperature(self):
        print("Getting value...")
        return self._temperature

    # setter
    def set_temperature(self, value):
        print("Setting value...")
        if value < -273.15:
            raise ValueError("Temperature below -273.15 is not possible")
        self._temperature = value

    # creating a property object
    temperature = property(get_temperature, set_temperature)
```

The last line of the code makes a property object temperature. Simply put, property attaches some code (get_temperature and set_temperature) to the member attribute accesses (temperature).

As we can see, any code that retrieves the value of temperature will automatically call get_temperature() instead of a dictionary (__dict__) look-up.

By using property, we can see that no modification is required in the implementation of the value constraint. Thus, our implementation is backward compatible.

### @property Decorator
In Python, `property()` is a built-in function that creates and returns a property object. The syntax of this function is:

`property(fget=None, fset=None, fdel=None, doc=None)`
<hr>

## Python Closures
A function defined inside another function is called a nested function. Nested functions can access variables of the enclosing scope.

In Python, these non-local variables are read-only by default and we must declare them explicitly as non-local (using nonlocal keyword) in order to modify them.

This value in the enclosing scope is remembered even when the variable goes out of scope or the function itself is removed from the current namespace.

### When do we have closures?
- As seen from the above example, we have a closure in Python when a nested function references a value in its enclosing scope.

- The criteria that must be met to create closure in Python are summarized in the following points.

- We must have a nested function (function inside a function).
The nested function must refer to a value defined in the enclosing function.
The enclosing function must return the nested function.

### When to use closures?
So what are closures good for?

Closures can avoid the use of global values and provides some form of data hiding. It can also provide an object oriented solution to the problem.

When there are few methods (one method in most cases) to be implemented in a class, closures can provide an alternate and more elegant solution. But when the number of attributes and methods get larger, it's better to implement a class.

Here is a simple example where a closure might be more preferable than defining a class and making objects. But the preference is all yours.

<hr>

## Python Decorator

In most basic sense, a decorator is a callable that returns a callable.

```python
def make_pretty(func):
    def inner():
        print("I got decorated")
        func()
    return inner


def ordinary():
    print("I am ordinary")
```

this is equivlent to 

```python
@make_pretty
def ordinary():
    print("I am ordinary")
```

### decorating functions with parameters.

```python
def smart_divide(func):
    def inner(a, b):
        print("I am going to divide", a, "and", b)
        if b == 0:
            print("Whoops! cannot divide")
            return

        return func(a, b)
    return inner


@smart_divide
def divide(a, b):
    print(a/b)
```

In Python, this magic is done as function(*args, **kwargs). In this way, args will be the tuple of positional arguments and kwargs will be the dictionary of keyword arguments. An example of such a decorator will be:

```python
def works_for_all(func):
    def inner(*args, **kwargs):
        print("I can decorate any function")
        return func(*args, **kwargs)
    return inner
```