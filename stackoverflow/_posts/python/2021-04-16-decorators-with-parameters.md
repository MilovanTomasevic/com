---
layout: post
title: Decorators with parameters?
image: https://www.worldofitech.com/wp-content/uploads/2020/06/python_1-3.png
description: >
  I have a problem with the transfer of variable 'insurance_mode' by the decorator. I would do it by the following decorator statement
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-16
categories: [stackoverflow]
tags: [python, decorator]
---

Decorators with parameters?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/5929107/decorators-with-parameters) by [falek.marcin](https://stackoverflow.com/users/611982/falek-marcin)

I have a problem with the transfer of variable 'insurance_mode' by the decorator. I would do it by the following decorator statement:

```py
@execute_complete_reservation(True)
def test_booking_gta_object(self):
    self.test_select_gta_object()
```
but unfortunately, this statement does not work. Perhaps maybe there is better way to solve this problem.
```py
def execute_complete_reservation(test_case,insurance_mode):
    def inner_function(self,*args,**kwargs):
        self.test_create_qsf_query()
        test_case(self,*args,**kwargs)
        self.test_select_room_option()
        if insurance_mode:
            self.test_accept_insurance_crosseling()
        else:
            self.test_decline_insurance_crosseling()
        self.test_configure_pax_details()
        self.test_configure_payer_details

    return inner_function
```

## [Answer](https://stackoverflow.com/a/5929165/13155046) by [t.dubrownik](https://stackoverflow.com/users/679897/t-dubrownik)

The syntax for decorators with arguments is a bit different - the decorator with arguments should return a function that will *take a function* and return another function. So it should really return a normal decorator. A bit confusing, right? What I mean is:

```py
def decorator_factory(argument):
    def decorator(function):
        def wrapper(*args, **kwargs):
            funny_stuff()
            something_with_argument(argument)
            result = function(*args, **kwargs)
            more_funny_stuff()
            return result
        return wrapper
    return decorator
```
[Here][1] you can read more on the subject - it's also possible to implement this using callable objects and that is also explained there.


  [1]: https://www.artima.com/weblogs/viewpost.jsp?thread=240845#decorator-functions-with-decorator-arguments


## [Answer](https://stackoverflow.com/a/25827070/13155046) by [srj](https://stackoverflow.com/users/1274195/srj)

**Edit** : for an in-depth understanding of the mental model of decorators, take a look at [this](https://youtu.be/MjHpMCIvwsY) awesome Pycon Talk. well worth the 30 minutes.

One way of thinking about decorators with arguments is
```py
@decorator
def foo(*args, **kwargs):
    pass
```
translates to
```py
foo = decorator(foo)
```
So if the decorator had arguments,
```py
@decorator_with_args(arg)
def foo(*args, **kwargs):
    pass
```
translates to
```py
foo = decorator_with_args(arg)(foo)
```
`decorator_with_args` is a function which accepts a custom argument and which returns the actual decorator (that will be applied to the decorated function).

I use a simple trick with partials to make my decorators easy
```py
from functools import partial

def _pseudo_decor(fun, argument):
    def ret_fun(*args, **kwargs):
        #do stuff here, for eg.
        print ("decorator arg is %s" % str(argument))
        return fun(*args, **kwargs)
    return ret_fun

real_decorator = partial(_pseudo_decor, argument=arg)

@real_decorator
def foo(*args, **kwargs):
    pass
```
*Update:*

Above, `foo` becomes `real_decorator(foo)`

One effect of decorating a function is that the name `foo` is overridden upon decorator declaration. `foo` is "overridden" by whatever is returned by `real_decorator`. In this case, a new function object. 

All of `foo`'s metadata is overridden, notably docstring and function name.
```py
>>> print(foo)
<function _pseudo_decor.<locals>.ret_fun at 0x10666a2f0>
```
[functools.wraps][1] gives us a convenient method to "lift" the docstring and name to the returned function.
```py
from functools import partial, wraps

def _pseudo_decor(fun, argument):
    # magic sauce to lift the name and doc of the function
    @wraps(fun)
    def ret_fun(*args, **kwargs):
        # pre function execution stuff here, for eg.
        print("decorator argument is %s" % str(argument))
        returned_value =  fun(*args, **kwargs)
        # post execution stuff here, for eg.
        print("returned value is %s" % returned_value)
        return returned_value

    return ret_fun

real_decorator1 = partial(_pseudo_decor, argument="some_arg")
real_decorator2 = partial(_pseudo_decor, argument="some_other_arg")

@real_decorator1
def bar(*args, **kwargs):
    pass

>>> print(bar)
<function __main__.bar(*args, **kwargs)>

>>> bar(1,2,3, k="v", x="z")
decorator argument is some_arg
returned value is None
```

  [1]: https://docs.python.org/2/library/functools.html#functools.wraps


## [Answer](https://stackoverflow.com/a/65655346/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

[![enter image description here][1]][1]

- Here we ran display info twice with two different names and two different ages. 
- Now every time we ran display info, our decorators also added the functionality of printing out a line before and a line after that wrapped function.

```py
def decorator_function(original_function):
    def wrapper_function(*args, **kwargs):
        print('Executed Before', original_function.__name__)
        result = original_function(*args, **kwargs)
        print('Executed After', original_function.__name__, '\n')
        return result
    return wrapper_function


@decorator_function
def display_info(name, age):
    print('display_info ran with arguments ({}, {})'.format(name, age))


display_info('Mr Bean', 66)
display_info('MC Jordan', 57)
```

output:
```sh
Executed Before display_info
display_info ran with arguments (Mr Bean, 66)
Executed After display_info 

Executed Before display_info
display_info ran with arguments (MC Jordan, 57)
Executed After display_info 
```

- So now let's go ahead and get our decorator function to accept arguments. 
- For example let's say that I wanted a customizable prefix to all of these print statements within the wrapper.
- Now this would be a good candidate for an argument to the decorator. 

- The argument that we pass in will be that prefix. Now in order to do, this we're just going to add another outer layer to our decorator, so I'm going to call this a function a prefix decorator.

```py
def prefix_decorator(prefix):
    def decorator_function(original_function):
        def wrapper_function(*args, **kwargs):
            print(prefix, 'Executed Before', original_function.__name__)
            result = original_function(*args, **kwargs)
            print(prefix, 'Executed After', original_function.__name__, '\n')
            return result
        return wrapper_function
    return decorator_function


@prefix_decorator('LOG:')
def display_info(name, age):
    print('display_info ran with arguments ({}, {})'.format(name, age))


display_info('Mr Bean', 66)
display_info('MC Jordan', 57)
```

output:
```py
LOG: Executed Before display_info
display_info ran with arguments (Mr Bean, 66)
LOG: Executed After display_info 

LOG: Executed Before display_info
display_info ran with arguments (MC Jordan, 57)
LOG: Executed After display_info 
```
- Now we have that `LOG:` prefix before our print statements in our wrapper function and you can change this any time that you want.


  [1]: https://i.stack.imgur.com/lGObx.gif