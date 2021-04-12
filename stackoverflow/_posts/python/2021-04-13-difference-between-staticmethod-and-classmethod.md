---
layout: post
title: Difference between staticmethod and classmethod?
image: https://i.ytimg.com/vi/aLBnKIu_WVU/maxresdefault.jpg
description: >
  What is the difference between a function decorated with `@staticmethod` and one decorated with `@classmethod`?
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-13
categories: [stackoverflow]
tags: [python, oop, methods, python-decorators]
---

Difference between staticmethod and classmethod?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/136097/difference-between-staticmethod-and-classmethod) by [Daryl Spitzer](https://stackoverflow.com/users/4766/daryl-spitzer)

What is the difference between a function decorated with [`@staticmethod`][1] and one decorated with [`@classmethod`][2]?


  [1]: http://docs.python.org/library/functions.html#staticmethod
  [2]: http://docs.python.org/library/functions.html#classmethod


## [Answer](https://stackoverflow.com/a/1669524/13155046) by [Mike T](https://stackoverflow.com/users/327026/mike-t) and [ubuntu](https://stackoverflow.com/users/190597/unutbu)

Maybe a bit of example code will help: Notice the difference in the call signatures of `foo`, `class_foo` and `static_foo`:

```py
class A(object):
    def foo(self, x):
        print(f"executing foo({self}, {x})")

    @classmethod
    def class_foo(cls, x):
        print(f"executing class_foo({cls}, {x})")

    @staticmethod
    def static_foo(x):
        print(f"executing static_foo({x})")

a = A()
```

Below is the usual way an object instance calls a method. The object instance, `a`, is implicitly passed as the first argument.

```py
a.foo(1)
# executing foo(<__main__.A object at 0xb7dbef0c>, 1)
```

----------

   
**With classmethods**, the class of the object instance is implicitly passed as the first argument instead of `self`.

```py  
a.class_foo(1)
# executing class_foo(<class '__main__.A'>, 1)
```

You can also call `class_foo` using the class. In fact, if you define something to be
a classmethod, it is probably because you intend to call it from the class rather than from a class instance. `A.foo(1)` would have raised a TypeError, but `A.class_foo(1)` works just fine:

```py      
A.class_foo(1)
# executing class_foo(<class '__main__.A'>, 1)
```

One use people have found for class methods is to create [inheritable alternative constructors](https://stackoverflow.com/a/1950927/190597).


----------


**With staticmethods**, neither `self` (the object instance) nor  `cls` (the class) is implicitly passed as the first argument. They behave like plain functions except that you can call them from an instance or the class:

```py 
a.static_foo(1)
# executing static_foo(1)

A.static_foo('hi')
# executing static_foo(hi)
```

Staticmethods are used to group functions which have some logical connection with a class to the class.

----------


`foo` is just a function, but when you call `a.foo` you don't just get the function,
you get a "partially applied" version of the function with the object instance `a` bound as the first argument to the function. `foo` expects 2 arguments, while `a.foo` only expects 1 argument.

`a` is bound to `foo`. That is what is meant by the term "bound" below:

```py
print(a.foo)
# <bound method A.foo of <__main__.A object at 0xb7d52f0c>>
```

With `a.class_foo`, `a` is not bound to `class_foo`, rather the class `A` is bound to `class_foo`.

```py
print(a.class_foo)
# <bound method type.class_foo of <class '__main__.A'>>
```

Here, with a staticmethod, even though it is a method, `a.static_foo` just returns
a good 'ole function with no arguments bound. `static_foo` expects 1 argument, and
`a.static_foo` expects 1 argument too.

```py
print(a.static_foo)
# <function static_foo at 0xb7d479cc>
```

And of course the same thing happens when you call `static_foo` with the class `A` instead.

```py
print(A.static_foo)
# <function static_foo at 0xb7d479cc>
```

## [Answer](https://stackoverflow.com/a/136138/13155046) by [Brian Burns](https://stackoverflow.com/users/243392/brian-burns) and [Thomas Wouters](https://stackoverflow.com/users/17624/thomas-wouters)

A **staticmethod** is a method that knows nothing about the class or instance it was called on. It just gets the arguments that were passed, no implicit first argument. It is basically useless in Python -- you can just use a module function instead of a staticmethod.

A **classmethod**, on the other hand, is a method that gets passed the class it was called on, or the class of the instance it was called on, as first argument. This is useful when you want the method to be a factory for the class: since it gets the actual class it was called on as first argument, you can always instantiate the right class, even when subclasses are involved. Observe for instance how `dict.fromkeys()`, a classmethod, returns an instance of the subclass when called on a subclass:

```py
>>> class DictSubclass(dict):
...     def __repr__(self):
...         return "DictSubclass"
... 
>>> dict.fromkeys("abc")
{'a': None, 'c': None, 'b': None}
>>> DictSubclass.fromkeys("abc")
DictSubclass
>>>
```

## [Answer](https://stackoverflow.com/a/64732009/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)


__*Instance Method*__:

`+` *Can* modify object instance state

`+` *Can* modify class state


__*Class Method*__:

`-` *Can't* modify object instance state

`+` *Can* modify class state

__*Static Method*__:

`-` *Can't* modify object instance state

`-` *Can't* modify class state


```py
class MyClass:
    ''' 
    Instance method has a mandatory first attribute self which represent the instance itself. 
    Instance method must be called by a instantiated instance.
    '''
    def method(self):
        return 'instance method called', self
    
    '''
    Class method has a mandatory first attribute cls which represent the class itself. 
    Class method can be called by an instance or by the class directly. 
    Its most common using scenario is to define a factory method.
    '''
    @classmethod
    def class_method(cls):
        return 'class method called', cls
    
    '''
    Static method doesn’t have any attributes of instances or the class. 
    It also can be called by an instance or by the class directly. 
    Its most common using scenario is to define some helper or utility functions which are closely relative to the class.
    '''
    @staticmethod
    def static_method():
        return 'static method called'


obj = MyClass()
print(obj.method())
print(obj.class_method()) # MyClass.class_method()
print(obj.static_method()) # MyClass.static_method()
```

output:
```sh
('instance method called', <__main__.MyClass object at 0x100fb3940>)
('class method called', <class '__main__.MyClass'>)
static method called
```

The instance method we actually had access to the object instance , right so this was an instance off a my class object whereas with the class method we have access to the class itself. But not to any of the objects,  because the class method doesn't really care about an object existing. However you can both call a class method and static method on an object instance. This is going to work it doesn't really make a difference, so again when you call static method here it's going to work and it's going to know which method you want to call. 

The Static methods are used to do some utility tasks, and class methods are used for factory methods. The factory methods can return class objects for different use cases.

And finally, a short example for better understanding:

```py
class Student:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

    @classmethod
    def get_from_string(cls, name_string: str):
        first_name, last_name = name_string.split()
        if Student.validate_name(first_name) and Student.validate_name(last_name):
            return cls(first_name, last_name)
        else:
            print('Invalid Names')

    @staticmethod
    def validate_name(name):
        return len(name) <= 10


stackoverflow_student = Student.get_from_string('Name Surname')
print(stackoverflow_student.first_name) # Name
print(stackoverflow_student.last_name) # Surname
```
